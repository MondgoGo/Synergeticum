Tu je **prepracovaná verzia ADR-077**, zapracovaná podľa tvojho finálneho komentára a upravená tak, aby bola čo najbližšie realite použitia aj budúcej implementácii. Vychádzam z aktuálnej verzie dokumentu , ale mením najmä:

* vstup do projektu pre solo začiatky,
* definíciu commitmentu,
* TTL/decay,
* event-driven revalidáciu,
* hysterézu,
* rozdiel medzi existenčnou validitou a operačnou pripravenosťou.

---

````md
# 🧾 ADR-077 — Minimal Viable Project Definition

## 1. 📌 Status

**Proposed (v2)**

---

## 2. 🎯 Kontext

Synergetikum umožňuje:

- vznik Topicov z otázok (ADR-031)
- transformáciu Topic → Project (ADR-078)
- existenciu viacerých vetiev projektu (ADR-024)
- distribúciu projektu v P2P sieti (ADR-023)

V praxi však vzniká zásadný problém:

> **Čo je vôbec projekt? Kedy je niečo už projekt a kedy je to ešte len nápad, téma alebo hlučný signál?**

Bez jasnej definície minima projektu hrozia:

| Riziko | Dôsledok |
|--------|----------|
| **Pseudo-projekty** | Topic s jednou odpoveďou sa stane "projektom" |
| **Nekonečný formujúci stav** | Projekt nikdy neopustí `forming` |
| **Nízka dôvera** | Používatelia nebudú brať projekty vážne |
| **Filtračný chaos** | Personal AI nevie, čo má ukazovať |
| **Dead projekty** | Projekty bez záväzku alebo aktivity zaberajú miesto |
| **Falošná sociálna podpora** | Staré commitment signály predstierajú, že projekt žije |

---

## 3. ⚖️ Rozhodnutie

Systém zavádza **Minimal Viable Project (MVPj) definition** – nie ako produktové minimum, ale ako **minimálne existenčné podmienky projektu**.

Zároveň sa zavádza rozdiel medzi dvoma úrovňami validity:

### 3.1 Existenčná validita

> **Projekt môže existovať v systéme ako Project Node.**

To znamená:
- môže byť uložený,
- môže mať lifecycle,
- môže byť v stave `draft` alebo `forming`,
- ale ešte nemusí byť pripravený na plné surfacovanie v sieti.

### 3.2 Operačná pripravenosť

> **Projekt je pripravený byť zobrazovaný, odporúčaný a braný ako reálne aktívny kolektívny projekt.**

To znamená:
- môže byť surfaced v UI,
- môže byť odporúčaný ďalším ľuďom,
- môže byť považovaný za `active`.

---

## 4. 🧠 Definícia projektu

> **Projekt nie je to, čo nazveme projektom. Projekt je to, čo spĺňa minimálne existenčné podmienky.**

Projekt môže byť:
- **solo projekt v stave `forming`**
- **kolektívny projekt v stave `active`**

To je zámerné.

Systém nesmie blokovať vznik solo iniciatív, ale zároveň nesmie predstierať, že solo zámer je už kolektívny projekt.

---

## 5. 📋 Existenčné minimum projektu (MVPj)

Projekt je **existenciálne validný**, ak spĺňa tieto podmienky:

### 5.1 Povinné atribúty

| Atribút | Typ | Popis |
|---------|-----|-------|
| `name` | string, 3–100 znakov | Zrozumiteľný názov projektu |
| `problem_statement` | string, min 20 znakov | Jasné pomenovanie problému |
| `owner_id` | node_id (Person) | Konkrétny človek zodpovedný za projekt |
| `created_at` | ISO timestamp | Čas vzniku |
| `lifecycle_stage` | enum | `draft`, `forming`, `active`, `dormant`, `archived` |
| `origin_topic_id` | node_id (Topic) alebo null | Odkiaľ projekt vznikol |

### 5.2 Minimálny obsah

Projekt musí mať **aspoň jednu** z týchto zložiek:

