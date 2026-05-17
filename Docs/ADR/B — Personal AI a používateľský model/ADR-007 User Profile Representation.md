ADR-007 — User Pr# ADR-007 — User Profile Representation

## Status

Proposed

## Kontext

Personal AI v systéme Synergetikum je lokálny, evolučný model používateľa. Už je definované:

* čo Personal AI je (ADR-005),
* ako sa učí (ADR-006).

Chýba však presná definícia toho, **ako je používateľ reprezentovaný dátovo a konceptuálne vo vnútri systému**.

Bez tejto definície hrozí:

* miešanie surových dát a odvodených záverov,
* nejasná hranica medzi históriou a aktuálnym stavom,
* slabá auditovateľnosť,
* nečitateľná persistence vrstva,
* ťažká rozšíriteľnosť modelu.

Preto je potrebné explicitne určiť, z akých vrstiev sa profil skladá a aký je medzi nimi vzťah.

---

## Rozhodnutie

V systéme Synergetikum je používateľ reprezentovaný ako:

> **viacvrstvový profilový model, ktorý oddeľuje pozorovania, odvodené signály a aktuálny agregovaný stav**

Tento model sa skladá z troch hlavných vrstiev:

1. **Raw Observations Layer**
2. **Derived Profile Layer**
3. **Snapshot Layer**

Tieto vrstvy sú doplnené o **Event Log Layer**, ktorá zachytáva zmeny v čase.

---

## 1. Raw Observations Layer

Táto vrstva obsahuje to, čo sa **skutočne stalo**.

Príklady:

* používateľ odpovedal na otázku,
* používateľ otázku preskočil,
* používateľ sa vrátil k téme,
* používateľ prijal alebo odmietol odporúčanie.

Raw vrstva neobsahuje interpretácie.

Jej účel:

* byť zdrojom pravdy o správaní,
* umožniť audit,
* umožniť replay a nové odvodenie profilu.

Táto vrstva zahŕňa entity ako:

* Question
* Answer
* raw interaction metadata

---

## 2. Derived Profile Layer

Táto vrstva obsahuje **odvodené signály**.

Nejde o priame dáta, ale o interpretované hodnoty, ktoré vznikli z raw observations.

Príklady:

* interest score pre tému,
* capability score,
* preference dimensions,
* participation metrics,
* cognitive modes distribution,
* confidence per dimension.

Jej účel:

* reprezentovať dlhodobejšie vzorce,
* umožniť personalizáciu,
* napájať learning a selection engine.

Táto vrstva sa mení postupne a je stabilizovaná.

---

## 3. Snapshot Layer

Táto vrstva obsahuje **aktuálny agregovaný pohľad na profil**, pripravený pre runtime a UI.

Snapshot je:

* zjednodušený,
* čitateľný,
* optimalizovaný pre rýchle použitie.

Príklady:

* top topics,
* top capabilities,
* aktuálne preference summary,
* aktuálne cognitive summary,
* participation summary,
* krátke vysvetlenie „čo sa systém naučil“.

UI pracuje prednostne so snapshotom, nie s raw dátami.

---

## 4. Event Log Layer

Popri vyššie uvedených vrstvách systém vedie:

> **event log zmien profilu**

Každá významná zmena je reprezentovaná ako event.

Príklady:

* `QuestionAnswered`
* `QuestionSkipped`
* `InterestUpdated`
* `PreferenceUpdated`
* `CognitiveModeAdjusted`
* `ProfileSnapshotUpdated`

Účel:

* audit,
* vysvetliteľnosť,
* rekonštrukcia,
* experimentovanie s novými pravidlami učenia.

---

## 5. Vzťahy medzi vrstvami

Tok dát je nasledovný:

```text
Raw Observations → Learning / Inference → Derived Profile → Snapshot
                        ↓
                     Event Log
```

Interpretácia:

* raw observations sú vstup,
* learning engine vytvára odvodené signály,
* snapshot je agregovaný aktuálny pohľad,
* event log zachytáva priebeh zmien.

---

## 6. Čo profil NIE JE

Používateľský profil v Synergetiku NIE JE:

