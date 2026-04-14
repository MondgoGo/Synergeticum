# ADR-012 — Topic Formation & Project Trigger

## Status

Proposed

---

## Kontext

Synergetikum už definuje:

* otázky (ADR-009)
* perspektívy (ADR-010)
* personalizáciu (ADR-011)
* learning model (ADR-006)

To znamená, že systém vie:

* klásť otázky
* zbierať odpovede
* učiť sa z nich

Chýba však kritická vec:

> Ako sa z otázok a odpovedí stane niečo reálne?

Bez toho:

* systém ostane len "inteligentný dotazník"
* nevznikne hodnota
* nevznikne spolupráca

---

## Rozhodnutie

> Keď sa okolo témy nazbiera dostatočný signál (záujem + zhoda + aktivita), systém automaticky vytvorí Topic a následne môže vzniknúť Project.

---

## Core princíp

> Interakcia → Signál → Topic → Project

---

## Fáza 1 — Signal aggregation

Systém sleduje odpovede na otázky viazané na rovnaký problém (TopicKey).

Zbiera:

* počet odpovedí
* pozitívny záujem
* intenzitu (scale)
* engagement
* návraty k téme

---

## Fáza 2 — Topic formation

Topic vznikne, keď:

* existuje dostatok odpovedí
* existuje konzistentný smer
* existuje záujem viac než jedného človeka

Topic obsahuje:

* názov problému
* agregované odpovede
* základné perspektívy
* prvé návrhy riešení

---

## Fáza 3 — Topic evolution

Topic žije a vyvíja sa:

* pribúdajú nové odpovede
* vznikajú konflikty (rôzne názory)
* vznikajú návrhy riešení

---

## Fáza 4 — Project trigger

Project vznikne, keď sa splní:

### 1. Interest

* dostatočný počet ľudí

### 2. Alignment

* určitá zhoda alebo jasné vetvy

### 3. Commitment

* niekto chce konať

---

## Project vzniká automaticky alebo poloautomaticky

Varianty:

### A. Auto-trigger

* systém navrhne projekt

### B. Human-trigger

* používateľ klikne "poďme to spraviť"

---

## Project obsahuje

* cieľ
* návrhy riešení
* účastníkov
* commitment
* vetvy (alternatívy)

---

## Branching (kritická časť)

Ak existuje konflikt:

* nevzniká boj
* vznikajú vetvy

Príklad:

* pizzeria 1-poschodová
* pizzeria 2-poschodová

➡️ oba projekty existujú

---

## Dead topics

Ak:

* nie je záujem
* nie je aktivita

Topic postupne "zomiera"

---

## Scoring Topicu

Každý topic má score:

Score =

* interest
* participation
* commitment
* aktivita

---

## Vzťah k Personal AI

Personal AI:

* pomáha vyhodnotiť relevantnosť topicu
* odporúča zapojenie

---

## Riziká

### 1. Fake interest

➡️ veľa odpovedí, málo akcie

### 2. Ego conflicts

➡️ vetvy riešia problém

### 3. Noise

➡️ filtering + scoring

---

## Architektonické implikácie

### Entities

* Question
* Answer
* Topic
* Project

---

### Services

* TopicAggregationService
* TopicScoringService
* ProjectTriggerService

---

### Storage

* Topic store
* Project store

---

## Alternatívy

### Manuálne vytváranie projektov

❌ slabá emergentnosť

### Hlasovanie

❌ centralizácia

---

## Väzby na ADR

* ADR-009 — Questions
* ADR-010 — Perspectives
* ADR-011 — Personalization

---

## Zhrnutie

Synergetikum premieňa otázky na realitu.

Nie cez hlasovanie.
Nie cez autoritu.

Ale cez:
👉 záujem
👉 aktivitu
👉 ochotu konať

---

## Finálna veta

> Projekt v Synergetiku nevzniká rozhodnutím — vzniká tým, že ho ľudia začnú robiť.