| Obsah | Popis |
|-------|-------|
| `initial_branch_id` | Aspoň jedna počiatočná vetva |
| `goal_statement` | Čo chceme dosiahnuť |
| `success_criteria` | Ako vieme, že sme uspeli |

> Projekt nesmie byť prázdny kontajner.

---

## 6. 👤 Solo projekt vs kolektívny projekt

### 6.1 Solo projekt (`forming`)

Projekt môže byť v stave `forming`, ak:
- má ownera,
- má povinné atribúty,
- má minimálny obsah,
- ale ešte nemá dostatočnú sociálnu podporu.

To znamená:

> **Jeden človek môže začať projekt.**

Toto je dôležité pre realitu:
- väčšina projektov začína ako solo iniciatíva,
- ľudia sa nezaväzujú hneď,
- systém nesmie blokovať tiché začiatky.

### 6.2 Kolektívny projekt (`active`)

Projekt môže byť v stave `active`, iba ak:
- je existenčne validný,
- a zároveň spĺňa podmienky operačnej pripravenosti.

Teda:

> **`forming` môže byť solo. `active` musí byť kolektívny.**

---

## 7. 🤝 Operačná pripravenosť projektu (`active`)

Projekt je **operačne pripravený**, ak spĺňa všetky tieto podmienky:

### 7.1 Sociálna podpora

| Podmienka | Popis |
|-----------|-------|
| `participant_count >= 2` | Aspoň owner + ešte jeden ďalší participant |
| `active_commitment_count >= 1` | Aspoň jeden aktívny commitment signal |
| `last_activity_at` v platnom okne | Projekt nie je mŕtvy |

### 7.2 Minimálny smer

Projekt musí mať aspoň:
- jednu vetvu,
- alebo explicitne definovaný cieľ,
- alebo success criteria.

### 7.3 Owner musí byť živý a validný

- owner node musí existovať,
- musí byť schopný niesť zodpovednosť,
- ak owner zmizne, projekt nemôže ostať dlhodobo `active` bez riešenia.

---

## 8. 🔄 Commitment model

ADR zavádza dva typy commitmentu:

### 8.1 Explicitný commitment

| Typ | Príklad | Váha |
|-----|---------|------|
| explicitný | klik „zapojím sa“ | 1 |

### 8.2 Behaviorálny (implicitný) commitment

| Typ | Príklad | Váha |
|-----|---------|------|
| behaviorálny | 3+ relevantné akcie (komentár, argument, návrh vetvy, úprava projektu) v definovanom okne | 1 |

### 8.3 Pravidlo výpočtu

