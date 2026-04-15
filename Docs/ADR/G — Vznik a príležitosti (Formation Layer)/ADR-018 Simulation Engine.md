# ADR-018 — Simulation Engine

## Status

Proposed

---

## Kontext

Synergetikum už definuje:

* otázky a perspektívy (ADR-009, ADR-010)
* personalizáciu (ADR-011)
* vznik tém a projektov (ADR-012)
* participáciu (ADR-013)
* spoluprácu (ADR-017)

Systém vie zhromaždiť informácie a premeniť ich na akciu.

Chýba však kľúčová schopnosť:

> Pomôcť človeku pochopiť dôsledky jeho rozhodnutí ešte predtým, než ich vykoná.

---

## Rozhodnutie

> Synergetikum obsahuje Simulation Engine, ktorý generuje viacero možných scenárov vývoja (budúcností) na základe dostupných dát, bez toho aby deklaroval jednu "pravdivú" predikciu.

AI:

* navrhuje scenáre
* porovnáva dôsledky
* upozorňuje na riziká

AI nikdy:

* nerozhoduje
* neurčuje jednu správnu cestu

---

## Core princíp

> Neexistuje jedna budúcnosť. Existujú možné budúcnosti.

---

## Ciele Simulation Engine

Simulation Engine musí:

1. Pomôcť používateľovi lepšie pochopiť dôsledky
2. Znížiť riziko zlých rozhodnutí
3. Podporiť reflexiu
4. Udržať pluralitu možností

---

## Typy simulácií

### 1. Decision Simulation

* "čo sa stane, ak spravím X"

---

### 2. Project Simulation

* vývoj projektu
* zdroje, riziká

---

### 3. Social Simulation

* reakcie ľudí
* dynamika skupiny

---

### 4. Long-term Simulation

* dlhodobé dôsledky

---

## Scenáre

Každá simulácia generuje:

* Scenario A
* Scenario B
* Scenario C

Každý scenár obsahuje:

* popis
* predpoklady
* možné výsledky
* riziká

---

## Inputs

Simulation Engine používa:

* Personal AI profil
* historické dáta
* odpovede používateľa
* projektové dáta

---

## Output

Používateľ vidí:

* viac možností
* ich dôsledky
* trade-offs

---

## Explainability

Každý scenár musí vysvetliť:

* z čoho vychádza
* aké sú predpoklady

---

## Anti-manipulation

Systém NESMIE:

* zvýhodniť jeden scenár bez vysvetlenia
* skryť alternatívy

---

## UX princípy

* jednoduché porovnanie scenárov
* žiadny overload
* vizualizácia rozdielov

---

## Architektonické implikácie

### Model

* Scenario
* SimulationRequest

---

### Services

* SimulationEngine
* ScenarioGenerator

---

## Riziká

### 1. Overconfidence

➡️ používateľ verí simulácii ako pravde

### 2. Complexity

➡️ príliš veľa scenárov

---

## Alternatívy

### Single prediction

❌ manipulácia

---

## Väzby na ADR

* ADR-002 — Human-in-the-loop
* ADR-010 — Perspectives

---

## Zhrnutie

Simulation Engine pomáha vidieť budúcnosť ako množinu možností.

---

## Finálna veta

> Synergetikum nehovorí, čo sa stane — ukazuje, čo sa môže stať.
