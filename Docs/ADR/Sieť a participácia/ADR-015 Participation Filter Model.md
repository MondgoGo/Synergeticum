# 🧾 ADR-029: Branch Lifecycle Model

---

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

Systém Personal AI Network reprezentuje projekty ako:

* distribuované objekty (ADR-023),
* s viacerými variantmi / vetvami (ADR-024),
* ktoré získavajú rôznu mieru podpory, zapojenia a pozornosti (ADR-028, ADR-015).

Prirodzeným dôsledkom je, že nie všetky vetvy projektu majú rovnakú životnosť.

Niektoré vetvy:

* rastú,
* získavajú ľudí,
* získavajú zdroje,
* a smerujú k realizácii.

Iné vetvy:

* strácajú podporu,
* prestávajú sa rozvíjať,
* ostávajú bez aktivity,
* a stávajú sa historickou stopou, nie aktívnou súčasťou projektu.

---

### Problém

Je potrebné definovať:

👉 **ako sa určuje stav vetvy v čase**
👉 **kedy sa vetva považuje za živú, slabnúcu alebo neaktívnu**
👉 **ako sa predchádza chaosu z nekonečného množstva starých alebo neudržiavaných vetiev**

---

### Kľúčová otázka

👉
**Ako má systém modelovať životný cyklus vetvy tak, aby bol prirodzený, decentralizovaný a nevyžadoval centrálny zásah?**

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Vetva je živá entita s vlastným životným cyklom

Každá vetva projektu je samostatná evolučná jednotka, ktorá:

* vzniká,
* rastie,
* mení sa,
* môže stagnovať,
* môže byť archivovaná,
* a môže byť znovu aktivovaná.

---

## 🔄 3.2 Vetva nemá binárny stav

Vetva NESMIE byť chápaná len ako:

* existuje / neexistuje

Namiesto toho má viac stavov.

---

## 🌱 3.3 Definované stavy vetvy

### 1. Proposed

Vetva bola vytvorená, ale ešte nemá dostatočnú podporu ani aktivitu.

Charakteristika:

* existuje návrh,
* minimálna alebo žiadna validácia,
* zatiaľ slabá interakcia.

Použitie:

* nové nápady,
* čerstvé odchýlky od existujúcej vetvy,
* experimentálne smery.

---

### 2. Active

Vetva je živá a rozvíjaná.

Charakteristika:

* má podporu,
* má aktivitu,
* prebieha okolo nej diskusia, úpravy alebo záväzky.

Použitie:

* relevantné a aktuálne varianty,
* vetvy, ktoré majú šancu postúpiť ďalej.

---

### 3. Stable

Vetva dosiahla určitú stabilitu.

Charakteristika:

* základný smer je ustálený,
* už neprechádza rýchlymi zmenami,
* stále má význam a podporu.

Použitie:

* zrelé varianty,
* vetvy vhodné pre širšie posúdenie alebo realizáciu.

---

### 4. Dormant

Vetva dočasne „spí“.

Charakteristika:

* nízka alebo nulová aktuálna aktivita,
* stále však má historický alebo potenciálny význam,
* nie je mŕtva, len neaktívna.

Použitie:

* vetvy bez aktuálneho ťahu,
* vetvy, ktoré môžu ožiť pri zmene okolností.

---

### 5. Archived

Vetva je historicky zachovaná, ale nie je aktívne súčasťou živého toku projektu.

Charakteristika:

* dlhodobo bez podpory a aktivity,
* nepovažuje sa za aktuálny smer,
* ostáva kvôli transparentnosti a možnosti obnovy.

Použitie:

* staré varianty,
* slepé vetvy,
* návrhy, ktoré stratili relevanciu.

---

### 6. Revived

Vetva, ktorá bola dormant alebo archived, bola znovu aktivovaná.

Charakteristika:

* znovu získala pozornosť,
* znovu sa objavila aktivita alebo commitment,
* znovu vstupuje do živého priestoru projektu.

Použitie:

* návrat starých myšlienok,
* zmena kontextu,
* oživenie alternatív.

---

## 📊 3.4 Stav vetvy je odvodený zo signálov, nie z autority

Stav vetvy nevzniká ručným rozhodnutím jednej osoby ani centrálneho systému.

Je odvodený z kombinácie signálov, najmä:

* úroveň podpory (alignment),
* úroveň záväzku (commitment),
* aktivita v čase,
* počet nových príspevkov a zmien,
* počet relevantných participantov,
* čas od poslednej významnej aktivity.

---

## ⚖️ 3.5 Nezáujem je legitímny mechanizmus zániku

Vetva „neumiera“ tým, že ju niekto zruší.

Vetva prirodzene slabne, ak:

* o ňu klesá záujem,
* nikto ju nerozvíja,
* nikto sa k nej nezaväzuje,
* nikomu už neprináša dostatočnú hodnotu.

