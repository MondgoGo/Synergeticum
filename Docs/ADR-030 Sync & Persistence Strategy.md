Áno. Toto je presne ten ADR, ktorý má ochrániť projekt pred technickou samovraždou.

Kľúč je nepokúsiť sa hneď stavať „dokonalý decentralizovaný sync engine“, ale zvoliť takú stratégiu, ktorá:

* rešpektuje víziu,
* sedí na P2P a local-first princípoch,
* ale dá sa postaviť po vrstvách.

Nižšie máš návrh.

---

# 🧾 ADR-030: Sync & Persistence Strategy

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

Synergetikum pracuje s týmito základnými objektmi:

* Personal AI modely,
* otázky a odpovede,
* distribuované projekty,
* vetvy projektov,
* argumenty,
* support signály,
* view projekcie.

Tieto objekty existujú v prostredí, ktoré má nasledujúce vlastnosti:

* systém je peer-to-peer,
* uzly môžu byť offline,
* mobil je primárne zariadenie,
* parent node je voliteľný,
* používateľské dáta majú zostať lokálne,
* projektové objekty sa vyvíjajú v čase,
* konflikty nemajú končiť chybou, ale vetvením.

---

### Problém

Je potrebné definovať:

👉 **ako budú objekty systému ukladané, synchronizované a udržiavané v čase**

tak, aby:

* systém ostal decentralizovaný,
* bol použiteľný na mobile,
* bol odolný voči offline režimu,
* a nevyžadoval vlastný extrémne komplikovaný sync engine od nuly.

---

### Kľúčová otázka

👉
**Aký model synchronizácie a perzistencie má Synergetikum použiť, aby bol realisticky implementovateľný a zároveň kompatibilný s víziou systému?**

---

## 3. ⚖️ Rozhodnutie

Synergetikum použije:

👉 **hybridnú local-first sync & persistence stratégiu**

Tento model kombinuje:

* **lokálny stav** ako primárny zdroj používateľskej skúsenosti,
* **distribuovanú synchronizáciu zmien** medzi relevantnými uzlami,
* **snapshoty a históriu** pre audit, vetvenie a rekonštrukciu,
* a **branching namiesto tvrdého konfliktu**.

---

## 4. 🧠 Hlavný princíp

Systém nebude stavať na:

❌ jednom centrálnom úložisku
❌ jednej „master“ verzii projektu
❌ plnom Git modeli ako jadre runtime
❌ vlastnom sync engine od nuly v prvej fáze

Namiesto toho bude stáť na tomto princípe:

👉
**každý uzol drží svoj lokálny stav, synchronizujú sa len relevantné zmeny, a história sa uchováva oddelene od živého runtime stavu**

---

## 5. 🧩 Architektonické vrstvy perzistencie

### 5.1 Local Runtime State

Každý uzol drží lokálny pracovný stav objektov, ktoré sú preň relevantné.

To znamená:

* používateľ má lokálnu kópiu svojich otázok,
* lokálnu interpretáciu projektov,
* lokálny stav relevantných vetiev,
* lokálny stav support signálov,
* lokálny stav participácie.

Táto vrstva je:

* rýchla,
* offline-first,
* optimalizovaná pre používateľskú skúsenosť.

👉 Toto je hlavný zdroj pravdy pre UX daného používateľa.

---

### 5.2 Sync Event Layer

Medzi uzlami sa neposielajú celé objekty pri každej zmene.

Synchronizujú sa prednostne:

* udalosti,
* zmeny,
* delty,
* nové vetvy,
* aktualizácie supportu,
* nové argumenty,
* zmeny lifecycle stavu.

Táto vrstva je:

* ľahšia než posielanie celých snapshotov,
* prirodzenejšia pre offline P2P,
* vhodná pre postupnú rekonštrukciu stavu.

👉 Sieťou má tiecť zmena, nie celý svet.

---

### 5.3 Derived State Layer

Aktuálny stav projektu alebo vetvy sa neukladá len ako „ručný dokument“, ale sa **odvodzuje** zo série zmien a lokálne sa materializuje do použiteľného snapshotu.

To znamená:

