# 🧾 ADR-001: Personal AI Ownership Model

---

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

Navrhovaný systém Personal AI Network je postavený na koncepte:

👉 každý používateľ má vlastnú digitálnu inteligenciu (Personal AI)

Táto inteligencia:

* reprezentuje jeho správanie, preferencie a spôsob myslenia
* učí sa z jeho interakcií
* ovplyvňuje spôsob, akým systém prezentuje informácie

---

### Problém

V tradičných systémoch:

* AI modely sú centrálne (server-side)
* používateľ nemá kontrolu nad dátami ani modelom
* dochádza k:

  * strate súkromia
  * netransparentnosti
  * strate dôvery

---

### Kľúčová otázka

👉
**Kto vlastní a kontroluje Personal AI model?**

---

## 3. ⚖️ Rozhodnutie

Systém bude implementovaný tak, že:

---

## 🔒 3.1 Vlastníctvo

👉
**Každý Personal AI model patrí výhradne používateľovi.**

---

## 📱 3.2 Lokalita modelu

Personal AI bude:

* primárne bežať na **mobilnom zariadení používateľa**
* voliteľne rozšírená o **desktopový (parent) node**

---

## 🚫 3.3 Neexistuje centrálny model používateľa

Systém NESMIE:

* uchovávať kompletný profil používateľa na serveri
* mať prístup k internému modelu bez súhlasu

---

## 🔐 3.4 Dáta ostávajú lokálne

* všetky citlivé dáta zostávajú na zariadení
* učenie modelu prebieha lokálne

---

## 🌐 3.5 Sieťová komunikácia

Pri komunikácii medzi modelmi:

* neprebieha zdieľanie surových dát
* zdieľajú sa len:

  * odpovede
  * agregované informácie
  * anonymizované výstupy

---

## ⚙️ 3.6 Používateľ má kontrolu

Používateľ musí mať možnosť:

* zapnúť / vypnúť sieťovú participáciu
* definovať rozsah zdieľania
* spravovať svoje dáta

---

## 🧠 3.7 Model ako reprezentácia, nie identita

Personal AI:

* reprezentuje správanie používateľa
* nie je jeho definitívna identita
* je dynamická a meniteľná

## 3.8 Distribuovaná architektúra (P2P kontext)

Systém je navrhnutý ako:

👉 peer-to-peer sieť Personal AI modelov

To znamená:

* modely komunikujú priamo medzi sebou
* neexistuje centrálna inteligencia ani centrálne riadenie
* komunikácia prebieha len za podmienok:
* relevancie
* povolenia používateľa
* lokálneho rozhodnutia modelu

### detailná špecifikácia je definovaná v:

* ADR-014 (Network Communication Model)
* ADR-015 (Participation Filter)
* ADR-016 (Collective Query System)

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitívne

### 🔒 Súkromie

* vysoká ochrana dát

---

### 🤝 Dôvera

* používateľ má kontrolu

---

### 🧩 Modularita

* systém je distribuovaný

---

### ⚖️ Etická čistota

* žiadne centrálne profilovanie

---

---

## ❗ Negatívne / Trade-offs

---

### ⚙️ Výpočtová náročnosť

* model beží na zariadení

---

### 🔄 Synchronizácia

* potreba riešiť mobil ↔ desktop

---

### 🌐 Komplexita siete

* agregácia bez dát je náročnejšia

---

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Centrálna AI architektúra

* model na serveri
* jednoduchšia implementácia
* ale:

  * strata súkromia
  * strata dôvery

---

---

### ❌ Hybridný model (server dominantný)

* časť modelu na serveri
* časť lokálne

→ zamietnuté kvôli:

* nejasnému vlastníctvu
* bezpečnostným rizikám

---

---

## 6. 🔗 Väzby na ďalšie ADR

---

Tento ADR ovplyvňuje:

* ADR-005 (Personal AI Model Definition)
* ADR-006 (Learning Model)
* ADR-014 (Network Communication)
* ADR-021 (Parent Node Architecture)

---

---

## 7. 📏 Pravidlá implementácie (high-level)

---

Systém musí:

* umožniť lokálne učenie modelu
* minimalizovať prenos dát
* zabezpečiť anonymizáciu komunikácie
* umožniť používateľovi kontrolu

---

---

# 🔥 8. Zhrnutie

---

👉
**Personal AI je lokálny, súkromný model, ktorý patrí používateľovi a nikdy nie je centrálne kontrolovaný.**

---

---

# 🧭 Finálna veta

👉
**Bez vlastníctva modelu používateľom systém stráca dôveru – a tým aj svoj zmysel.**

---

