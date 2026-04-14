# ADR-037 — Identity Recovery & Continuity Model

## Status

Proposed

---

## Kontext

Synergetikum je systém, v ktorom Personal AI model predstavuje:

* digitálnu identitu používateľa,
* jeho rozhodovacie vzorce,
* jeho preferencie, hodnoty a vývoj v čase.

Na rozdiel od bežných aplikácií:

> strata dát ≠ strata súboru
> strata dát = strata časti „digitálneho ja“

To vytvára zásadný problém:

* používateľ môže stratiť zariadenie,
* môže dôjsť k poškodeniu dát,
* môže dôjsť k strate prístupu.

Bez riešenia:

> používateľ nebude ochotný investovať energiu do budovania svojho modelu.

Tento problém nie je primárne technický, ale:

> **psychologický a existenčný pre dôveru v systém**

---

## Rozhodnutie

Synergetikum implementuje princíp:

> **Digitálna identita musí byť obnoviteľná bez narušenia dôvery, súkromia a vlastníctva.**

To znamená:

* používateľ má možnosť obnoviť svoj Personal AI model,
* bez potreby centrálnej autority,
* bez kompromitovania súkromia,
* bez straty kontroly nad dátami.

---

## Kľúčový princíp

> **Identity Recovery ≠ Backup**

Backup je:

* technická kópia dát

Identity Recovery je:

* zachovanie kontinuity identity používateľa

---

## Core vlastnosť systému

> **Perceived Continuity of Self**

Používateľ musí veriť, že:

* jeho digitálna identita pretrvá,
* môže ju obnoviť,
* nestratí „seba“ v systéme.

Bez tejto vlastnosti:

* nevznikne dôvera,
* nevznikne dlhodobá participácia.

---

## Mechanizmus (principiálny, nie implementačný)

### 1. Local-first základ

* primárny model je vždy lokálny (mobile / desktop)

---

### 2. Distributed encrypted backup

Obnova identity je umožnená cez:

* **parent node** (osobné zariadenie používateľa),
* **trusted circle** (vybraní používatelia),
* alebo kombináciu oboch.

---

### 3. Split knowledge (secret distribution)

* žiadna entita nemá kompletný prístup,
* obnova vyžaduje kombináciu častí,
* minimalizuje riziko zneužitia.

---

### 4. User-controlled recovery

* iniciácia obnovy je vždy na strane používateľa,
* systém nikdy automaticky neobnovuje identitu bez jeho vedomia.

---

## Čo systém nerobí

Systém NEPOUŽÍVA:

* centralizovaný cloud ako jediný zdroj pravdy,
* nešifrované zálohy,
* automatické zdieľanie identity,
* obnovu bez explicitného súhlasu používateľa.

---

## Dôsledky

### Pozitívne:

* vysoká dôvera používateľov,
* ochota investovať čas do systému,
* stabilita identity v čase.

---

### Negatívne:

* vyššia komplexita systému,
* nutnosť vysvetliť používateľovi recovery mechanizmus,
* potenciálne zložitejší onboarding.

---

## Riziká

### 1. Strata recovery prístupov

Používateľ môže stratiť všetky recovery možnosti.

Mitigácia:

* viacero recovery ciest,
* upozornenia na nastavenie recovery,
* jednoduché UX pre konfiguráciu.

---

### 2. Zneužitie trusted circle

Ak dôjde k kompromitácii dôveryhodných uzlov.

Mitigácia:

* šifrovanie,
* rozdelenie prístupu,
* možnosť revokácie.

---

### 3. Používateľ nepochopí systém

Recovery môže byť príliš komplexné.

Mitigácia:

* veľmi jednoduché UI,
* postupné vysvetlenie,
* default bezpečné nastavenia.

---

## Architektonické implikácie

### Personal AI

* musí byť serializovateľná a rekonštruovateľná

### Storage

* musí podporovať čiastočné a šifrované uloženie

### Sync Layer

* musí vedieť distribuovať časti bez kompromitácie celku

### UX Layer

* musí vysvetliť recovery bez technických detailov

---

## Alternatívy

### 1. Central cloud backup

Zamietnuté.

Dôvod:

* porušuje ownership a privacy princípy

---

### 2. Žiadna obnova

Zamietnuté.

Dôvod:

* nulová dôvera
* nulová adopcia

---

### 3. Manuálny export/import

Čiastočne prijaté ako doplnok.

---

## Väzby na ďalšie ADR

* ADR-001 — Personal AI Ownership Model
* ADR-004 — Privacy-first Architecture
* ADR-014 — P2P Communication Model
* ADR-036 — Reciprocity Engine

---

## Zhrnutie

Identity Recovery je:

* základ dôvery,
* základ dlhodobého používania,
* základ investície používateľa do systému.

Bez nej:

> Synergetikum neprežije adopciu.

---

## Finálna veta

> Používateľ nebude budovať digitálnu identitu, ak si nie je istý, že ju nikdy nestratí.

---
