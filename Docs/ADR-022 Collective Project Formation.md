# 🧾 ADR-022: Collective Project Formation

---

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

Systém Personal AI Network umožňuje:

* kladenie otázok
* kolektívne odpovede (Collective Query – ADR-016)
* simuláciu scenárov (ADR-018)
* identifikáciu zapojenia (ADR-019)

---

Doteraz systém pokrýva:

👉 myslenie, diskusiu, perspektívy

---

### Problém

V systéme vznikajú situácie, kde:

* viac používateľov prejaví záujem o rovnakú vec
* existujú konkrétne návrhy riešení
* objavujú sa ľudia, ktorí vedia prispieť

---

👉 ale systém nemá mechanizmus, ktorý by:

* tieto signály zachytil
* štrukturoval
* transformoval na realizovateľný projekt

---

### Kľúčová otázka

👉
**Ako transformovať kolektívne odpovede a záujem na konkrétny projekt bez centrálneho riadenia?**

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Zavedenie „Project Formation Layer“

Systém bude obsahovať vrstvu:

👉 **Collective Project Formation Layer**

---

Táto vrstva:

* deteguje vznikajúce zámery
* vytvára projektové entity
* umožňuje ich evolúciu

---

---

## 📄 3.2 Entita: Living Project Document

---

Systém zavádza novú entitu:

👉 **Living Project Document (LPD)**

---

### Definícia

Dynamický, kolektívne vytváraný objekt, ktorý:

* vzniká z diskusie
* priebežne sa aktualizuje
* reprezentuje projekt

---

---

## 🔄 3.3 Lifecycle projektu

---

### Fáza 1: Signal Detection

Systém deteguje:

* opakujúce sa témy
* vysoký záujem
* konkrétne návrhy

---

👉 identifikuje:

**potenciálny projekt**

---

---

### Fáza 2: Initialization

Systém vytvorí:

👉 základný projektový záznam

---

Obsah:

* názov
* kontext
* pôvodná otázka

---

---

### Fáza 3: Structuring

Systém extrahuje z odpovedí:

---

#### 📌 Problém

čo chýba / čo treba riešiť

---

#### 📌 Navrhované riešenie

čo sa navrhuje

---

#### 📌 Dopyt

koľko ľudí prejavilo záujem

---

#### 📌 Zdroje

ľudia, ktorí sa chcú zapojiť

---

#### 📌 Riziká

identifikované problémy

---

---

### Fáza 4: Human Validation

Používatelia môžu:

* potvrdiť obsah
* upraviť
* rozšíriť

---

👉 človek má finálne slovo

---

---

### Fáza 5: Activation

Projekt sa aktivuje, ak:

* dosiahne dostatočný záujem
* existujú zdroje
* existuje iniciátor

---

---

### Fáza 6: Execution (mimo core systému)

Systém:

* môže podporovať koordináciu

ALE:

👉 neriadi realizáciu

---

---

## 🧠 3.4 Typy projektových dokumentov

---

### 🟢 Insight Document

* sumarizácia diskusie

---

### 🔵 Proposal Document

* návrh riešenia

---

### 🔴 Project Document

* konkrétny projekt

---

---

## 🤝 3.5 Prepojenie s používateľmi

---

Každý používateľ vidí projekt:

👉 cez svoju Personal AI

---

Systém mu navrhne:

* ako sa môže zapojiť
* akú rolu môže mať

---

---

## 🔄 3.6 Evolúcia projektu

---

Living Project Document:

* sa mení v čase
* môže rásť
* môže zaniknúť

---

👉 nie je statický

---

---

## 🚫 3.7 Bez centrálneho riadenia

---

Projekt NESMIE byť:

* centrálne schvaľovaný
* riadený autoritou systému

---

👉 vzniká emergentne

---

---

## 🔐 3.8 Ochrana súkromia

---

Pri tvorbe projektu:

* sa používajú anonymizované vstupy
* Personal AI neposiela surové dáta
* identita používateľa je chránená

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitíva

---

### 🔗 Prepojenie ľudí

* vznik spolupráce

---

### ⚙️ Transformácia myslenia na akciu

* systém má reálny dopad

---

### 📈 Vznik iniciatív

* projekty vznikajú prirodzene

---

---

## ❗ Negatíva / Trade-offs

---

### ⚠️ Falošné projekty

* veľa návrhov bez realizácie

---

### 📉 Overload

* príliš veľa projektov

---

### 🧩 Koordinačný chaos

* nejasné role

---

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Manuálne zakladanie projektov

* neškálovateľné
* nevyužíva kolektívnu inteligenciu

---

---

### ❌ Centrálne kurátorovaný systém

* kontrolovaný
* ale:

  * strata decentralizácie

---

---

## 6. 🔗 Väzby na ďalšie ADR

---

* ADR-016 (Collective Query)
* ADR-019 (Opportunity Engine)
* ADR-014 (P2P Network)
* ADR-011 (Personalization)

---

---

## 7. 📏 Pravidlá implementácie (high-level)

---

Systém musí:

* detegovať vznikajúce zámery
* extrahovať štruktúrované informácie
* umožniť editáciu používateľmi
* definovať threshold pre aktiváciu

---

---

# 🔥 8. Zhrnutie

---

👉
**Systém transformuje kolektívne odpovede na živé projektové dokumenty, ktoré môžu viesť k reálnym aktivitám a spolupráci.**

---

---

# 🧭 Finálna veta

👉
**Ak otázky generujú myslenie, Project Formation generuje realitu.**

---

