# 🧾 ADR-077: Minimal Viable Project Definition

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

Synergetikum umožňuje:

- vznik Topicov z otázok (ADR-031)
- transformáciu Topic → Project (ADR-078)
- existenciu viacerých vetiev projektu (ADR-024)
- distribúciu projektu v P2P sieti (ADR-023)

V praxi však vzniká problém:

> **Čo je vôbec projekt? Kedy je niečo už projekt a kedy je to ešte len nápad, téma alebo hlučný signál?**

Bez jasnej definície minima projektu hrozia:

| Riziko | Dôsledok |
|--------|----------|
| **Pseudo-projekty** | Topic s jednou odpoveďou sa stane "projektom" |
| **Nekonečný formujúci stav** | Projekt nikdy neopustí "forming" stage |
| **Nízka dôvera** | Používatelia nebudú brať projekty vážne |
| **Filtračný chaos** | Personal AI nevie, čo má ukazovať |
| **Dead projekty** | Projekty bez jediného záväzku zaberajú miesto |

---

## 3. ⚖️ Rozhodnutie

Systém zavádza **Minimal Viable Project (MVPj) definition** – nie ako produktové minimum, ale ako **existenčné minimum projektu**.

> **Projekt existuje v systéme len vtedy, ak spĺňa všetky povinné atribúty MVPj. Inak je to Topic, nápad alebo neúplný objekt.**

---

## 4. 🧠 Definícia Minimal Viable Project (MVPj)

Projekt je v systéme **platný** (môže existovať ako Project Node) **iba ak**:

### 4.1 Povinné atribúty (všetky musia byť splnené)

| Atribút | Typ | Čo to znamená | Príklad |
|---------|-----|---------------|---------|
| **name** | string, 3-100 znakov | Zrozumiteľný názov projektu | "Komunitná záhrada v Petržalke" |
| **problem_statement** | string, min 20 znakov | Čo chceme riešiť | "V Petržalke chýba verejný priestor na komunitné pestovanie" |
| **owner** | node_id (Person) | Konkrétny človek zodpovedný za projekt | "user_miro_001" |
| **created_at** | ISO timestamp | Kedy vznikol | "2026-04-16T10:00:00Z" |
| **lifecycle_stage** | enum | Kde v životnom cykle sa nachádza | "forming" |
| **origin_topic_id** | node_id (Topic) | Z akej diskusie vznikol (alebo null ak manuálny) | "topic_community_garden_001" |

### 4.2 Minimálny obsah (aspoň jeden z nich)

| Obsah | Popis |
|-------|-------|
| **initial_branch** | Aspoň jedna vetva (main branch) |
| **goal_statement** | Čo chceme dosiahnuť (ak nie je v problem_statement) |
| **success_criteria** | Ako vieme, že sme uspeli |

> Projekt nemôže byť prázdny – musí mať aspoň jeden z týchto troch.

### 4.3 Minimálna sociálna podpora (aspoň jeden z nich)

| Podpora | Popis |
|---------|-------|
| **owner + aspoň 1 participant** | Minimálne dvaja ľudia (jeden je owner) |
| **owner + commitment signal** | Owner musí byť ochotný do toho dať energiu |
| **minimálne 2 vetvy** | Alebo jeden branch s aspoň 2 argumentami |

> Projekt nemôže byť "projekt jedného človeka" (to je osobný zámer, nie kolektívny projekt).

---

## 5. 🚫 Čo NIE JE projekt

Systém **nesmie** považovať za projekt:

| Entita | Prečo |
|--------|-------|
| **Topic** | Nemá záväzok, len diskusiu |
| **Nápad bez majiteľa** | Nikto nie je zodpovedný |
| **Otázka** | Je to vstup, nie zámer |
| **Projekt bez participantov** | Iba owner nie je kolektív |
| **Projekt bez problem_statement** | Nevieme, čo rieši |
| **Projekt bez vetvy** | Nemá žiadny smer |

---

## 6. 🔄 Lifecycle stage a MVPj

Projekt prechádza týmito fázami. MVPj je definované **inak pre každú fázu**:

| Stage | MVPj podmienka | Môže byť zobrazený? |
|-------|----------------|---------------------|
| **draft** | Len povinné atribúty (4.1) | Iba ownerovi (súkromný) |
| **forming** | Povinné atribúty + minimálny obsah (4.2) | Áno, ale s označením "formujúci sa" |
| **active** | Povinné atribúty + obsah + sociálna podpora (4.3) | Áno, plnohodnotne |
| **dormant** | Kedysi spĺňal active, už nespĺňa | Áno, ale s označením "neaktívny" |
| **archived** | Nespĺňa ani forming | Iba v histórii |

---

## 7. 📋 Validácia projektu

### 7.1 Pri vytvorení (transition Topic → Project)

Systém **automaticky validuje**:

