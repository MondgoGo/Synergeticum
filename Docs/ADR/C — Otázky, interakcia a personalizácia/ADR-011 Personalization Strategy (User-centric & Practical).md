# ADR-011 — Personalization Strategy (User-centric & Practical)

## Status

Proposed

---

## Kontext

Synergetikum má definované:

* otázky ako základ interakcie (ADR-009)
* perspektívy ako vstupné body (ADR-010)
* Personal AI profil (ADR-005, ADR-007)

Teraz riešime kritickú otázku:

> Ako rozhodnúť, čo konkrétny používateľ uvidí ako prvé?

Toto rozhodnutie priamo ovplyvňuje:

* či používateľ odpovie
* či sa systém naučí
* či vzniknú projekty

---

## Rozhodnutie

> Personalizácia v Synergetiku riadi PORADIE, VÝBER a INTENZITU obsahu na základe Personal AI, pričom nikdy nesmie obmedziť realitu len na jeden pohľad.

👉 kľúč:

* personalizácia = prioritizácia
* nie filtrácia

---

## Cieľ personalizácie

Personalizácia musí zabezpečiť:

1. Používateľ odpovie (engagement)
2. Odpoveď má hodnotu (signal quality)
3. Používateľ sa rozvíja (exploration)
4. Nevzniká bublina (anti-bias)

Ak chýba bod 1 → systém zomrie
Ak chýba bod 2 → systém je zbytočný

---

## Core princíp

> Najprv zobraz to, na čo používateľ pravdepodobne odpovie

Nie:

* čo je "najlepšie"
* čo je "najpravdivejšie"

Ale:

* čo spustí interakciu

---

## Personalization pipeline

Personalizácia prebieha v 4 krokoch:

### 1. Candidate Selection

Vyber potenciálnych otázok:

* podľa tém (interest)
* podľa kontextu (lokalita, čas)
* podľa aktivity systému

---

### 2. Scoring

Každý kandidát dostane score:

Score =

* engagement probability
* topic relevance
* novelty
* diversity penalty
* confidence weight

---

### 3. Perspective Selection

Pre každú tému sa vyberie perspektíva:

* preferovaná (najvyššia šanca odpovede)
* alebo exploration (na rozšírenie)

---

### 4. Feed Composition

Finálny výber:

* mix tém
* mix perspektív
* limit počtu otázok

👉 výsledok: krátky, kvalitný batch

---

## Typy personalizácie

### 1. Engagement-first

Používateľ ešte nemá model:

* jednoduché otázky
* entry perspektívy

---

### 2. Learning phase

Model sa buduje:

* viac rôznych perspektív
* exploration

---

### 3. Optimization phase

Model je stabilný:

* vysoká relevancia
* presné otázky

---

## Exploration vs Exploitation

Systém musí balansovať:

* Exploitation → to čo používateľ chce
* Exploration → to čo ešte nepozná

Pravidlo:

> Exploration nikdy nesmie byť 0

---

## Diversity pravidlá

Feed musí obsahovať:

* rôzne témy
* rôzne perspektívy

❌ zakázané:

* monotematický feed
* jedna perspektíva stále dokola

---

## Anti-manipulation pravidlá

Systém NESMIE:

* tlačiť jeden názor
* skrývať alternatívy
* optimalizovať na "presvedčenie"

Systém MÔŽE:

* zvýrazniť relevantné
* ponúknuť iné pohľady

---

## Explainability

Každý výber musí vedieť vysvetliť:

Príklady:

* "Na základe tvojho záujmu"
* "Nová téma na rozšírenie"
* "Túto perspektívu si dlho nevidel"

---

## Personal AI vstupy

Personalizácia využíva:

* Interest Profile
* Preference Profile
* Participation Profile
* Confidence

---

## Riziká

### 1. Overfitting

➡️ používateľ vidí stále to isté

### 2. Underfitting

➡️ obsah je náhodný

### 3. Complexity overload

➡️ používateľ odchádza

---

## Pravidlá návrhu

1. Jedna otázka > 10 otázok
2. Odpoveď > perfektná otázka
3. Engagement > komplexnosť
4. Rozšírenie až po odpovedi

---

## Architektonické implikácie

### Services

* QuestionSelectionService
* PersonalizationEngine

---

### Model

* scoring layer
* ranking layer
* exploration controller

---

### UX

* 1–3 otázky na obrazovku
* možnosť "ďalší pohľad"

---

## Alternatívy

### Chronologický feed

❌ nulová hodnota

### Popularity-based feed

❌ social network problém

### Fully random

❌ slabý learning

---

## Väzby na ADR

* ADR-009 — Question Model
* ADR-010 — Perspective System
* ADR-006 — Learning Model

---

## Zhrnutie

Personalizácia v Synergetiku nie je o tom, čo je "správne".

Je o tom:

👉 čo spustí interakciu
👉 čo dá kvalitný signál
👉 čo postupne rozšíri myslenie

---

## Finálna veta

> Najlepšia otázka nie je tá najinteligentnejšia — ale tá, na ktorú človek odpovie.