```text
active_commitment_count =
  explicit_commitments_within_ttl
  + implicit_commitments_within_activity_window
````

---

## 9. ⏳ Commitment TTL a decay

Commitment nesmie platiť navždy.

### 9.1 Explicitný commitment

* má TTL, napr. **30 dní**
* po vypršaní sa prestáva počítať ako aktívny, ak nebol obnovený

### 9.2 Implicitný commitment

* vzniká len pri aktivite
* má kratšie okno, napr. **14 dní**
* ak aktivita prestane, commitment decayne

### 9.3 Dôsledok

> Systém nesmie držať falošnú sociálnu podporu zo starých signálov.

---

## 10. 🔄 Lifecycle a validita

Projekt prechádza týmito stavmi:

| Stage      | Existenčná validita | Operačná pripravenosť | UI správanie                         |
| ---------- | ------------------- | --------------------- | ------------------------------------ |
| `draft`    | áno                 | nie                   | len owner                            |
| `forming`  | áno                 | nie                   | viditeľné s označením „formujúci sa“ |
| `active`   | áno                 | áno                   | plnohodnotný projekt                 |
| `dormant`  | áno                 | nie                   | viditeľné s označením „neaktívny“    |
| `archived` | áno (historicky)    | nie                   | história / archív                    |

### Princíp

* **existenčná validita** hovorí, že projekt smie existovať,
* **operačná pripravenosť** hovorí, že projekt smie byť surfaced ako aktívny.

---

## 11. 📡 Revalidácia: event-driven + fallback timer

### 11.1 Event-driven revalidácia

Po každej relevantnej udalosti sa prehodnotí stav projektu:

Relevantné udalosti:

* nový participant
* nový argument
* nová vetva
* editácia projektu
* explicitný commitment
* strata participantu
* odchod ownera

### 11.2 Fallback timer

Ak projekt nemá žiadnu relevantnú udalosť po definované obdobie, napr. **7 dní**, spustí sa revalidácia.

### Princíp

> Nie „každých 7 dní bez ohľadu na realitu“, ale „po 7 dňoch nečinnosti“.

---

## 12. 🧠 Hysteréza medzi stavmi

Aby projekt neskákal medzi stavmi pri malých výkyvoch, zavádza sa hysteréza.

### Príklad

| Prechod              | Podmienka                                                                                         |
| -------------------- | ------------------------------------------------------------------------------------------------- |
| `forming → active`   | spĺňa operačnú pripravenosť                                                                       |
| `active → dormant`   | 7 dní bez aktivity alebo strata sociálnej podpory                                                 |
| `dormant → active`   | silnejšia podmienka než pôvodná aktivácia, napr. 3 relevantné udalosti v 48h + aktívny commitment |
| `dormant → archived` | dlhodobá nečinnosť, napr. 30 dní                                                                  |

### Princíp

> Návrat do vyššieho stavu musí byť o niečo ťažší než pád do nižšieho.

To stabilizuje:

* lifecycle,
* notifikácie,
* UX.

---

## 13. 🚫 Čo NIE JE projekt

Systém nesmie považovať za projekt:

| Entita                                       | Prečo                                 |
| -------------------------------------------- | ------------------------------------- |
| `Topic`                                      | Je to diskusia, nie akčný objekt      |
| nápad bez ownera                             | Nikto nenesie zodpovednosť            |
| otázka                                       | Je to vstup, nie projekt              |
| prázdny Project node                         | Nemá minimálny obsah                  |
| `active` projekt bez sociálnej podpory       | Predstiera kolektív, ktorý neexistuje |
| projekt bez vetvy / cieľa / success criteria | Nemá smer                             |

---

## 14. 📋 Validácia projektu

### 14.1 Existenčná validácia

```python
def is_existentially_valid(project):
    if not project.name or len(project.name) < 3:
        return False, "Chýba názov"
    if not project.problem_statement or len(project.problem_statement) < 20:
        return False, "Chýba problem statement"
    if not project.owner_id:
        return False, "Chýba owner"
    if not project.created_at:
        return False, "Chýba created_at"
    if not project.lifecycle_stage:
        return False, "Chýba lifecycle_stage"
    if not (
        project.initial_branch_id
        or project.goal_statement
        or project.success_criteria
    ):
        return False, "Chýba minimálny obsah"
    return True, "Valid"
```

### 14.2 Operačná pripravenosť

```python
def is_operationally_ready(project, now):
    if not is_existentially_valid(project)[0]:
        return False, "Nie je existenčne validný"
    if project.participant_count < 2:
        return False, "Chýba druhý participant"
    if project.active_commitment_count(now) < 1:
        return False, "Chýba aktívny commitment"
    if not project.has_direction():
        return False, "Chýba smer projektu"
    return True, "Ready"
```

---

## 15. 🧪 Revalidácia projektu

```python
def revalidate_project(project, now):
    if project.lifecycle_stage == "active":
        if not is_operationally_ready(project, now)[0]:
            project.lifecycle_stage = "dormant"
            notify_owner("Projekt prešiel do dormant stavu")

    elif project.lifecycle_stage == "forming":
        if is_operationally_ready(project, now)[0]:
            project.lifecycle_stage = "active"
            notify_participants("Projekt je teraz aktívny")

    elif project.lifecycle_stage == "dormant":
        if meets_reactivation_hysteresis(project, now):
            project.lifecycle_stage = "active"
            notify_participants("Projekt bol znovu aktivovaný")
        elif exceeds_archive_window(project, now):
            project.lifecycle_stage = "archived"
            notify_owner("Projekt bol archivovaný")
