# 🧾 ADR-035 — Sustainability & Incentive Model

---

## 1. 📌 Status

Proposed

---

## 2. 🎯 Kontext

Synergetikum je systém postavený na princípoch:

* **Personal ownership** (ADR-001)
* **Privacy-first architecture** (ADR-004)
* **Non-manipulation** (ADR-003)
* **Human-in-the-loop** (ADR-002)

Tieto princípy vylučujú bežné modely monetizácie:

* ❌ predaj používateľských dát
* ❌ reklama založená na profilovaní
* ❌ manipulácia správania pre zisk

---

### Problém

> **Ako zabezpečiť dlhodobú udržateľnosť systému bez porušenia jeho základných princípov?**

---

Bez riešenia:

* systém nemá zdroje na rozvoj
* infraštruktúra nebude udržateľná
* vznikne tlak na kompromisy (data selling, ads, centralizácia)

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Decentralizovaná udržateľnosť

---

Synergetikum nepoužíva jeden centrálny biznis model.

👉 namiesto toho:

> **kombinuje viacero zdrojov hodnoty, ktoré sú v súlade s princípmi systému**

---

---

## 💡 3.2 Core princíp

---

> **Používateľ nie je produkt. Hodnota vzniká zo služieb, nie z dát.**

---

---

## 🧩 3.3 Zdroje udržateľnosti

---

### 1. 🧠 Open-source základ

---

* core systém je open-source
* umožňuje komunitný vývoj
* znižuje závislosť na jednej entite

---

👉 výhoda:

* transparentnosť
* dôvera
* dlhodobá životaschopnosť

---

---

### 2. 🖥️ Parent node služby

---

Používatelia môžu:

* prevádzkovať vlastný parent node
* alebo využiť poskytovateľa

---

👉 platené služby môžu zahŕňať:

* hosting parent node
* zálohovanie (šifrované)
* synchronizačné služby
* vyšší výkon / dostupnosť

---

👉 dôležité:

> poskytovateľ nemá prístup k dátam (end-to-end šifrovanie)

---

---

### 3. 🤝 Granty a verejné financovanie

---

Systém môže byť financovaný cez:

* výskumné granty
* verejné inštitúcie
* neziskové organizácie

---

👉 dôvod:

* spoločenský prínos (lepšie rozhodovanie, spolupráca)

---

---

### 4. 🧩 Modulárne služby (opt-in)

---

Rozšírenia systému môžu byť:

* platené
* dobrovoľné

---

napr.:

* pokročilé AI modely
* špecializované analýzy
* enterprise nástroje

---

👉 základ systému ostáva:

> **plne funkčný bez platenia**

---

---

### 5. 🔁 Reciprocity (ADR-036)

---

Hodnota v systéme vzniká aj:

* participáciou
* zdieľaním (dobrovoľným)
* spoluprácou

---

👉 nejde o finančný model, ale:

> **internú ekonomiku hodnoty**

---

---

## 🚫 3.4 Zakázané modely

---

Synergetikum nikdy nepoužíva:

---

### ❌ Predaj dát

* žiadne obchodovanie s používateľskými dátami

---

### ❌ Reklama založená na profilovaní

* žiadne behaviorálne reklamy

---

### ❌ Manipulácia engagementu

* žiadne algoritmy na „udržanie pozornosti“

---

### ❌ Paywall na základné funkcie

* základ systému musí byť dostupný

---

---

## 🧠 3.5 Oddelenie dát a monetizácie

---

> Monetizácia nikdy nesmie závisieť od prístupu k dátam.

---

To znamená:

* dáta sú vždy vlastníctvom používateľa
* poskytovateľ služieb pracuje len s metadátami alebo šifrovanými dátami

---

---

## 🧠 3.6 Dlhodobá udržateľnosť

---

Systém je navrhnutý tak, aby:

* prežil aj bez jednej organizácie
* bol prevádzkovateľný komunitou
* nebol závislý na jednom zdroji príjmu

---

---

## 🧠 3.7 Incentive model

---

Motivácia účastníkov je:

---

### 🔹 Používateľ

* lepšie rozhodovanie
* kvalitnejšie výstupy

---

### 🔹 Vývojár

* open-source príspevok
* reputácia (mimo systému, nie skóre)

---

### 🔹 Poskytovateľ služby

* príjem zo služieb (nie z dát)

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitíva

---

### 🔒 Zachovanie princípov

* súkromie
* vlastníctvo
* ne-manipulácia

---

### 🌐 Decentralizácia

* systém nie je závislý na jednej firme

---

### 🤝 Dôvera používateľov

* transparentný model

---

---

## ❗ Negatíva / Trade-offs

---

### 💰 Pomalší rast

* bez agresívnej monetizácie

---

### ⚙️ Komplexnejší ekosystém

* viac zdrojov financovania

---

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Reklamný model

* porušuje privacy
* vytvára manipuláciu

---

---

### ❌ Predaj dát

* priamo proti core princípom

---

---

### ❌ Centralizovaný SaaS model

* vytvára závislosť
* ohrozuje ownership

---

---

## 6. 🔗 Väzby na ďalšie ADR

---

* ADR-001 — Personal AI Ownership
* ADR-002 — Human-in-the-loop
* ADR-003 — Non-manipulation
* ADR-004 — Privacy-first
* ADR-036 — Reciprocity
* ADR-037 — Identity

---

---

## 7. 📏 Pravidlá implementácie (high-level)

---

Systém musí:

* oddeliť dáta od monetizácie
* podporovať open-source jadro
* umožniť platené služby bez prístupu k dátam
* zachovať plnú funkcionalitu bez paywallu
* podporovať decentralizované prevádzkovanie

---

---

# 🔥 8. Zhrnutie

---

👉
**Synergetikum je financované službami a komunitou, nie dátami používateľov.**

---

---

# 🧭 Finálna veta

---

👉
**Ak musí systém zarábať na dátach používateľa, prestáva byť systémom pre používateľa.**

---
