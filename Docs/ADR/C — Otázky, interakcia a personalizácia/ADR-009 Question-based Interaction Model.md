# ADR-009 — Question-based Interaction Model

## Status

Proposed

## Kontext

Synergetikum buduje Personal AI na základe interakcie používateľa so systémom (ADR-006, ADR-007, ADR-008). Kľúčová voľba je, **aký je primárny interakčný model** medzi používateľom a systémom.

Alternatívy zahŕňajú:

* voľný chat (LLM štýl),
* formuláre/dotazníky,
* feed obsahu (scroll),
* explicitne navrhnuté otázky.

Bez jasného rozhodnutia hrozí:

* nekonzistentná UX,
* slabé a nekvalitné signály pre učenie,
* ťažká vysvetliteľnosť.

---

## Rozhodnutie

V systéme Synergetikum platí:

> Primárnym interakčným mechanizmom sú **kurátorsky navrhnuté otázky** (Question-based Interaction).

Používateľ vstupuje do systému cez:

* otázky,
* ich perspektívy,
* odpovede a rozhodnutia.

Chat je sekundárny (doplnkový) nástroj, nie primárny.

---

## Ciele modelu

Question-based model má zabezpečiť:

* vysokú kvalitu signálov pre learning,
* porovnateľnosť odpovedí medzi používateľmi,
* vysvetliteľnosť,
* možnosť škálovať do kolektívnej vrstvy (topics → projekty),
* kontrolu nad framingom otázky.

---

## Štruktúra otázky

Každá otázka obsahuje minimálne:

* `QuestionId`
* `Text`
* `TopicKey`
* `QuestionType` (single, multi, scale, text)
* `PerspectiveType`
* `ComplexityLevel`
* `Tags`
* `IsExplorationQuestion`

Voliteľne:

* kontext (lokácia, čas),
* prečo je otázka zobrazená (explainability),
* možné doplňujúce otázky.

---

## Perspektívy (Perspective System)

Tá istá téma môže byť položená viacerými spôsobmi:

* Praktická (čo by si spravil)
* Analytická (aké sú možnosti/dáta)
* Systémová (dopad na komunitu/spoločnosť)
* Reflexívna (čo je pre teba dôležité)
* Akčná (chceš sa zapojiť?)

Princíp:

* pravda sa nemení,
* mení sa uhol pohľadu.

---

## Typy otázok

### 1. Core otázky

* základné budovanie profilu
* vysoká priorita

### 2. Context otázky

* viazané na miesto/čas
* napr. lokálne potreby

### 3. Exploration otázky

* rozširujú obzor
* znižujú bias modelu

### 4. Commitment otázky

* zisťujú ochotu konať

---

## Odpoveďový model

Každá otázka má definovaný typ odpovede:

* Single choice
* Multi choice
* Scale (napr. 1–5)
* Short text
* Skip

Každá odpoveď generuje:

* explicitný signál,
* implicitné metadata (čas, frekvencia).

---

## Question Lifecycle

Otázka má lifecycle:

1. Creation (seed alebo generovaná)
2. Activation
3. Distribution (výber pre používateľa)
4. Interaction (odpoveď/skip)
5. Evaluation (kvalita signálov)
6. Deactivation / update

---

## Selection & Ordering

Výber otázok pre používateľa riadi `QuestionSelectionService`.

Faktory:

* relevance (profil),
* exploration (nové témy),
* diverzita,
* nedávna história,
* confidence.

Výstup:

* batch otázok v poradí.

---

## Explainability

Každá otázka môže obsahovať:

* „Prečo to vidím"
* napr.:

  * „pretože si sa zaujímal o tému X",
  * „chceme lepšie pochopiť tvoje preferencie",
  * „nová téma na rozšírenie obzoru".

---

## Vzťah k Topic Layer

Otázky sú vstupná vrstva.

Viac odpovedí na rovnakú tému vytvára:

→ Topic Object (ADR-031)

A z topicu môže vzniknúť:

→ Project (ADR-022)

---

## Vzťah k Personal AI

Otázky sú hlavný zdroj učenia:

* feedujú learning engine,
* aktualizujú interest, preference, participation,
* zvyšujú confidence.

---

## Dôsledky

### Pozitíva

* kvalitné a štruktúrované dáta,
* vysvetliteľnosť,
* škálovateľnosť do kolektívnych procesov,
* lepší cold start než voľný chat.

### Negatíva

* potreba kvalitného návrhu otázok,
* riziko „dotazníkového“ pocitu,
* UX musí byť veľmi dobre navrhnuté.

---

## Architektonické implikácie

### Domain

* Question ako prvotriedna entita

### Services

* QuestionSelectionService
* Answer processing pipeline

### Storage

* samostatná tabuľka/collection pre otázky
* väzby na topicy

### UI

* QuestionFeed
* QuestionDetail
* Answer input komponenty

---

## Alternatívy

### Chat-first model (LLM chat)

Zamietnuté ako primárny model.

Dôvod:

* slabá štruktúra dát,
* nízka porovnateľnosť,
* ťažká vysvetliteľnosť.

### Statický dotazník

Zamietnuté.

Dôvod:

* neadaptívne,
* slabý engagement.

---

## Väzby na ďalšie ADR

* ADR-006 — Learning Model
* ADR-007 — User Profile Representation
* ADR-008 — Data Sources Strategy
* ADR-010 — Perspective System
* ADR-011 — Personalization Strategy
* ADR-031 — Topic Layer & Project Formation

---

## Zhrnutie

Otázky sú základná jednotka interakcie v Synergetiku.

Nie chat, nie feed, ale premyslene navrhnuté otázky.

---

## Finálna veta

> Synergetikum sa nepýta náhodne — každá otázka je navrhnutý nástroj na pochopenie človeka a sveta.
