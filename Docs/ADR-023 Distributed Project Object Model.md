# 🧾 ADR-023: Distributed Project Object Model

---

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

Systém Personal AI Network:

* je peer-to-peer (ADR-014)
* obsahuje Personal AI modely (ADR-001)
* umožňuje vznik projektov (ADR-022)
* podporuje vetvenie (ADR-024)
* používa View Layer (ADR-027)

---

Projekt (Living Project Document):

* má viacero variantov
* má históriu
* má účastníkov
* sa vyvíja v čase

---

### Problém

Je potrebné definovať:

👉 **kde projekt existuje a ako je udržiavaný v P2P sieti**

---

Kľúčové otázky:

* Je projekt viazaný na jedno zariadenie?
* Ako prežije odchod iniciátora?
* Ako sa synchronizujú zmeny?
* Kto má „pravdu“ pri konfliktoch?

---

## 3. ⚖️ Rozhodnutie

---

## 🌐 3.1 Projekt ako distribuovaný objekt

👉
**Projekt je distribuovaný objekt existujúci v P2P sieti.**

---

To znamená:

* projekt nie je viazaný na jedno zariadenie
* projekt je replikovaný medzi účastníkmi
* projekt má viacero nositeľov

---

---

## 🧠 3.2 Lifecycle existencie

---

### 🔹 Fáza 1 – Lokálny vznik

* projekt vznikne na jednom uzle
* iniciátor vytvorí základ

---

---

### 🔹 Fáza 2 – Distribúcia

* projekt sa zdieľa do siete
* relevantné uzly ho prevezmú

---

---

### 🔹 Fáza 3 – Kolektívna existencia

* projekt je replikovaný
* má viacero nositeľov

---

👉 projekt už nepatrí jednému uzlu

---

---

## 🔄 3.3 Replikácia

---

Projekt je:

👉 uložený na viacerých uzloch (participant nodes)

---

Uzly:

* udržiavajú lokálnu kópiu
* synchronizujú zmeny

---

---

## ⚖️ 3.4 Neexistuje jediný zdroj pravdy

---

Systém NESMIE:

* mať centrálnu verziu projektu
* mať „master node“

---

👉 pravda je:

👉 **výsledok synchronizácie medzi uzlami**

---

---

## 🔀 3.5 Riešenie konfliktov

---

Konflikty sú riešené:

👉 **branchingom (ADR-024)**

---

To znamená:

* konfliktné zmeny nevytvárajú chybu
* vytvárajú nové vetvy

---

---

## 🌳 3.6 Projekt ako strom

---

Projekt je reprezentovaný ako:

👉 **tree (strom)**

---

Obsahuje:

* root (pôvodný návrh)
* vetvy (varianty)
* históriu zmien

---

---

## 🔐 3.7 Vlastníctvo

---

👉 projekt nie je vlastnený jedným používateľom

---

Obsahuje:

* iniciátora
* contributorov
* účastníkov

---

---

## 🔄 3.8 Synchronizácia

---

Uzly:

* zdieľajú zmeny
* aktualizujú lokálne verzie

---

Systém musí:

* tolerovať offline režim
* podporovať neskoršiu synchronizáciu

---

---

## 🧠 3.9 Perzistencia

---

Projekt prežije, ak:

* existuje aspoň jeden uzol, ktorý ho drží

---

---

## ⚠️ Riziko

---

Ak všetky uzly zmiznú:

👉 projekt zaniká

---

---

## 🌐 3.10 Discovery

---

Uzly musia byť schopné:

* nájsť projekt
* pripojiť sa k nemu

---

---

## 🧩 3.11 Identita projektu

---

Projekt má:

* unikátne ID
* históriu
* lineage (odkiaľ vznikol)

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitíva

---

### 🌐 Decentralizácia

* žiadny single point of failure

---

### 🔄 Odolnosť

* projekt prežije výpadky

---

### ⚖️ Sloboda

* nikto nemá kontrolu

---

---

## ❗ Negatíva / Trade-offs

---

### ⚙️ Komplexita synchronizácie

* zložité riešenie konfliktov

---

### 📉 Nekonzistentnosť

* dočasne rôzne verzie

---

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Centrálne uložený projekt

* jednoduchý
* ale:

  * strata decentralizácie

---

---

### ❌ Projekt viazaný na iniciátora

* intuitívny
* ale:

  * neodolný
  * centralizovaný

---

---

## 6. 🔗 Väzby na ďalšie ADR

---

* ADR-014 (P2P Network)
* ADR-022 (Project Formation)
* ADR-024 (Branching Model)
* ADR-027 (View Layer)
* ADR-028 (Support Model)

---

---

## 7. 📏 Pravidlá implementácie (high-level)

---

Systém musí:

* umožniť replikáciu projektu
* podporovať offline režim
* riešiť konflikty cez branching
* zabezpečiť unikátnu identitu projektu

---

---

# 🔥 8. Zhrnutie

---

👉
**Projekt je distribuovaný objekt, ktorý vzniká lokálne, ale žije kolektívne v P2P sieti a je replikovaný medzi účastníkmi.**

---

---

# 🧭 Finálna veta

---

👉
**Projekt nežije na jednom zariadení – žije v sieti tých, ktorí ho udržiavajú.**

---

---

# 👉 Čo teraz

---

Teraz máte:

* projekty
* vetvy
* podporu
* view
* distribúciu

---

👉 chýba už len:

👉 **ADR-015: Participation Filter**

→ kto sa zapojí, kedy, prečo

---