* jedna pevná kategória človeka,
* jeden numerický „level človeka",
* psychologická nálepka,
* statický dotazníkový výsledok,
* black-box embedding bez vysvetlenia.

Profil je:

* dynamický,
* pravdepodobnostný,
* čiastočne neistý,
* vždy otvorený zmene.

---

## 7. Povinné profilové oblasti

Každý používateľský profil musí minimálne obsahovať:

### 7.1 Interest Profile

* témy,
* intenzita záujmu,
* trend,
* confidence.

### 7.2 Capability Profile

* oblasti možného prínosu,
* confidence,
* source.

### 7.3 Preference Profile

* decision dimensions,
* spôsob myslenia,
* prioritné uhly pohľadu.

### 7.4 Participation Profile

* response rate,
* commitment tendency,
* dropout tendency,
* preferred participation type.

### 7.5 Cognitive Modes Distribution

* rozloženie typov uvažovania,
* vždy ako distribúcia, nie label.

---

## 8. Confidence ako súčasť profilu

Každá významná profilová dimenzia musí mať aj confidence.

Dôvod:

* systém si nesmie byť „príliš istý“ z mála dát,
* nové alebo nestabilné profily musia byť označené ako neisté,
* exploration engine sa musí správať inak pri nízkej confidence.

Confidence je preto povinná meta-vrstva profilu.

---

## 9. Reprezentačný princíp

Profil sa má reprezentovať:

* modulárne,
* po menších objektoch,
* s oddelenou persistenciou,
* s možnosťou nezávislej aktualizácie jednotlivých častí.

To znamená, že systém nesmie ukladať Personal AI ako jeden monolitický blob.

Preferovaný model:

* samostatné profily / tabuľky / dokumenty,
* plus agregovaný snapshot.

---

## 10. Dôsledky

### Pozitíva

* vysoká vysvetliteľnosť,
* auditovateľnosť,
* jednoduchšia debugovateľnosť,
* možnosť postupného rozširovania,
* menšia závislosť na black-box AI.

### Negatíva

* vyššia modelová a dátová komplexita,
* potreba dobre definovaných mapperov a storage vrstvy,
* viac objektov na udržiavanie.

---

## 11. Architektonické implikácie

### Domain model

Doména musí odlíšiť:

* observations,
* profile signals,
* snapshots.

### Persistence

Storage musí odlíšiť:

* raw history,
* derived state,
* event log,
* latest snapshot.

### Application layer

Use cases musia pracovať primárne:

* s raw vstupom pri zápise,
* so snapshotom pri čítaní,
* s derived profile pri rozhodovaní.

### UI

UI nesmie priamo čítať raw observations ako primárny zdroj identity používateľa.

---

## Alternatívy

### Jedna tabuľka / jeden objekt „user_profile"

Zamietnuté.

Dôvod:

* nečitateľnosť,
* slabá auditovateľnosť,
* ťažké rozšírenie.

### Čisto raw history bez derived state

Zamietnuté.

Dôvod:

* príliš pomalé a nešikovné pre runtime personalizáciu.

### Čisto derived profile bez raw vrstvy

Zamietnuté.

Dôvod:

* nedá sa vysvetliť a spätne overiť, odkiaľ profil vznikol.

---

## Väzby na ďalšie ADR

* ADR-005 — Personal AI Model Definition
* ADR-006 — Learning Model (Behavior-based Learning)
* ADR-009 — Question-based Interaction Model
* ADR-011 — Personalization Strategy
* ADR-030 — Sync & Persistence Strategy
* ADR-034 — Personal AI Internal Model

---

## Zhrnutie

Používateľský profil v Synergetiku nie je jedna entita, ale vrstvený model.

Jeho základom sú:

* raw observations,
* derived profile signals,
* snapshot,
* event log.

Toto rozdelenie je nevyhnutné pre vysvetliteľnosť, audit a kvalitnú personalizáciu.

---

## Finálna veta

> Používateľ v Synergetiku nie je reprezentovaný jedným číslom ani jednou nálepkou, ale živým viacvrstvovým profilom, ktorý sa vyvíja v čase.