```

---

## 16. 📊 Dôsledky straty podmienok

| Situácia                        | Reakcia systému                                      |
| ------------------------------- | ---------------------------------------------------- |
| owner zmizne                    | `active/forming → dormant`, hľadá sa nový owner      |
| explicitný commitment expiroval | zníži sa social support score                        |
| participant prestal byť aktívny | môže prestať byť počítaný do operačnej pripravenosti |
| žiadna vetva / cieľ / criteria  | `forming → draft` alebo validation failure           |
| dlhodobé ticho                  | `active → dormant`, neskôr `archived`                |

---

## 17. 🧠 MVPj vs UX

| Stage      | Čo používateľ vidí        | Ako sa surfacuje       |
| ---------- | ------------------------- | ---------------------- |
| `draft`    | len owner                 | nesurfacovať ďalej     |
| `forming`  | „🔨 Formujúci sa projekt“ | obmedzene, s varovaním |
| `active`   | normálny projekt          | plne surfacovať        |
| `dormant`  | „😴 Neaktívny projekt“    | znížená priorita       |
| `archived` | história                  | len na požiadanie      |

### Dôležitý princíp

> Nie všetko, čo existuje, sa má zobrazovať ako plnohodnotný projekt.

---

## 18. 🚫 Čo sa NESMIE stať

| Zakázaná situácia                    | Prečo                          |
| ------------------------------------ | ------------------------------ |
| `active` projekt bez 2+ ľudí         | Predstiera kolektívny stav     |
| commitment bez expiry                | Vytvára falošnú podporu        |
| revalidácia len podľa kalendára      | Nerešpektuje realitu           |
| lifecycle bez hysterézy              | Projekt „tancuje“ medzi stavmi |
| draft/forming surfacované ako active | UX klame používateľa           |

---

## 19. 📊 Príklad validného solo projektu (`forming`)

```json
{
  "node_id": "proj_garden_001",
  "node_type": "Project",
  "name": "Komunitná záhrada v Petržalke",
  "content": {
    "problem_statement": "V Petržalke chýba verejný priestor na komunitné pestovanie.",
    "goal_statement": "Overiť záujem a pripraviť prvý návrh záhrady.",
    "origin_topic_id": "topic_garden_001",
    "lifecycle_stage": "forming",
    "created_at": "2026-04-16T10:00:00Z"
  },
  "owner_id": "user_miro_001",
  "participant_ids": ["user_miro_001"],
  "initial_branch_id": "branch_main_001"
}
```

Tento projekt:

* **je existenčne validný**
* **nie je operačne pripravený**
* môže existovať,
* ale ešte sa nesurfacuje ako plnohodnotne aktívny kolektívny projekt.

---

## 20. 📊 Príklad validného kolektívneho projektu (`active`)

```json
{
  "node_id": "proj_garden_001",
  "node_type": "Project",
  "name": "Komunitná záhrada v Petržalke",
  "content": {
    "problem_statement": "V Petržalke chýba verejný priestor na komunitné pestovanie.",
    "goal_statement": "Vytvoriť komunitnú záhradu s 20 záhonmi.",
    "success_criteria": "20 aktívnych členov a 2 podujatia ročne",
    "origin_topic_id": "topic_garden_001",
    "lifecycle_stage": "active",
    "created_at": "2026-04-16T10:00:00Z"
  },
  "owner_id": "user_miro_001",
  "participant_ids": ["user_miro_001", "user_jana_001"],
  "initial_branch_id": "branch_main_001",
  "signals": {
    "explicit_commitments": [
      {
        "user_id": "user_jana_001",
        "created_at": "2026-04-16T11:00:00Z",
        "expires_at": "2026-05-16T11:00:00Z"
      }
    ]
  }
}
```

---

## 21. 🧠 Dôsledky rozhodnutia

### ✅ Pozitíva

| Dôsledok                     | Popis                                     |
| ---------------------------- | ----------------------------------------- |
| **Podpora reality**          | 1 človek môže začať projekt               |
| **Žiadne falošné kolektívy** | `active` vyžaduje reálnu sociálnu oporu   |
| **Čisté dáta**               | Projekty majú minimálnu štruktúru         |
| **Lepší filtering**          | UI a AI rozlišujú existenciu od readiness |
| **Stabilnejší lifecycle**    | hysteréza a decay znižujú chaos           |

### ❗ Negatíva / Trade-offs

| Dôsledok                          | Mitigácia                          |
| --------------------------------- | ---------------------------------- |
| zložitejší lifecycle              | lepšia realita a UX                |
| potreba decay logiky              | nevyhnutné proti falošným signálom |
| vyššie nároky na event processing | riešené infra vrstvou              |

---

## 22. 🚫 Alternatívy (zamietnuté)

| Alternatíva                            | Dôvod zamietnutia             |
| -------------------------------------- | ----------------------------- |
| 2 ľudia ako podmienka už pre `forming` | zabíja solo začiatky          |
| commitment bez TTL                     | vytvára ilúziu podpory        |
| timer-only revalidácia                 | nerešpektuje realitu aktivity |
| bez hysterézy                          | lifecycle je nestabilný       |
| všetko surfacovať rovnako              | UX klame                      |

---

## 23. 🔗 Väzby na ďalšie ADR

| ADR     | Vzťah                                                           |
| ------- | --------------------------------------------------------------- |
| ADR-022 | Project Formation musí vytvárať len existenčne validné projekty |
| ADR-023 | Distributed Project Model pracuje s validnými projektmi         |
| ADR-024 | Branching poskytuje minimálny smer projektu                     |
| ADR-026 | owner a participant roly sú súčasťou minima                     |
| ADR-027 | view vrstva rešpektuje operačnú pripravenosť                    |
| ADR-031 | origin_topic_id                                                 |
| ADR-078 | transition Topic → Project vytvára projekty v stave `forming`   |
| ADR-082 | domain validation implementuje tieto pravidlá                   |

---

## 24. 📏 Pravidlá implementácie

Systém musí:

1. rozlišovať **existenčnú validitu** a **operačnú pripravenosť**
2. umožniť vznik solo projektu v `forming`
3. vyžadovať 2+ ľudí pre `active`
4. definovať commitment ako explicitný aj implicitný
5. commitment signalom priradiť TTL / decay
6. používať event-driven revalidáciu
7. mať fallback timer po období nečinnosti
8. používať hysterézu pri zmenách stage
9. nesurfacovať `forming` projekt rovnako ako `active`

---

## 25. 🔥 Zhrnutie

> **Projekt môže vzniknúť ako solo iniciatíva, ale aktívnym kolektívnym projektom sa stáva až vtedy, keď získa reálnu sociálnu podporu.**

Tento ADR definuje:

* minimálne existenčné podmienky projektu,
* rozdiel medzi existenciou a operačnou pripravenosťou,
* solo `forming` vs kolektívny `active`,
* commitment model s TTL/decay,
* event-driven revalidáciu,
* hysterézu medzi stavmi.

---

## 26. 🧭 Finálna veta

> **Uvoľni vstup, sprísni výstup.**
>
> Synergetikum nesmie blokovať tiché začiatky, ale zároveň nesmie predstierať, že každý solo nápad je už kolektívny projekt.

```

---

Toto je verzia, ktorú by som už pokojne dal architektovi ako pracovný podklad.

Vidím tam ešte **2 možné jemné upgrady do ďalšieho kola**, ale tie by som už nerobil teraz, iba ak budeš chcieť dokument dotiahnuť úplne nadoraz:

1. doplniť **presné default windows** ako policy, nie ako invariant  
   - napr. `explicit_commitment_ttl_days = 30`
   - `implicit_commitment_window_days = 14`
   - `dormant_after_days = 7`

2. oddeliť v texte ešte explicitnejšie:
   - **domain invarianty**
   - **operational policy thresholds**

To už ale patrí aj k ADR-082.

Pošli ďalší a prepracujem ho rovnako.
```