* eventy sú zdroj evolúcie,
* odvodený stav je zdroj UX.

👉 Systém žije na aktuálnych snapshotových stavoch, ale vie ich spätne vysvetliť cez históriu.

---

### 5.4 Snapshot & Audit Layer

Významné stavy objektov sa ukladajú ako snapshoty.

Používajú sa na:

* zrýchlenie rekonštrukcie,
* audit,
* rollback,
* branching,
* obnovu po dlhšom offline režime.

Táto vrstva nie je primárny runtime mechanizmus, ale stabilizačná vrstva systému.

👉 Nie všetko sa má počítať z nekonečného streamu od začiatku.

---

## 6. 🔄 Sync model

### 6.1 Local-first ako default

Každý uzol musí vedieť fungovať plnohodnotne aj bez siete.

To znamená:

* používateľ vie čítať a reagovať aj offline,
* jeho Personal AI vie lokálne vyhodnocovať a filtrovať,
* lokálne zmeny sa zaznamenávajú bez potreby okamžitého potvrdenia zo siete.

---

### 6.2 P2P sync ako eventual process

Synchronizácia prebieha:

* keď sú uzly dostupné,
* keď sú relevantné,
* keď Participation Filter dovolí výmenu,
* keď sú splnené podmienky siete a zariadenia.

Systém predpokladá:

👉 **eventual consistency**, nie okamžitú globálnu synchronizáciu.

---

### 6.3 Konflikt ≠ chyba

Ak sa na dvoch uzloch objaví:

* nekompatibilná zmena,
* zásadne odlišný smer,
* odlišná interpretácia projektu,

systém to nemá chápať ako chybu synchronizácie.

Má to chápať ako:

👉 **signál pre branching**

Toto je zásadné.

---

## 7. 🌳 Branching ako sync safety valve

Branching nie je len governance funkcia.
Je to aj technický mechanizmus ochrany systému.

Pomáha zabrániť tomu, aby:

* konfliktné zmeny rozbili projekt,
* používatelia museli robiť tvrdé merge rozhodnutia,
* sync končil neúspechom.

👉 Pri vysokom konflikte sa objekt nerozbije. Rozdelí sa.

---

## 8. 🧱 Prečo nie čistý Git model

Git-like myslenie je užitočné pre:

* históriu,
* vetvenie,
* snapshoty,
* lineage.

Ale Git nie je vhodný ako celý runtime model systému, pretože:

* je príliš rigidný pre živý kolaboratívny objekt,
* prirodzene vedie k manuálnemu merge mindsetu,
* je príliš „developer-native“,
* nehodí sa ako hlavný interakčný model pre mobile-first systém.

👉 Git-like princípy budú použité skôr v audit/snapshot vrstve než v každodennom live sync jadre.

---

## 9. 🧠 Prečo nie vlastný sync engine od nuly

Vlastný sync engine od nuly je architektonicky lákavý, ale prakticky veľmi rizikový.

Riziká:

* extrémna komplexita,
* dlhý čas do prvého použiteľného výsledku,
* vysoká pravdepodobnosť chýb v konflikte, offline režime a merge,
* odčerpanie energie z hlavnej hodnoty produktu.

👉 Sync má byť infra vrstva, nie hlavný spotrebiteľ celej firmy.

---

## 10. ✅ Preferovaný technický smer

Na úrovni princípu sa rozhodujeme pre:

👉 **local-first distribuovaný objektový model s eventovým syncom a snapshotmi**

To znamená:

* runtime stav je lokálny,
* zmeny sú eventy,
* významné body sú snapshoty,
* vetvenie rieši konflikty,
* používateľ pracuje so snapshot/view vrstvou, nie s raw syncom.

Tento smer je bližší local-first / CRDT svetu než tradičnému Git-only svetu. CRDT frameworky ako Yjs alebo C# orientované knižnice typu Harmony ukazujú, že offline-first kolaboratívne objekty sú reálny smer, aj keď nie všetko treba prebrať 1:1.

---

## 11. 🪜 Fázovanie implementácie

Aby nás to „nezabilo“, sync stratégia musí mať fázy.