```python
def is_valid_project(project):
    # Povinné atribúty (4.1)
    if not project.name or len(project.name) < 3:
        return False, "Chýba názov (min 3 znaky)"
    if not project.problem_statement or len(project.problem_statement) < 20:
        return False, "Chýba problem statement (min 20 znakov)"
    if not project.owner:
        return False, "Chýba owner"
    if not project.created_at:
        return False, "Chýba dátum vytvorenia"
    if not project.lifecycle_stage:
        return False, "Chýba lifecycle stage"
    
    # Minimálny obsah (4.2)
    if not (project.initial_branch or project.goal_statement or project.success_criteria):
        return False, "Chýba aspoň jeden z: initial_branch, goal_statement, success_criteria"
    
    # Minimálna sociálna podpora (4.3) – len pre active stage, nie pre forming
    if project.lifecycle_stage == "active":
        if not (len(project.participants) >= 2 or project.commitment_signals >= 1):
            return False, "Nedostatočná sociálna podpora pre active stage"
    
    return True, "Valid"
```

### 7.2 Pri manuálnom vytvorení projektu (nie z Topicu)

Používateľ môže vytvoriť projekt **priamo** (bez Topicu). Vtedy:

- MVPj podmienky sú **prísnejšie** (vyžaduje sa aj minimálny obsah)
- Projekt začína v `draft` stage
- Owner musí explicitne "publikovať" projekt, aby prešiel do `forming`

### 7.3 Periodická revalidácia

Každých **7 dní** systém revaliduje existujúce projekty:

```python
def revalidate_project(project):
    if project.lifecycle_stage == "active":
        if not meets_social_support(project):  # 4.3
            project.lifecycle_stage = "dormant"
            notify_owner("Projekt prešiel do dormant stavu pre nedostatok podpory")
    
    if project.lifecycle_stage == "dormant":
        if not meets_minimal_content(project):  # 4.2
            project.lifecycle_stage = "archived"
            notify_owner("Projekt bol archivovaný")
    
    if project.lifecycle_stage == "forming":
        if meets_social_support(project):  # 4.3
            project.lifecycle_stage = "active"
            notify_participants("Projekt je teraz aktívny!")
```

---

## 8. 📊 Čo sa stane, keď projekt prestane spĺňať MVPj

| Situácia | Systémová reakcia |
|----------|-------------------|
| Chýba problem_statement | Projekt nejde publikovať (ostáva v draft) |
| Owner odíde (zmizne node) | Project prejde do `dormant`, hľadá sa nový owner |
| Len 1 participant (owner) | `active` → `dormant` |
| Žiadna vetva | `forming` → `draft` |
| 30 dní v dormant | `dormant` → `archived` |

---

## 9. 🧠 MVPj vs UX (čo vidí používateľ)

| Stage | Zobrazenie v UI | Môže sa zapojiť? |
|-------|-----------------|------------------|
| **draft** | Iba owner vidí | Nie (len owner) |
| **forming** | "🔨 Formujúci sa projekt" | Áno, ale s varovaním |
| **active** | Normálny projekt | Áno |
| **dormant** | "😴 Neaktívny projekt" | Áno, ale s varovaním |
| **archived** | V histórii, nie v aktívnych | Nie (len view) |

Príklad varovania pre `forming`:

```
⚠️ Tento projekt je ešte vo fáze formovania.
Nemá ešte dostatočnú podporu alebo obsah.
Chceš sa napriek tomu zapojiť?
[Áno] [Nie]
```

---

## 10. 🚫 Čo sa NESMIE stať

| Zakázaná situácia | Prečo |
|-------------------|-------|
| Projekt bez problem_statement | Nevieme, čo rieši |
| Projekt bez ownera | Nikto nie je zodpovedný |
| "Projekt" s 0 vetvami | Nemá žiadny smer |
| Projekt, ktorý nikdy nebol valid | System nesmie vytvoriť invalidný projekt |
| Automatická archivácia bez notifikácie | Používateľ musí vedieť prečo |

---

## 11. 📊 Príklad kompletného validného projektu (MVPj splnený)

```json
{
  "node_id": "proj_community_garden_001",
  "node_type": "Project",
  "name": "Komunitná záhrada v Petržalke",
  "content": {
    "problem_statement": "V Petržalke chýba verejný priestor na komunitné pestovanie a stretávanie sa susedov.",
    "goal_statement": "Vytvoriť funkčnú komunitnú záhradu s 20 záhonmi a spoločným priestorom.",
    "success_criteria": "20 aktívnych záhradkárov, 2 komunitné podujatia za rok",
    "origin_topic_id": "topic_community_garden_001",
    "lifecycle_stage": "active",
    "created_at": "2026-04-16T10:00:00Z"
  },
  "owner": "user_miro_001",
  "participants": ["user_miro_001", "user_jana_001", "user_peter_001"],
  "branch_root_id": "branch_main_001",
  "relations": [
    { "target_id": "topic_community_garden_001", "relation_type": "originated_from" }
  ]
}
```

---

## 12. 📊 Príklad nevalidného projektu (čo systém odmietne)

```json
{
  "node_id": "proj_bad_001",
  "node_type": "Project",
  "name": "Nápad",  // ❌ príliš krátky (min 3 znaky, ale toto je slabé)
  "content": {
    "problem_statement": "",  // ❌ prázdne
    "lifecycle_stage": "active"
  },
  "owner": "user_miro_001",
  "participants": ["user_miro_001"],  // ❌ len owner
  "branch_root_id": null  // ❌ žiadna vetva
}
```

