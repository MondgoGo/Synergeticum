# ADR-013 — Commitment & Participation Model

## Status

Proposed

---

## Kontext

V ADR-012 sme definovali, ako vznikajú Topics a Projects z interakcií.

Kritický problém, ktorý teraz riešime:

> Nie každý, kto odpovie, je ochotný konať.

Bez rozlíšenia medzi:

* názorom
* záujmom
* reálnou akciou

systém:

* generuje šum
* vytvára "mŕtve projekty"
* nevie odlíšiť hodnotných participantov

---

## Rozhodnutie

> Synergetikum explicitne rozlišuje medzi úrovňami participácie na základe správania používateľa (nie deklarácie).

Participácia je modelovaná ako **gradient**, nie binárny stav.

---

## Core princíp

> Názor má nízku váhu. Akcia má vysokú váhu.

---

## Úrovne participácie

### 1. Observer

* len sleduje
* neodpovedá

➡️ nulový signál

---

### 2. Responder

* odpovedá na otázky

➡️ signal: interest

---

### 3. Engaged

* vracia sa k téme
* odpovedá opakovane

➡️ signal: sustained interest

---

### 4. Contributor

* pridáva návrhy
* reaguje na iných

➡️ signal: value creation

---

### 5. Committed

* vyjadrí ochotu konať
* prevezme rolu

➡️ signal: execution readiness

---

### 6. Executor

* robí reálne kroky

➡️ signal: real impact

---

## Dôležité pravidlo

> Úroveň participácie sa neurčuje tým, čo človek povie — ale tým, čo robí.

---

## Commitment signal

Commitment vzniká kombináciou:

* explicitného vyjadrenia ("chcem sa zapojiť")
* správania (návraty, akcie)

---

## Participation scoring

Každý používateľ má:

ParticipationScore =

* response rate
* re-engagement
* contribution activity
* commitment actions

---

## Vplyv na systém

### Topic

* viac responderov → interest
* viac engaged → stability

---

### Project

* viac committed → vznik
* viac executors → prežitie

---

## Filtering

Systém môže:

* filtrovať podľa participácie
* prioritizovať committed používateľov

---

## Anti-pattern

❌ rovnaká váha pre všetkých
❌ hlasovanie
❌ popularity bias

---

## UX implikácie

Používateľ musí vidieť:

* "chceš sa zapojiť?"
* "chceš pomôcť?"

---

## Architektonické implikácie

### Model

* ParticipationProfile
* ParticipationLevel

---

### Services

* ParticipationTrackingService
* CommitmentDetectionService

---

## Riziká

### 1. Fake commitment

➡️ rieši behavior tracking

### 2. Low engagement

➡️ rieši ADR-011

---

## Alternatívy

### Hlasovanie

❌ neodráža realitu

### Role assignment manuálne

❌ centralizácia

---

## Väzby na ADR

* ADR-012 — Topic & Project
* ADR-006 — Learning

---

## Zhrnutie

Participácia je jadro reality systému.

Nie každý hlas má rovnakú váhu.

---

## Finálna veta

> V Synergetiku rozhodujú tí, ktorí robia — nie tí, ktorí hovoria.
