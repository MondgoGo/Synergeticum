# 🧾 ADR-025: Argumentation & Reasoning Model (Conceptual Projects)

---

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

Systém Personal AI Network:

* umožňuje vznik projektov (ADR-022)
* podporuje multi-variant vývoj (ADR-024)
* používa support model (ADR-028)
* filtruje participáciu (ADR-015)

---

Tieto mechanizmy dobre fungujú pre:

* realizačné projekty (napr. pizzéria)

---

### Problém

Pre **konceptuálne projekty** (napr. ústava, pravidlá, hodnoty):

* neexistuje jednoznačné „overenie realitou“
* commitment nestačí
* hlasovanie je manipulovateľné
* dominujú:

  * silné osobnosti
  * emócie
  * rétorika

---

👉 systém potrebuje inú vrstvu

---

### Kľúčová otázka

👉
**Ako umožniť kvalitnú kolektívnu diskusiu a rozhodovanie bez dominancie ega, manipulácie a chaosu?**

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Zavedenie „Argumentation Layer“

---

Systém zavádza:

👉 **Argumentation & Reasoning Layer**

---

Táto vrstva:

* štrukturuje diskusiu
* oddeľuje argument od osoby
* umožňuje porovnanie návrhov

---

---

## 🧩 3.2 Argument ako základná jednotka

---

Základná entita nie je:

❌ komentár
❌ názor

---

👉 ale:

👉 **argument**

---

---

## 🧾 3.3 Štruktúra argumentu

Každý argument obsahuje:

---

### 📌 Claim (tvrdenie)

čo tvrdím

---

### 📌 Reasoning (odôvodnenie)

prečo to tvrdím

---

### 📌 Assumptions (predpoklady)

na čom to stojí

---

### 📌 Implications (dôsledky)

čo to spôsobí

---

---

👉 bez tejto štruktúry:

* argument nemá plnú váhu

---

---

## ⚔️ 3.4 Povinná existencia protiargumentu

---

Každý významný návrh musí mať:

👉 **counter-argument**

---

To znamená:

* systém aktívne generuje opozíciu
* alebo ju vyžaduje

---

👉 cieľ:

👉 vyvážiť bias

---

---

## 🧠 3.5 Oddelenie osoby od argumentu

---

Systém NESMIE:

* zvýhodňovať autora argumentu
* zvýhodňovať popularitu

---

👉 hodnotí sa:

👉 **argument, nie človek**

---

---

## 🧠 3.6 Argument graph

---

Argumenty tvoria:

👉 **graph (sieť vzťahov)**

---

Obsahuje:

* podporujúce argumenty
* protiargumenty
* alternatívne pohľady

---

---

👉 projekt nie je lineárna diskusia
👉 ale **mapa argumentov**

---

---

## ⚖️ 3.7 Hodnotenie argumentov

---

Argument sa hodnotí podľa:

---

### 🔹 Koherencia

je logicky konzistentný

---

### 🔹 Pokrytie

zohľadňuje viac aspektov

---

### 🔹 Robustnosť

odoláva protiargumentom

---

### 🔹 Praktické dôsledky

je aplikovateľný

---

---

👉 NIE:

* podľa počtu hlasov

---

---

## 🧠 3.8 Úloha AI

---

AI:

* extrahuje argumenty z textu
* štrukturuje ich
* odstraňuje emócie
* generuje protiargumenty
* sumarizuje diskusiu

---

---

AI NESMIE:

* rozhodovať
* určovať „pravdu“

---

---

## 🔄 3.9 Výstup projektu

---

Výsledok nie je:

❌ jeden víťaz

---

👉 ale:

👉 **structured reasoning output**

---

Obsahuje:

* hlavné varianty
* argumenty pre každý
* protiargumenty
* dôsledky

---

---

## 🧠 3.10 Prepojenie na View Layer

---

Používateľ vidí:

* zjednodušené argumenty
* hlavné konflikty
* kľúčové rozhodovacie body

---

👉 nie celý graph

---

---

## 🧠 3.11 Governance napojenie

---

Argumentation Layer:

* dopĺňa Support Model (ADR-028)
* dopĺňa Branching (ADR-024)

---

👉 používa sa hlavne pre:

* conceptual / normative projekty

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitíva

---

### 🧠 Kvalitnejšie myslenie

* menej emócií
* viac logiky

---

### ⚖️ Ochrana pred manipuláciou

* argument > ego

---

### 🌐 Škálovateľnosť diskusie

* veľké témy zvládnuteľné

---

---

## ❗ Negatíva / Trade-offs

---

### ⚙️ Komplexita

* vyšší vstupný náklad

---

### 🧠 Náročnosť pre usera

* treba UX podporu

---

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Voľná diskusia (komentáre)

* jednoduchá
* ale:

  * chaotická
  * manipulovateľná

---

---

### ❌ Hlasovanie

* intuitívne
* ale:

  * skreslené
  * náchylné na dav

---

---

## 6. 🔗 Väzby na ďalšie ADR

---

* ADR-024 (Branching)
* ADR-027 (View Layer)
* ADR-028 (Support Model)
* ADR-015 (Participation Filter)

---

---

## 7. 📏 Pravidlá implementácie (high-level)

---

Systém musí:

* umožniť štruktúrované argumenty
* podporovať protiargumenty
* vytvárať argument graph
* oddeliť autora od obsahu
* prepojiť s AI summarization

---

---

# 🔥 8. Zhrnutie

---

👉
**Konceptuálne projekty sú riadené kvalitou argumentov, nie hlasovaním ani silou osobností.**

---

---

# 🧭 Finálna veta

---

👉
**V komplexných problémoch nevyhráva ten, kto kričí najhlasnejšie – ale ten, koho argument prežije najviac otázok.**

---

---

# 🧠 FINÁLNY STAV SYSTÉMU

---

Teraz máš komplet:

* P2P sieť
* Personal AI
* projekty
* vetvy
* lifecycle
* support
* view
* participation
* 👉 argumentačný model

---

👉 toto je už reálne nový typ systému

---
