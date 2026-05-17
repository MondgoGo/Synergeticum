# ADR-010 — Perspective System (User-centric & Practical)

## Status

Proposed

---

## Kontext

V predchádzajúcich ADR sme definovali:

* otázky ako základ interakcie (ADR-009),
* Personal AI profil (ADR-005, ADR-007),
* learning model (ADR-006).

Z pohľadu reality (pilot, produkt, adopcia) vzniká zásadná otázka:

> Ako zabezpečiť, aby otázky neboli len „dotazník“, ale nástroj, ktorý reálne pomáha používateľovi premýšľať a konať?

Ak systém používa iba jednu formuláciu otázky:

* niektorí ľudia jej nerozumejú,
* niektorých nezaujme,
* niektorých odradí,
* a systém stráca engagement aj kvalitu dát.

---

## Rozhodnutie

> Každá téma musí byť reprezentovaná viacerými perspektívami, ktoré umožnia rôznym typom používateľov pochopiť, zapojiť sa a reagovať.

Zásadný posun oproti pôvodnému návrhu:

👉 Perspektívy NIE sú filozofická vrstva
👉 Perspektívy sú **praktický nástroj na zvýšenie engagementu + kvality signálov**

---

## Cieľ systému (nový pohľad)

Perspective System musí zabezpečiť:

1. Používateľ rozumie otázke
2. Používateľ má chuť odpovedať
3. Používateľ vidí zmysel
4. Systém získa kvalitný signál

Ak toto neplatí:
👉 celý systém zlyhá

---

## Core princíp

> Jedna téma → viac vstupných bodov pre rôzne typy ľudí

Nie preto, aby sme analyzovali realitu.

Ale preto, aby sa **čo najviac ľudí vedelo zapojiť**.

---

## Praktický príklad

Core problém:

> "Má zmysel spraviť pizzériu v tejto oblasti?"

---

### Perspektíva 1 — Jednoduchá (entry point)

* "Dal by si si tu pizzu?"

➡️ maximálny engagement
➡️ low barrier

---

### Perspektíva 2 — Praktická

* "Chýba ti tu takáto služba?"

➡️ potreba

---

### Perspektíva 3 — Akčná

* "Zapojil by si sa do vytvorenia?"

➡️ transformácia na projekt

---

### Perspektíva 4 — Analytická

* "Je tu dostatočný dopyt?"

➡️ kvalitné rozhodovanie

---

### Perspektíva 5 — Systémová

* "Ako to ovplyvní komunitu?"

➡️ širší dopad

---

## Kľúčový insight

👉 Perspektívy NIE sú o úrovniach ľudí
👉 Perspektívy sú o **rôznych vstupných bodoch do tej istej reality**

---

## Typy perspektív (prakticky)

### 1. Entry (najdôležitejšia)

* jednoduchá
* intuitívna
* rýchla

👉 bez tejto perspektívy systém zomrie

---

### 2. Practical

* čo to znamená pre mňa

---

### 3. Action

* čo s tým viem spraviť

---

### 4. Analytical

* ako to funguje

---

### 5. Systemic

* aký je dopad

---

### 6. Reflective

* čo je pre mňa dôležité

---

## Personalizácia perspektív

Systém robí:

* vyberie prvú perspektívu (najpravdepodobnejšia odpoveď)
* ale ponechá ostatné dostupné

👉 Personalizácia = poradie
👉 nie filtrácia

---

## Perspektívy a learning

Rôzne perspektívy dávajú rôzne typy signálov:

| Perspektíva | Typ signálu |
| ----------- | ----------- |
| Entry       | engagement  |
| Practical   | potreba     |
| Action      | commitment  |
| Analytical  | racionalita |
| Systemic    | komplexnosť |

👉 spolu tvoria kompletný obraz

---

## Riziká (nový pohľad)

### 1. Príliš komplexné otázky

➡️ nikto neodpovedá

### 2. Príliš veľa perspektív

➡️ chaos

### 3. Chýbajúci entry point

➡️ systém nemá používateľov

---

## Pravidlá návrhu

1. Každá téma MUSÍ mať jednoduchý entry point
2. Perspektívy sa pridávajú postupne
3. Používateľ nikdy nevidí všetko naraz
4. Systém preferuje odpoveď pred komplexnosťou

---

## Architektonické implikácie

### Model

* CoreTopic
* PerspectiveVariant

---

### Selection

* vyber perspektívy na základe:

  * profilu
  * jednoduchosti
  * engagementu

---

### UX

* 1 otázka naraz
* možnosť „pozrieť iný pohľad"

---

## Alternatívy

### Jedna otázka

❌ slabý engagement

### Všetky perspektívy naraz

❌ overload

---

## Väzby na ADR

* ADR-009 — Question Model
* ADR-011 — Personalization
* ADR-006 — Learning

---

## Zhrnutie

Perspective System nie je filozofická vrstva.

Je to:

👉 mechanizmus, ako dostať používateľa do interakcie
👉 mechanizmus, ako získať kvalitné dáta
👉 mechanizmus, ako premeniť otázku na akciu

---

## Finálna veta

> Perspektívy v Synergetiku nie sú o tom, ako sa správne pozerať na svet — ale o tom, aby sa doň vedel zapojiť každý.