### Fáza 1 – Single-node truth with export/import sync

* projekt vzniká lokálne,
* zmeny sú zaznamenávané ako eventy,
* snapshoty existujú,
* sync je ešte jednoduchý a obmedzený.

Cieľ:

* overiť Project Object Model,
* overiť event model,
* overiť vetvenie.

---

### Fáza 2 – Trusted-circle sync

* relevantné uzly si vymieňajú eventy,
* rekonštruujú lokálny stav,
* vzniká reálny distributed object model.

Cieľ:

* overiť P2P synchronizáciu bez masívnej siete.

---

### Fáza 3 – Parent node acceleration

* parent node pomáha s:

  * dlhšou históriou,
  * snapshotmi,
  * kompakciou eventov,
  * rýchlejšou rekonštrukciou.

Cieľ:

* odľahčiť mobil,
* zvýšiť robustnosť.

---

### Fáza 4 – Pokročilé merge / CRDT-like správanie

* zjemnenie konfliktov,
* automatickejšie zlučovanie neproblematických zmien,
* sofistikovanejší distributed sync.

Cieľ:

* znížiť trenie,
* ale až po tom, čo core model funguje.

---

## 12. 📊 Dôsledky rozhodnutia

### ✅ Pozitíva

#### 12.1 Realistická implementovateľnosť

Systém nevyžaduje dokonalý decentralizovaný engine v prvej verzii.

#### 12.2 Offline-first kompatibilita

Používateľ môže normálne fungovať aj bez siete.

#### 12.3 Vysoká kompatibilita s vetvením

Konflikty nevedú k zlyhaniu, ale k novým variantom.

#### 12.4 Auditovateľnosť

Snapshoty a eventy umožňujú spätné vysvetlenie.

---

### ❗ Negatíva / Trade-offs

#### 12.5 Vyššia architektonická komplexita než centrálne úložisko

Je to cena za decentralizáciu.

#### 12.6 Eventual consistency

Uzly nemusia mať okamžite ten istý stav.

#### 12.7 Potreba kvalitného View Layer

Bez dobrého zjednodušenia bude distribuovaný projekt pre používateľa stále nepriehľadný.

---

## 13. 🚫 Alternatívy (zamietnuté)

### ❌ Centrálna databáza ako hlavný zdroj pravdy

Zamietnuté, pretože porušuje decentralizačný princíp a ownership model.

### ❌ Čistý Git ako runtime jadro

Zamietnuté, pretože je vhodnejší na audit, verzie a históriu než na živý kolaboratívny objekt.

### ❌ Full blockchain persistence

Zamietnuté, pretože je príliš rigidná, drahá a nevhodná pre editovateľný projektový objekt.

### ❌ Vlastný sync engine od nuly od prvej iterácie

Zamietnuté, pretože by dramaticky zvýšil riziko projektu.

---

## 14. 🔗 Väzby na ďalšie ADR

* ADR-014: Network Communication Model
* ADR-015: Participation Filter Model
* ADR-022: Collective Project Formation
* ADR-023: Distributed Project Object Model
* ADR-024: Multi-Variant Project Governance
* ADR-027: Project View & Simplification Model
* ADR-029: Branch Lifecycle Model

---

## 15. 📏 Pravidlá implementácie (high-level)

Systém musí:

* držať runtime stav lokálne,
* synchronizovať zmeny, nie celé objekty,
* vedieť rekonštruovať stav zo zmien,
* používať snapshoty pre výkon a audit,
* riešiť konflikty branchingom,
* fungovať offline-first,
* byť zavádzaný po fázach.

---

# 🔥 16. Zhrnutie

👉
**Synergetikum použije hybridný local-first model, v ktorom projekty žijú ako distribuované objekty, zmeny sa synchronizujú ako eventy, snapshoty stabilizujú históriu a konflikty sa riešia vetvením.**

---

# 🧭 Finálna veta

👉
**Nesnažíme sa postaviť dokonalý sync engine od prvého dňa.
Staviame systém, ktorý vie rásť od jednoduchého lokálneho stavu k distribuovanému živému objektu bez toho, aby nás zabil vlastnou infraštruktúrou.**
