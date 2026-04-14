# 🧾 ADR-028: Support & Alignment Model

---

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

Systém Personal AI Network:

* obsahuje projekty s viacerými variantmi (ADR-024)
* prezentuje ich cez View Layer (ADR-027)
* umožňuje používateľom reagovať a zapájať sa

---

Každý variant projektu:

* má podporovateľov
* má kritikov
* má rôznu mieru zapojenia

---

### Problém

Je potrebné definovať:

👉 **ako reprezentovať podporu jednotlivých variantov**

tak, aby:

* nedošlo k redukcii na jednoduché hlasovanie
* nedochádzalo k manipulácii
* bol zachovaný kontext a kvalita rozhodovania

---

### Kľúčová otázka

👉
**Ako vyjadriť podporu variantu bez degradácie systému na „like/dislike“ model?**

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Zavedenie „Support Model“

Systém zavádza:

👉 **multi-dimenzionálny model podpory**

---

---

## 📊 3.2 Dimenzie podpory

Každý používateľ môže vyjadriť voči variantu:

---

### 🔹 Alignment (súlad)

👉 „tento smer mi dáva zmysel“

---

### 🔹 Commitment (záväzok)

👉 „som ochotný sa zapojiť“

---

### 🔹 Confidence (istota)

👉 „ako veľmi tomu verím“

---

---

👉 tieto dimenzie sú nezávislé

---

---

## ⚖️ 3.3 Váhovanie podpory

---

Podpora variantu NIE JE:

❌ počet hlasov

---

👉 ale:

👉 kombinácia:

* alignment
* commitment
* confidence
* (voliteľne) expertíza

---

---

## 🧠 3.4 Rozdiel medzi názorom a akciou

---

👉 systém explicitne rozlišuje:

---

### 🟢 Názor

* alignment
* nízka váha

---

### 🔴 Akcia

* commitment
* vysoká váha

---

---

👉
**akcia má vyššiu váhu než názor**

---

---

## 🧠 3.5 Profil podpory variantu

---

Každý variant obsahuje:

---

### 📊 Alignment score

koľko ľudí s ním súhlasí

---

### 🤝 Commitment score

koľko ľudí sa zapája

---

### 🎯 Confidence score

ako silno tomu veria

---

### 🧠 Expertise signal (voliteľné)

aká je kvalita zdrojov

---

---

👉 vzniká:

**profil podpory, nie výsledok hlasovania**

---

---

## ⚖️ 3.6 Žiadne priame hlasovanie

---

Systém NESMIE:

* používať jednoduché hlasovanie
* určovať víťaza na základe počtu hlasov

---

👉 cieľ nie je „vyhrať“

---

---

## 🧠 3.7 Zobrazenie používateľovi

---

Používateľ vidí:

---

* ktoré varianty sú populárne
* ktoré majú reálnu podporu
* ktoré majú zdroje

---

👉 nie:

* kto vyhráva

---

---

## 🔄 3.8 Dynamika v čase

---

Podpora:

* sa mení
* reaguje na nové informácie
* nie je fixná

---

---

## 🚫 3.9 Ochrana proti manipulácii

---

Systém musí:

* ignorovať spam
* znižovať váhu neaktívnych účtov
* oddeľovať hlasitosť od kvality

---

---

## 🔍 3.10 Personalizované vnímanie

---

Personal AI používateľa:

* interpretuje podporu
* zvýrazní relevantné signály
* filtruje šum

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitíva

---

### ⚖️ Odolnosť voči manipulácii

* nie je možné „prehlasovať“ systém

---

### 🧠 Kvalitnejšie rozhodovanie

* viac dimenzií

---

### 🤝 Prepojenie na realitu

* zapojenie = reálny signál

---

---

## ❗ Negatíva / Trade-offs

---

### ⚙️ Komplexita

* ťažšie pochopiteľné

---

### 📉 Menej intuitívne než hlasovanie

* treba UI vysvetlenie

---

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Like / dislike systém

* jednoduchý
* ale manipulovateľný

---

---

### ❌ Jednoduché hlasovanie

* demokratické
* ale skreslené

---

---

## 6. 🔗 Väzby na ďalšie ADR

---

* ADR-024 (Multi-variant Governance)
* ADR-027 (View Layer)
* ADR-015 (Participation Filter)
* ADR-019 (Opportunity Engine)

---

---

## 7. 📏 Pravidlá implementácie (high-level)

---

Systém musí:

* podporovať viac dimenzií podpory
* oddeliť názor od záväzku
* zabrániť hlasovaciemu modelu
* zobrazovať profil podpory

---

---

# 🔥 8. Zhrnutie

---

👉
**Podpora variantu je reprezentovaná ako viacrozmerný profil (alignment, commitment, confidence), nie ako jednoduché hlasovanie.**

---

---

# 🧭 Finálna veta

---

👉
**Nie všetky hlasy majú rovnakú váhu – a systém to musí vedieť bez toho, aby potlačil slobodu.**

---

---

# 👉 Ďalší logický krok

---

Teraz má zmysel:

👉 ADR-023 (Distributed Project Object Model)

→ lebo už máme:

* projekty
* vetvy
* podporu

ale ešte nemáme:

👉 kde to celé „žije“ a ako sa synchronizuje

---