👉
**Systém používa nezáujem ako primárny mechanizmus prirodzeného útlmu.**

---

## 🗃 3.6 Archivácia neznamená zmazanie

Archived vetva:

* sa nestráca,
* nevymazáva sa,
* ostáva prístupná v histórii,
* ostáva súčasťou lineage projektu.

To je dôležité z dôvodov:

* transparentnosti,
* spätnej vysvetliteľnosti,
* možnosti oživenia.

---

## 🔁 3.7 Oživenie vetvy je podporovaná vlastnosť

Vetva môže byť znovu aktivovaná, ak:

* sa k nej objaví nový alignment,
* vznikne nový commitment,
* zmení sa kontext,
* nové podmienky jej dávajú zmysel.

Systém teda musí rátať s tým, že „mŕtve“ vetvy môžu znovu získať život.

---

## 🌿 3.8 Vetva môže vytvárať ďalšie vetvy aj počas slabnutia

Aj dormant alebo archived vetva môže byť základom pre:

* nový branch,
* derivovaný variant,
* novú interpretáciu.

To znamená, že aj historicky slabá vetva môže mať vysokú genealogickú hodnotu.

---

## 🧠 3.9 Nie všetky vetvy musia prejsť rovnakými fázami

Niektoré vetvy:

* vzniknú a rýchlo zaniknú,
* nikdy nebudú stable,
* nikdy nebudú mať reálnu podporu.

Iné vetvy:

* môžu rýchlo prerásť do stable stavu,
* môžu preskočiť dlhú fázu experimentation.

Lifecycle model musí byť flexibilný, nie striktne lineárny.

---

## 📉 3.10 Ochrana pred zahltením systému

Aby systém zostal použiteľný:

* archived vetvy sa štandardne nezobrazujú medzi hlavnými aktívnymi smermi,
* dormant vetvy sa zobrazujú len vtedy, ak sú pre používateľa relevantné,
* active a stable vetvy majú prioritu vo View Layer (ADR-027).

Tým sa zachová:

* čitateľnosť,
* použiteľnosť,
* kontinuita histórie bez vizuálneho chaosu.

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitíva

### 🌱 Prirodzená evolúcia

Vetvy môžu rásť a slabnúť organicky bez potreby centrálneho rozhodovania.

### 🧹 Samočistenie systému

Nezaujímavé alebo neudržiavané smery sa postupne odsúvajú bez toho, aby bolo treba ich aktívne mazať.

### 🧾 Zachovanie histórie

Systém ostáva auditovateľný a vysvetliteľný.

### ♻️ Recyklácia myšlienok

Staré vetvy sa môžu vrátiť, ak znovu získajú zmysel.

---

## ❗ Negatíva / Trade-offs

### ⚙️ Vyššia zložitosť modelu

Lifecycle vetiev je bohatší než jednoduché „active/inactive“.

### 🧠 Potreba správneho view layer

Bez dobrej interpretácie sa používateľ v stave vetiev stratí.

### 📉 Nejednoznačné hranice

Nie vždy bude ostro jasné, kedy vetva prechádza medzi stavmi.

---

## 5. 🚫 Alternatívy (zamietnuté)

### ❌ Tvrdé mazanie vetiev

Zamietnuté, pretože:

* ničí históriu,
* ničí auditovateľnosť,
* znemožňuje revival.

### ❌ Vetvy bez lifecycle

Zamietnuté, pretože:

* vedie k zahlteniu,
* všetko ostáva „rovnako živé“,
* systém sa časom stane neprehľadný.

### ❌ Centrálne rušenie vetiev

Zamietnuté, pretože:

* porušuje decentralizáciu,
* zavádza autoritu tam, kde má rozhodovať dynamika siete.

---

## 6. 🔗 Väzby na ďalšie ADR

* ADR-022: Collective Project Formation
* ADR-023: Distributed Project Object Model
* ADR-024: Multi-Variant Project Governance
* ADR-027: Project View & Simplification Model
* ADR-028: Support & Alignment Model
* ADR-015: Participation Filter Model

---

## 7. 📏 Pravidlá implementácie (high-level)

Systém musí:

* uchovávať stav vetvy ako explicitnú vlastnosť,
* odvodzovať stav z viacerých signálov,
* podporovať archiváciu bez mazania,
* podporovať revival,
* prepojiť lifecycle vetvy s View Layer,
* umožniť lineage tracking medzi vetvami.

---

# 🔥 8. Zhrnutie

👉
**Vetva je živá projektová entita, ktorá má vlastný životný cyklus – môže vzniknúť, rásť, stabilizovať sa, uspať sa, byť archivovaná a znovu ožiť.**

---

# 🧭 Finálna veta

👉
**Vetvy nemajú byť rušené silou – majú prirodzene slabnúť, ak ich už nikto neživí.**
