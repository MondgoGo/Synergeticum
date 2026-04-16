# 🧾 ADR-078: Topic vs Project Transition Contract

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

Synergetikum má dve samostatné vrstvy:

| Vrstva | ADR | Účel |
|--------|-----|------|
| **Topic Layer** | ADR-031 | Kolektívna diskusia, explorácia, bez záväzku |
| **Project Layer** | ADR-022, 023, 024 | Kolektívna akcia, záväzok, realizácia |

V reálnej implementácii vzniká problém:

> **Kedy presne sa z Topicu stane Project? A čo sa pri tom prenáša a čo nie?**

Bez jasného kontraktu hrozia:

- **Predčasné projekty** – Topic s jednou odpoveďou sa stane projektom
- **Uviaznuté topicy** – diskusia nikdy neprejde do akcie, lebo nikto nevie ako
- **Strata kontextu** – projekt nevie, z akej diskusie vznikol
- **Duplicita** – rovnaká diskusia vedie k trom projektom

---

## 3. ⚖️ Rozhodnutie

Systém zavádza **explicitný transition contract** medzi Topicom a Projectom.

> **Topic → Project je jednosmerná, explicitná, merateľná transformácia, ktorá vytvára nový Project Node a zachováva Topic ako historický kontext.**

---

## 4. 🧠 Definícia Topic vs Project (ostré hranice)

| Vlastnosť | **Topic** | **Project** |
|-----------|-----------|--------------|
| **Primárny účel** | Explorácia, diskusia, zber perspektív | Akcia, realizácia, rozhodovanie |
| **Záväzok** | Žiadny (môžeš len čítať) | Explicitný (commitment signal) |
| **Výstupy** | Odpovede, názory, perspektívy | Vetvy, argumenty, úlohy, rozhodnutia |
| **Lifecycle** | Krátky (dni až týždne) | Dlhý (týždne až roky) |
| **Stabilita** | Nízka (mení sa každou odpoveďou) | Vyššia (zmeny sú vetvy, nie prepis) |
| **Kto môže vytvoriť** | Ktokoľvek (otázkou) | Len system (cez transition) alebo user explicitne |
| **Expectation** | "Budeme sa rozprávať" | "Budeme niečo robiť" |

---

## 5. 🔄 Transition Trigger Conditions

Projekt môže vzniknúť z Topicu **iba ak sú splnené všetky tieto podmienky**:

### 5.1 Interest threshold

```
topic.aggregates.interest_level > 0.6
```

Merané ako kombinácia:
- Počet unikátnych participantov
- Frekvencia odpovedí za posledných 7 dní
- Počet "follow" alebo "watch" signálov

### 5.2 Commitment threshold

```
topic.signals.commitment_count >= 3
```

Kde commitment je:
- Explicitné "chcem sa zapojiť" (používateľ klikne)
- Alebo implicitné (3+ odpovede v diskusii)

### 5.3 Direction clarity

```
topic.signals.has_concrete_direction == true
```

Topic musí mať aspoň jeden:
- Návrh riešenia (proposed solution)
- Alebo identifikovaný problém (problem statement)
- Alebo konkrétnu otázku, ktorá sa pýta "ako spraviť"

### 5.4 Minimum participants

```
topic.aggregates.participant_count >= 2
```

Okrem autora otázky musí byť aspoň jeden ďalší participant.

### 5.5 No active project already

```
topic.project_reference == null
```

Topic ešte nesmie byť spojený s existujúcim projektom.

---

## 6. 🔄 Transition Process (krok za krokom)

### Krok 1: Detection

Personal AI používateľa (alebo ktoréhokoľvek participanta) deteguje, že Topic spĺňa trigger conditions.

### Krok 2: Notification

Systém zobrazí participantom:

```
📢 Téma "{{topic.name}}" dosiahla dostatočný záujem na vznik projektu.

Kto chce byť súčasťou projektu?
[✓] Chcem sa zapojiť
[ ] Nie, len diskutujem
```

### Krok 3: Confirmation

Minimálne **2 participant** (alebo 50% aktívnych participantov, podľa toho, čo je viac) musia kliknúť "Chcem sa zapojiť".

### Krok 4: Project Creation

Systém automaticky vytvorí:

```json
{
  "node_id": "proj_{uuid}",
  "node_type": "Project",
  "name": topic.name + " (projekt)",
  "content": {
    "origin_topic_id": topic.topic_id,
    "branch_root_id": "branch_main_{uuid}",
    "lifecycle_stage": "forming",
    "created_from_transition": true,
    "transition_timestamp": "2026-04-16T10:00:00Z"
  },
  "relations": [
    { "target_id": topic.topic_id, "relation_type": "originated_from" }
  ],
  "rights": {
    "owner": topic.created_by,  // autor otázky
    "visibility": topic.attributes.visibility
  }
}
```

### Krok 5: Initial Branch Creation

Systém vytvorí **počiatočnú vetvu** projektu, ktorá obsahuje:

- Problem statement (extrahovaný z Topicu)
- Navrhované smery (z topic.signals.directions)
- Zoznam participantov, ktorí potvrdili záujem

### Krok 6: Topic Update

Topic sa aktualizuje:

```json
{
  "topic.lifecycle.stage": "converted_to_project",
  "topic.project_reference": "proj_{uuid}",
  "topic.converted_at": "2026-04-16T10:00:00Z"
}
```

### Krok 7: Notification to all participants

Všetci, ktorí kedy odpovedali v Topici, dostanú notifikáciu:

```
💡 Z témy "{{topic.name}}" vznikol projekt.
👉 Pozrieť projekt
```

---

## 7. 📦 Čo sa prenáša (Topic → Project)

| Položka | Prenáša sa? | Ako |
|---------|-------------|-----|
| Názov | ✅ | Base name + " (projekt)" |
| Popis | ✅ | Extrahovaný problem statement |
| Autor otázky | ✅ | Stáva sa owner projektu |
| Participants | ✅ | Tí, čo potvrdili, sú initial contributors |
| Odpovede | ❌ | Zostávajú v Topicu ako historický kontext |
| Perspektívy | ⚠️ | Extrahujú sa do initial branch ako "considerations" |
| Commitment signals | ✅ | Prepočítajú sa na initial alignment |
| Diskusia | ❌ | Nie je súčasťou projektu (len odkaz) |

---

## 8. 🚫 Čo sa NEPRENÁŠA (a prečo)

| Položka | Dôvod |
|---------|-------|
| Všetky odpovede | Projekt by bol preťažený diskusiou, nie akciou |
| Hlasovania | Topic nemá hlasovania, len signály |
| Neaktívni participant | Len aktívni chcú byť v projekte |
| Dočasné agregáty | Projekt začína čistým štítom (okrem kontextu) |

---

## 9. 🔗 Vzťah po transition

Po transition:

- **Topic zostáva** – ako historický kontext a zdroj argumentov
- **Topic je len na čítanie** – už sa nedá odpovedať (alebo áno, ale neovplyvňuje to projekt)
- **Project má odkaz na Topic** – `origin_topic_id`
- **Topic má odkaz na Project** – `project_reference`

Používateľ môže kedykoľvek:
- Prejsť z Projectu do pôvodného Topicu (historický kontext)
- Prejsť z Topicu do Projectu (ak existuje)

---

## 10. 🧠 Manuálny override

Používateľ (ktorýkoľvek participant) môže **explicitne vytvoriť projekt z Topicu** aj bez splnenia trigger conditions.

Ale systém zobrazí varovanie:

```
⚠️ Téma ešte nemá dostatočný záujem.
Projekt vytvorený z nej bude mať nízku šancu na úspech.
Naozaj chcete pokračovať?
[Áno] [Nie]
```

---

## 11. 🚫 Čo sa NESMIE stať

| Zakázaná situácia | Prečo |
|-------------------|-------|
| Topic → Project bez zachovania Topic | Stráca sa historický kontext |
| Automatický transition bez potvrdenia | Používatelia stratia kontrolu |
| Projekt bez origin_topic_id | Nevie sa, odkiaľ prišiel |
| Topic s project_reference, ktorý neexistuje | Orphaned reference |
| Viacero projektov z jedného Topicu | Iba jeden (ale Topic môže byť forknutý) |

---

## 12. 📊 Príklad kompletného transition

### Before: Topic

```json
{
  "node_id": "topic_community_garden_001",
  "node_type": "Topic",
  "name": "Komunitná záhrada v Petržalke",
  "lifecycle": {
    "stage": "candidate_for_project"
  },
  "aggregates": {
    "interest_level": 0.78,
    "participant_count": 5,
    "response_count": 23
  },
  "signals": {
    "commitment_count": 4,
    "directions": [
      "hľadáme pozemok",
      "potrebujeme grant"
    ]
  }
}
```

### After: Project