Systém vráti:

```json
{
  "valid": false,
  "errors": [
    "Názov je príliš krátky alebo neinformatívny (min 3 znaky, odporúčame 10+)",
    "Chýba problem_statement (min 20 znakov)",
    "Chýba aspoň jeden z: initial_branch, goal_statement, success_criteria",
    "Pre active stage je potrebný aspoň 1 participant okrem ownera"
  ]
}
```

---

## 13. 🧠 Dôsledky rozhodnutia

### ✅ Pozitívne

| Dôsledok | Popis |
|----------|-------|
| **Čisté dáta** | Každý projekt má minimálnu štruktúru |
| **Žiadne pseudo-projekty** | Topic bez záväzku nemôže byť projekt |
| **Filtrovateľnosť** | Personal AI vie, čo je "skutočný" projekt |
| **Používateľská dôvera** | Projekty vyzerajú seriózne |
| **Automatická údržba** | Dormant a archived projekty sa čistia |

### ❗ Negatívne / Trade-offs

| Dôsledok | Mitigácia |
|----------|-----------|
| **Bariéra vstupu** | Draft stage umožňuje pomalé budovanie |
| **Falošne negatívne** | Manuálny override pre špeciálne prípady |
| **Nároky na UX** | Treba dobre vysvetliť, čo chýba |

---

## 14. 🚫 Alternatívy (zamietnuté)

| Alternatíva | Dôvod zamietnutia |
|-------------|-------------------|
| **Žiadne minimum** | Chaos, pseudo-projekty, strata dôvery |
| **Príliš prísne minimum** | Nikto nevytvorí projekt (napr. vyžadovať 5 participantov) |
| **Iba technické minimum** | Chýba sociálna dimenzia |
| **Manuálna moderácia** | Nekškálovateľné, centralizované |

---

## 15. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-022 | Project Formation – vznik projektu musí spĺňať MVPj |
| ADR-023 | Distributed Project Model – distribuovaný objekt musí byť validný |
| ADR-024 | Branching – aspoň jedna vetva je povinná |
| ADR-026 | Project Roles – owner je povinný |
| ADR-031 | Topic Layer – origin_topic_id je povinný (okrem manuálnych) |
| ADR-078 | Topic → Project Transition – transition vytvára len validné projekty |
| ADR-015 | Participation Filter – filter používa lifecycle_stage |
| ADR-027 | Project View – view zohľadňuje stage |

---

## 16. 📏 Pravidlá implementácie (high-level)

Systém musí:

1. **Pri vytvorení projektu** (transition alebo manuálne) vykonať validáciu podľa MVPj
2. **Odmietnuť** vytvorenie projektu, ktorý nespĺňa MVPj (s výnimkou `draft` stage)
3. **Periodicky revalidovať** existujúce projekty (každých 7 dní)
4. **Automaticky meniť stage** pri nesplnení podmienok (`active → dormant → archived`)
5. **Notifikovať** ownera a participantov pri zmene stage
6. **Nikdy nemažte** projekt (len archivujte)
7. **Umožniť revivnutie** archivovaného projektu (ak znovu spĺňa MVPj)

---

## 17. 🧪 Testovacie scenáre (pre pilot)

| Scenár | Očakávaný výsledok |
|--------|---------------------|
| Vytvoriť projekt s name="A", problem_statement="" | Odmietnuté |
| Vytvoriť projekt s name="Komunitná záhrada", problem_statement="Chýba priestor...", owner="user_001", bez vetvy | Vytvorený v `forming` |
| Pridať vetvu k `forming` projektu | Stále `forming` (chýba social support) |
| Pridať druhého participanta | Stále `forming` (treba explicitne publikovať) |
| Owner publikuje projekt | Prejde do `active` (ak spĺňa 4.3) |
| Po 30 dňoch bez aktivity | `active → dormant` |
| Po 90 dňoch bez aktivity | `dormant → archived` |
| Owner revivne archivovaný projekt | Prejde do `forming` (ak spĺňa 4.2) |

---

## 18. 🔥 Zhrnutie

> **Projekt nie je to, čo nazveme projektom. Projekt je to, čo spĺňa minimálne existenčné podmienky.**

Tento ADR definuje:

- **Povinné atribúty** (name, problem_statement, owner, created_at, lifecycle_stage, origin_topic_id)
- **Minimálny obsah** (branch, goal, alebo success_criteria)
- **Minimálnu sociálnu podporu** (aspoň 2 ľudia alebo commitment)
- **Lifecycle stage a MVPj** (draft, forming, active, dormant, archived)
- **Validáciu a revalidáciu**
- **Čo sa nesmie stať**

---

## 19. 🧭 Finálna veta

> **Bez definície minima nie je projekt nič iné než nápad s dobrým menom. MVPj nie je o dokonalosti – je o tom, aby sme vedeli, kedy už niečo naozaj je projekt a kedy ešte len ním chce byť.**

---

**Dátum:** 2026-04-16  
**Stav:** Proposed  
**Typ:** Domain Definition ADR  
**Verzia:** 1.0