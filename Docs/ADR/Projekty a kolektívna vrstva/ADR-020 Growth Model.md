# ADR-020 — Growth Model (Non-coercive User Development)

## Status

Proposed

---

## Kontext

Synergetikum definuje:

* otázky a perspektívy (ADR-009, ADR-010)
* personalizáciu (ADR-011)
* participáciu a spoluprácu (ADR-013, ADR-017)
* simuláciu (ADR-018)
* príležitosti (ADR-019)

Systém už vie:

* zapojiť používateľa
* premeniť interakciu na akciu

Otvorená otázka:

> Ako systém podporuje rozvoj používateľa bez manipulácie a nátlaku?

---

## Rozhodnutie

> Synergetikum implementuje Growth Model, ktorý rozširuje perspektívu používateľa pomocou riadenej expozície novým témam a pohľadom, bez potlačenia jeho autonómie.

Kľúč:

* ponúkať, nie tlačiť
* rozširovať, nie prepisovať

---

## Core princíp

> Rozvoj vzniká z dobrovoľného stretu s novým, nie z donútenia.

---

## Ciele

Growth Model musí:

1. Rozširovať perspektívu používateľa
2. Znižovať kognitívne bubliny
3. Zachovať dôveru a autonómiu
4. Podporovať reflexiu

---

## Mechanizmy rozvoja

### 1. Exploration Injection

Systém vkladá do feedu:

* nové témy
* neznáme perspektívy

Pravidlo:

* 10–30% obsahu = exploration

---

### 2. Perspective Rotation

Používateľ vidí:

* rôzne pohľady na tú istú tému

---

### 3. Soft Challenge

Systém občas ponúkne:

* náročnejšiu otázku
* inú perspektívu

---

### 4. Reflection prompts

* "prečo si sa rozhodol takto?"
* "čo by si zmenil?"

---

## Growth signals

Systém sleduje:

* diverzitu odpovedí
* zmenu preferencií
* schopnosť pracovať s rôznymi perspektívami

---

## Anti-manipulation pravidlá

Systém NESMIE:

* nútiť používateľa
* skrývať alternatívy
* optimalizovať na "zmenu názoru"

Systém MÔŽE:

* ponúknuť nový pohľad
* vysvetliť kontext

---

## Personalizácia rastu

Growth Model používa:

* current preference profile
* exploration tolerance
* cognitive diversity

---

## UX princípy

* nenápadné rozšírenie
* žiadny tlak
* možnosť preskočiť

---

## Riziká

### 1. Over-challenge

➡️ používateľ odchádza

### 2. Under-challenge

➡️ stagnácia

---

## Architektonické implikácie

### Services

* GrowthEngine
* ExplorationController

---

### Model

* GrowthProfile
* ExplorationRate

---

## Alternatívy

### Hard recommendation

❌ manipulácia

### Žiadny growth

❌ stagnácia

---

## Väzby na ADR

* ADR-010 — Perspectives
* ADR-011 — Personalization
* ADR-018 — Simulation

---

## Zhrnutie

Growth Model zabezpečuje, že používateľ sa rozvíja prirodzene, nie nútene.

---

## Finálna veta

> Synergetikum nerastie používateľa tým, že ho mení — ale tým, že mu ukazuje viac, než videl doteraz.