```json
{
  "node_id": "proj_community_garden_001",
  "node_type": "Project",
  "name": "Komunitná záhrada v Petržalke (projekt)",
  "content": {
    "origin_topic_id": "topic_community_garden_001",
    "lifecycle_stage": "forming"
  },
  "relations": [
    { "target_id": "topic_community_garden_001", "relation_type": "originated_from" }
  ]
}
```

### After: Topic (updated)

```json
{
  "node_id": "topic_community_garden_001",
  "node_type": "Topic",
  "lifecycle": {
    "stage": "converted_to_project"
  },
  "project_reference": "proj_community_garden_001",
  "converted_at": "2026-04-16T10:00:00Z"
}
```

---

## 13. 🧠 Dôsledky rozhodnutia

### ✅ Pozitívne

| Dôsledok | Popis |
|----------|-------|
| **Jasné hranice** | Developer vie, čo je Topic a čo Project |
| **Žiadne predčasné projekty** | Trigger conditions bránia chaosu |
| **Historický kontext** | Topic zostáva, nestráca sa diskusia |
| **Používateľská kontrola** | Transition vyžaduje explicitné potvrdenie |
| **Škálovateľnosť** | Väčšina topicov zostane v diskusii |

### ❗ Negatívne / Trade-offs

| Dôsledok | Mitigácia |
|----------|-----------|
| **Ďalšia komplexita** | Ale je to explicitné a testovateľné |
| **Možnosť uviaznutia** | Manuálny override + fade mechanism |
| **Thresholds môžu byť nesprávne** | Nastaviteľné používateľom (advanced) |

---

## 14. 🚫 Alternatívy (zamietnuté)

| Alternatíva | Dôvod zamietnutia |
|-------------|-------------------|
| **Žiadny transition (Topic a Project oddelené)** | Nikdy nevzniknú projekty |
| **Automatický transition bez potvrdenia** | Používatelia stratia kontrolu |
| **Topic sa stane Projectom (zmena typu)** | Stráca sa historická diskusia |
| **Len manuálny override** | Väčšina topicov nikdy neprejde do projektu |

---

## 15. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-022 | Project Formation – tento ADR je špecifikácia prechodu |
| ADR-023 | Distributed Project Model – projekt vzniká z Topicu |
| ADR-024 | Branching – initial branch vzniká z Topic signálov |
| ADR-031 | Topic Layer – definuje Topic, z ktorého sa prechádza |
| ADR-015 | Participation Filter – kto vidí transition návrh |
| ADR-036 | Reciprocity – commitment signals ovplyvňujú transition |

---

## 16. 📏 Pravidlá implementácie (high-level)

Systém musí:

1. Ukladať `topic.aggregates` a `topic.signals` pre detekciu triggerov
2. Periodicky (napr. každých 24h) vyhodnocovať topicy v stave `active` alebo `candidate_for_project`
3. Pri splnení podmienok zobraziť notifikáciu participantom
4. Po potvrdení vytvoriť Project Node a prepojiť ho s Topicom
5. Aktualizovať Topic na `converted_to_project`
6. Nikdy nemažte Topic (len mení stage)

---

## 17. 🧪 Testovacie scenáre (pre pilot)

| Scenár | Očakávaný výsledok |
|--------|---------------------|
| Topic s 1 participantom, vysoký interest | Žiadny transition |
| Topic s 2 participantmi, nízky interest | Žiadny transition |
| Topic s 3 participantmi, 3 commitments, direction | Transition navrhnutý |
| Iba 1 participant potvrdí záujem | Žiadny transition |
| 2+ participantov potvrdí | Transition prebehne |
| Manuálny override pri nespĺňaní podmienok | Varovanie, ale povolené |

---

## 18. 🔥 Zhrnutie

> **Topic je diskusia. Project je akcia. Prechod z Topicu do Projectu je explicitný, merateľný a kontrolovaný používateľmi.**

Tento ADR definuje:

- Ostré hranice medzi Topic a Project
- Trigger conditions (interest, commitment, direction, participants)
- 7-krokový transition process
- Čo sa prenáša a čo nie
- Manuálny override
- Čo sa nesmie stať

---

## 19. 🧭 Finálna veta

> **Projekty nevznikajú z otázok. Vznikajú z diskusií, ktoré dosiahnu dostatočný záujem a záväzok. Až potom – a len vtedy – sa Topic stane Projectom.**

---

**Dátum:** 2026-04-16  
**Stav:** Proposed  
**Typ:** Transition Contract ADR  
**Verzia:** 1.0