# 🧾 ADR-027: Project View & Simplification Model

---

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

Systém Personal AI Network umožňuje:

* vznik projektov (ADR-022)
* ich vetvenie (ADR-024)
* kolektívne prispievanie

---

Výsledkom je, že projekt:

* obsahuje viacero variantov (branchov)
* má rozsiahlu históriu
* obsahuje množstvo argumentov a príspevkov

---

### Problém

Pre bežného používateľa:

👉 je takýto projekt neprehľadný a nepochopiteľný

---

Bez riešenia:

* používateľ sa stratí
* prestane systém používať
* systém stratí adopciu

---

### Kľúčová otázka

👉
**Ako prezentovať komplexný, rozvetvený projekt tak, aby bol pochopiteľný pre každého používateľa bez straty podstaty?**

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Zavedenie „View Layer“

Systém zavádza:

👉 **Project View Layer**

---

### Definícia

Vrstva, ktorá:

* transformuje komplexný projekt
* do zjednodušeného, personalizovaného pohľadu

---

---

## 🔄 3.2 Oddelenie vrstiev

Projekt má dve vrstvy:

---

### 🔹 Reality Layer (plný stav)

Obsahuje:

* všetky vetvy
* všetky príspevky
* celú históriu

---

👉 kompletný, ale komplexný

---

---

### 🔹 View Layer (pre používateľa)

Obsahuje:

* vybrané relevantné vetvy
* zhrnutia
* interpretácie

---

👉 zjednodušený, ale verný

---

---

## 🧠 3.3 Personalizovaný pohľad

---

View Layer je:

👉 generovaný Personal AI používateľa

---

To znamená:

* každý používateľ vidí projekt inak
* podľa svojho profilu a preferencií

---

---

## 🔍 3.4 Výber relevantných vetiev

---

Systém vyberá:

👉 **limitovaný počet vetiev (napr. 3–5)**

---

Na základe:

* preferencií používateľa
* typu myslenia
* predchádzajúcich reakcií
* relevancie

---

---

## 🧾 3.5 Zhrnutie vetiev

---

Každá vetva je prezentovaná ako:

---

### 📌 stručný popis

čo návrh rieši

---

### ⚖️ hlavné rozdiely

čo ju odlišuje od ostatných

---

### 📊 podpora

koľko ľudí ju podporuje

---

### 🤝 zapojenie

koľko ľudí sa chce zapojiť

---

### ⚠️ riziká

hlavné slabiny

---

---

## ⚔️ 3.6 Identifikácia konfliktov

---

Systém zvýrazňuje:

👉 hlavné konflikty medzi vetvami

---

Príklad:

* 1 poschodie vs 2 poschodia
* nízke náklady vs vysoká kvalita

---

---

## ❓ 3.7 Reflexná vrstva

---

Systém generuje otázky pre používateľa:

👉 „ktorý smer ti dáva väčší zmysel?“

👉 „čo je pre teba dôležitejšie?“

---

---

## 🚫 3.8 Zákaz manipulácie

---

View Layer NESMIE:

* skryť existujúce vetvy
* preferovať jednu vetvu bez dôvodu
* meniť význam návrhov

---

---

## 🔍 3.9 Prístup k plnej realite

---

Používateľ musí mať možnosť:

👉 prepnúť na Reality Layer

---

---

## 🧠 3.10 Adaptácia v čase

---

View Layer:

* sa mení podľa používateľa
* reaguje na jeho rozhodnutia
* učí sa jeho preferencie

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitíva

---

### 🧩 Zrozumiteľnosť

* projekt je pochopiteľný

---

### 📈 Škálovateľnosť

* systém zvládne komplexitu

---

### 🎯 Personalizácia

* každý vidí relevantné veci

---

---

## ❗ Negatíva / Trade-offs

---

### ⚠️ Riziko skreslenia

* používateľ nevidí všetko

---

### 🧠 Závislosť na kvalite AI

* zlé zhrnutie = zlý pohľad

---

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Zobraziť celý projekt všetkým

* transparentné
* ale nepoužiteľné

---

---

### ❌ Fixné zhrnutie pre všetkých

* jednoduché
* ale nepersonalizované

---

---

## 6. 🔗 Väzby na ďalšie ADR

---

* ADR-022 (Project Formation)
* ADR-024 (Multi-variant Governance)
* ADR-028 (Support Model)
* ADR-011 (Personalization)

---

---

## 7. 📏 Pravidlá implementácie (high-level)

---

Systém musí:

* oddeliť reality a view layer
* generovať personalizované zhrnutia
* limitovať počet vetiev
* umožniť prístup k plnej verzii

---

---

# 🔥 8. Zhrnutie

---

👉
**Komplexný projekt je transformovaný do personalizovaného, pochopiteľného pohľadu bez straty podstaty.**

---

---

# 🧭 Finálna veta

---

👉
**Používateľ nemusí pochopiť celý systém – stačí, ak pochopí svoju časť reality.**

---

