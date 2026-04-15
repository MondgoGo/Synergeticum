# ADR-006 — Learning Model (Behavior-based Learning)

## Status

Proposed

## Kontext

Personal AI v Synergetiku je evolučný model, ktorý sa má učiť priamo zo správania konkrétneho používateľa. Na rozdiel od klasických AI prístupov:

* nejde o offline trénovaný globálny model,
* nejde primárne o veľké LLM inferencie,
* nejde o jednorazové profilovanie.

Model sa musí učiť priebežne z interakcie (question → answer → update), byť vysvetliteľný a stabilný. Bez jasne definovaného learning modelu hrozí:

* nekonzistentné správanie systému,
* „skákanie" profilu pri malej zmene,
* neauditovateľnosť (nevieme vysvetliť prečo sa profil zmenil),
* slabá kvalita personalizácie.

---

## Rozhodnutie

V systéme Synergetikum platí:

> Personal AI sa učí primárne z pozorovaného správania používateľa (behavior-based learning) a aktualizuje sa postupne cez event-driven mechanizmus.

Základné princípy:

* učenie je **inkrementálne** (po každej interakcii),
* učenie je **lokálne** (on-device),
* učenie je **vysvetliteľné** (rule + score based),
* učenie je **stabilizované** (smoothing, decay),
* učenie je **reverzibilné** (cez event log a rekonštrukciu).

---

## Vstupy do učenia (Signals)

Learning engine pracuje so signálmi:

### Explicitné signály

* odpoveď na otázku (value),
* typ odpovede (single, scale, text),
* výber možností.

### Implicitné signály

* preskočenie otázky,
* čas odpovede (DurationMs),
* frekvencia interakcie,
* návrat k téme (re-engagement).

### Kontextové signály (voliteľné)

* čas dňa,
* lokálny kontext (napr. mesto),
* session pattern.

---

## Aktualizačný model

Každá interakcia generuje **event**, ktorý spúšťa update profilu.

### 1. Interest update

* zvýšenie skóre pre TopicKey, ak používateľ odpovedá,
* zníženie skóre pri opakovanom ignorovaní,
* aplikácia **decay** pre dlhodobo neaktívne témy.

### 2. Preference update

* mapovanie odpovedí na dimenzie (Practical vs Conceptual, Local vs Systemic, ...),
* aplikácia malých delt (δ),
* následná **normalizácia**.

### 3. Cognitive modes update

* mapovanie typu otázky/odpovede na kognitívne módy,
* aktualizácia **distribúcie** (nie labelu),
* zachovanie sumy/rozsahu (normalization).

### 4. Participation update

* výpočet response rate,
* detekcia záväzku (commitment signals),
* odhad dropout tendency.

### 5. Confidence update

* rast confidence s počtom kvalitných signálov,
* penalizácia pri nekonzistentnom správaní,
* per-dimension confidence.

---

## Stabilizačné mechanizmy

Aby sa zabránilo „skáčaniu" profilu:

* **Learning rate (α)**: malé kroky pri každej zmene,
* **Smoothing**: exponenciálne vyhladzovanie,
* **Decay (λ)**: postupné zabúdanie starých signálov,
* **Clamping**: limity min/max hodnôt,
* **Hysteresis**: zmena trendu vyžaduje viac dôkazov.

---

## Event-driven architektúra

Každá zmena je reprezentovaná ako event:

* `QuestionAnswered`
* `QuestionSkipped`
* `InterestUpdated`
* `PreferenceUpdated`
* `CognitiveModeAdjusted`
* `ParticipationUpdated`
* `ProfileSnapshotUpdated`

Eventy sa ukladajú a umožňujú:

* audit („prečo sa to zmenilo"),
* replay (rekonštrukcia stavu),
* experimentovanie s novými pravidlami.

---

## Snapshot model

Po určitom počte eventov alebo po každej interakcii sa vytvára:

* **PersonalAiSnapshot**

Snapshot obsahuje:

* agregované hodnoty,
* top topics,
* preference summary,
* cognitive summary,
* participation summary,
* krátke vysvetlenie (explanation).

UI vždy pracuje so snapshotom, nie s raw eventmi.

---

## Výstupy learning modelu

Learning model ovplyvňuje:

* poradie otázok (ranking),
* výber tém,
* výber perspektív,
* intenzitu exploration,
* obsah Reflection view.

---

## Dôsledky

* systém sa učí rýchlo aj z malého počtu interakcií,
* správanie je stabilné a predvídateľné,
* zmeny sú vysvetliteľné,
* profil je auditovateľný a reprodukovateľný,
* systém funguje bez veľkých modelov a bez cloudu.

---

## Architektonické implikácie

### Domain Services

* `ProfileLearningService` obsahuje jadro update logiky,
* `ParticipationInferenceService` pre participation.

### Rules layer

* `InterestUpdateRules`
* `PreferenceUpdateRules`
* `CognitiveModeRules`
* `ExplorationRules`

### Persistence

* oddelenie **event logu** a **snapshotu**,
* možnosť replay.

### Performance

* výpočty musia byť ľahké (on-device),
* bez potreby GPU alebo veľkých modelov.

---

## Alternatívy

### Batch/offline learning (periodický tréning)

Zamietnuté.

Dôvod:

* pomalá adaptácia,
* slabá spätná väzba pre používateľa.

### Black-box ML/LLM ako hlavný learning mechanizmus

Zamietnuté.

Dôvod:

* nevysvetliteľnosť,
* vysoké nároky,
* slabá kontrola.

### Čisto statické pravidlá bez učenia

Zamietnuté.

Dôvod:

* nulová adaptácia,
* slabá personalizácia.

---

## Väzby na ďalšie ADR

* ADR-005 — Personal AI Model Definition
* ADR-007 — User Profile Representation
* ADR-009 — Question-based Interaction Model
* ADR-011 — Personalization Strategy
* ADR-030 — Sync & Persistence Strategy
* ADR-034 — Personal AI Internal Model

---

## Zhrnutie

Learning model je jednoduchý, inkrementálny a vysvetliteľný.

Namiesto „veľkej AI" ide o dobre navrhnutý systém signálov, pravidiel a postupnej adaptácie.

---

## Finálna veta

> Personal AI sa učí z toho, čo používateľ robí — nie z toho, čo by mal robiť.
