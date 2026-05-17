# ADR-005 — Personal AI Model Definition

## Status

Proposed

## Kontext

Synergetikum je postavené na koncepte osobnej digitálnej inteligencie (Personal AI), ktorá reprezentuje používateľa v digitálnom priestore.

Na rozdiel od klasických AI systémov:

* nejde o generickú AI,
* nejde o centrálny model,
* nejde o chatbot.

Ide o **osobný model**, ktorý sa učí výhradne z konkrétneho používateľa a slúži jemu.

Bez presnej definície Personal AI hrozí:

* nejasná architektúra,
* miešanie UI, AI a dát,
* neskoršie problémy so škálovaním,
* nepochopenie systému tímom.

Preto je potrebné presne definovať, čo Personal AI je a čo nie je.

---

## Rozhodnutie

V systéme Synergetikum platí:

> Personal AI je lokálny, evolučný, vysvetliteľný model používateľa, ktorý reprezentuje jeho preferencie, záujmy, schopnosti a vzorce rozhodovania.

Personal AI:

* existuje per používateľ,
* je uložená primárne lokálne,
* je dynamická (mení sa v čase),
* je vysvetliteľná (nie black-box),
* je rozdelená na viacero sub-modelov.

---

## Štruktúra Personal AI

Personal AI nie je jeden objekt, ale kompozícia:

### 1. Interest Profile

Reprezentuje:

* o čo sa používateľ zaujíma
* ako silno
* ako sa to mení v čase

### 2. Capability Profile

Reprezentuje:

* v čom vie používateľ prispieť
* aké má schopnosti
* odhadované na základe správania

### 3. Preference Profile

Reprezentuje spôsob rozhodovania:

* praktický vs konceptuálny
* lokálny vs systémový
* akcia vs analýza
* individuálne vs kolektívne

### 4. Participation Profile

Reprezentuje:

* ako sa používateľ zapája
* či odpovedá, diskutuje alebo koná
* mieru záväzku

### 5. Cognitive Modes Distribution

Reprezentuje:

* rozloženie typov myslenia
* nie ako kategóriu, ale ako distribúciu

⚠️ Dôležité:
Toto nie je hodnotenie človeka, ale model správania.

### 6. Event Log

Záznam:

* všetkých odpovedí
* zmien profilu
* rozhodnutí

Používa sa na:

* audit
* rekonštrukciu
* učenie

### 7. Snapshot

Agregovaný stav:

* aktuálny profil
* pripravený pre UI

---

## Vstupy do Personal AI

Personal AI sa učí z:

* odpovedí na otázky
* ignorovania otázok
* času reakcie
* typu odpovede
* zapojenia do tém

Nie z:

* skrytého sledovania
* externých dát bez súhlasu

---

## Výstupy Personal AI

Personal AI ovplyvňuje:

* poradie otázok
* výber tém
* perspektívy
* odporúčania
* reflexiu

Nevytvára:

* rozhodnutia za používateľa
* pravdu

---

## Dôsledky

* systém je vysvetliteľný
* dá sa debugovať
* dá sa testovať
* dá sa rozširovať

---

## Architektonické implikácie

### Modularita

Každá časť profilu je oddelená.

### Event-driven learning

Profil sa aktualizuje na základe eventov.

### Snapshot layer

UI nikdy nečíta raw eventy, ale snapshot.

### Local-first

Profil je uložený lokálne.

---

## Alternatívy

### Jeden black-box model

Zamietnuté.

Dôvod:

* nevysvetliteľné
* neudržateľné

### Čisto rule-based systém bez modelu

Zamietnuté.

Dôvod:

* nedokáže sa adaptovať

---

## Väzby na ďalšie ADR

* ADR-001 — Personal AI Ownership Model
* ADR-002 — Human-in-the-loop Decision Model
* ADR-006 — Learning Model
* ADR-007 — User Profile Representation
* ADR-034 — Personal AI Internal Model

---

## Zhrnutie

Personal AI je jadro systému.

Nie je to chatbot.
Nie je to black box.

Je to živý model používateľa.

---

## Finálna veta

> Personal AI je digitálne zrkadlo používateľa, ktoré sa učí z jeho rozhodnutí, ale nikdy ho nenahrádza.
