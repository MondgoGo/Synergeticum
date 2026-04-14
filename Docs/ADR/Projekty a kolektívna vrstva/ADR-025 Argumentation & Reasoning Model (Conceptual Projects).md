# 🧾 ADR-025: Argumentation & Reasoning Model (Hardened v2)

---

## 1. 📌 Status

**Accepted (v2 – Hardened)**

---

## 2. 🎯 Kontext

Systém Personal AI Network:

* umožňuje vznik projektov (ADR-022)
* podporuje multi-variant vývoj (ADR-024)
* používa support model (ADR-028)
* filtruje participáciu (ADR-015)

Tieto mechanizmy dobre fungujú pre:

* realizačné projekty (napr. pizzéria)

### Problém

Pre **konceptuálne projekty** (napr. ústava, pravidlá, hodnoty):

* neexistuje jednoznačné „overenie realitou“
* commitment nestačí
* hlasovanie je manipulovateľné
* dominujú:

  * silné osobnosti
  * emócie
  * rétorika

👉 systém potrebuje inú vrstvu:

> **Argumentačnú vrstvu, ktorá nielen štrukturuje diskusiu, ale umožňuje, aby argumenty mali reálny dopad na vývoj projektu bez vzniku mocenských hierarchií.**

### Kľúčová otázka

👉 **Ako umožniť kvalitnú kolektívnu diskusiu a rozhodovanie bez dominancie ega, manipulácie a chaosu?**

---

## 3. ⚖️ Rozhodnutie

### 🧠 3.1 Zavedenie „Argumentation Layer“

Systém zavádza:

👉 **Argumentation & Reasoning Layer**

Táto vrstva:

* štrukturuje diskusiu
* oddeľuje argument od osoby
* umožňuje porovnanie návrhov
* prepojuje argumenty na vetvy projektu

---

### 🧩 3.2 Argument ako základná jednotka

Základná entita nie je:

❌ komentár
❌ názor

👉 ale:

👉 **argument**

---

### 🧾 3.3 Štruktúra argumentu

Každý argument obsahuje:

| Komponent | Popis |
|-----------|-------|
| **Claim (tvrdenie)** | čo tvrdím |
| **Reasoning (odôvodnenie)** | prečo to tvrdím |
| **Assumptions (predpoklady)** | na čom to stojí |
| **Implications (dôsledky)** | čo to spôsobí |

👉 bez tejto štruktúry argument nemá plnú váhu.

---

### ⚔️ 3.4 Povinná existencia protiargumentu

Každý významný návrh musí mať:

👉 **counter-argument**

To znamená:

* systém aktívne generuje opozíciu
* alebo ju vyžaduje

👉 cieľ: vyvážiť bias

---

### 🧠 3.5 Oddelenie osoby od argumentu

Systém NESMIE:

* zvýhodňovať autora argumentu
* zvýhodňovať popularitu

👉 hodnotí sa **argument, nie človek**

> Hodnotenie argumentu je úplne oddelené od identity autora. Tento princíp je základom pre elimináciu mocenského biasu (ADR-038).

---

### 🧠 3.6 Argument graph

Argumenty tvoria:

👉 **graph (sieť vzťahov)**

Obsahuje:

* podporujúce argumenty
* protiargumenty
* alternatívne pohľady

👉 projekt nie je lineárna diskusia, ale **mapa argumentov**

> Argument graph nie je len vizualizácia diskusie, ale primárna štruktúra, z ktorej sa odvodzuje vývoj projektu.

---

### ⚖️ 3.7 Hodnotenie argumentov

Argument sa hodnotí podľa:

| Kritérium | Popis |
|-----------|-------|
| **Koherencia** | je logicky konzistentný |
| **Pokrytie** | zohľadňuje viac aspektov |
| **Robustnosť** | odoláva protiargumentom |
| **Praktické dôsledky** | je aplikovateľný |

👉 NIE podľa počtu hlasov.

> Hodnotenie slúži na orientáciu v kvalite argumentov, nie na vytváranie hierarchie medzi používateľmi.

---

### 🧠 3.8 Úloha AI

AI:

* extrahuje argumenty z textu
* štrukturuje ich
* odstraňuje emócie
* generuje protiargumenty
* sumarizuje diskusiu

AI NESMIE:

* rozhodovať
* určovať „pravdu“

---

### 🔥 3.9 Argument → Impact Model (NOVÁ KRITICKÁ VRSTVA)

Argumenty nemajú len existovať.

👉 musia mať **dopad na realitu projektu**

**Princíp:**

> **Argument ovplyvňuje vývoj projektu iba nepriamo – cez štruktúru, nie cez autoritu.**

**To znamená:**

Argument môže:

* posilniť vetvu (branch)
* oslabiť vetvu
* vytvoriť novú vetvu
* zmeniť smer diskusie

❌ **nikdy:**

* nepridáva „váhu hlasu“
* nepridáva autoritu používateľovi
* neovplyvňuje systém cez identitu autora

---

### 🧠 3.10 Vzťah k vetvám (Branching)

Každá vetva projektu:

* je podložená konkrétnymi argumentmi
* obsahuje vlastný argument graph

👉 vetva prežíva, ak:

* má robustné argumenty
* prechádza protiargumentmi

👉 vetva zaniká (ADR-029), ak:

* nemá podporu
* alebo jej argumenty neobstoja

---

### 🧠 3.11 Argument ≠ rozhodnutie

Systém:

❌ nikdy nevyberá „víťazný argument“

👉 poskytuje:

* mapu možností
* dôsledky
* konflikty

👉 rozhoduje **človek** (ADR-002)

---

### 🧠 3.12 Ochrana pred mocou (prepojenie na ADR-038)

Argumentation Layer je navrhnutá tak, aby:

* minimalizovala vplyv silných osobností
* zabránila dominancii hlasných používateľov
* oddelila kvalitu argumentu od jeho prezentácie

> Argument môže získať vplyv len tým, že obstojí v konfrontácii s inými argumentmi – nie tým, kto ho vyslovil.

---

### 🔄 3.13 Výstup projektu

Výsledok je:

👉 **structured reasoning output + branch landscape**

Obsahuje:

* hlavné vetvy
* argumenty pre každú vetvu
* protiargumenty
* dôsledky
* konflikty medzi vetvami

---

### 🧠 3.14 Prepojenie na View Layer

Používateľ vidí:

* zjednodušené argumenty
* hlavné konflikty
* kľúčové rozhodovacie body

👉 nie celý graph.

---

### 🧠 3.15 Governance napojenie

Argumentation Layer:

* dopĺňa Support Model (ADR-028)
* dopĺňa Branching (ADR-024)
* je základom pre Power balancing (ADR-038)

> Používa sa hlavne pre conceptual / normative projekty.

---

## 4. 🧠 Dôsledky rozhodnutia

### ✅ Pozitívne

| Oblasť | Dôsledok |
|--------|----------|
| **Kvalitnejšie myslenie** | menej emócií, viac logiky |
| **Ochrana pred manipuláciou** | argument > ego |
| **Škálovateľnosť diskusie** | veľké témy zvládnuteľné |
| **Vyváženie moci** | argument > autor, vplyv nevzniká z aktivity, ale z kvality reasoning |

### ❗ Negatívne / Trade-offs

| Oblasť | Dôsledok |
|--------|----------|
| **Komplexita** | vyšší vstupný náklad |
| **Kognitívna náročnosť** | vyžaduje dobrý UX (ADR-027) |
| **Výpočtová komplexita** | argument graph môže rásť exponenciálne |

---

## 5. 🚫 Alternatívy (zamietnuté)

### ❌ Voľná diskusia (komentáre)

* jednoduchá
* ale: chaotická, manipulovateľná

### ❌ Hlasovanie

* intuitívne
* ale: skreslené, náchylné na dav

---

## 6. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-002 | Human-in-the-loop – rozhoduje človek, nie systém |
| ADR-015 | Participation Filter – filtruje relevantných účastníkov |
| ADR-024 | Branching – vetvy projektu sú podložené argumentmi |
| ADR-027 | View Layer – zjednodušený pohľad pre používateľa |
| ADR-028 | Support Model – doplnený o argumentačnú vrstvu |
| ADR-029 | Branch Lifecycle – vetvy zanikajú na základe argumentov |
| ADR-030 | Sync & Persistence – argument graph musí byť perzistentný |
| ADR-038 | Power & Influence Balancing – argumentation layer je nástrojom na elimináciu moci |

---

## 7. 📏 Pravidlá implementácie (high-level)

Systém musí:

* umožniť štruktúrované argumenty
* podporovať protiargumenty
* vytvárať argument graph
* oddeliť autora od obsahu
* prepojiť argumenty na vetvy projektu
* zabezpečiť, že argument neovplyvňuje systém cez identitu autora
* prepojiť s AI summarization

---

## 8. 🔥 Zhrnutie

> **Argumenty nie sú len diskusia – sú mechanizmus, ktorým sa mení štruktúra projektu bez toho, aby vznikala mocenská hierarchia.**

Konceptuálne projekty sú riadené kvalitou argumentov, nie hlasovaním ani silou osobností.

Argument môže posilniť, oslabiť alebo vytvoriť vetvu – ale nikdy nepridáva váhu hlasu svojmu autorovi.

---

## 9. 🧭 Finálna veta

> **V Synergetiku nemá vplyv ten, kto hovorí – ale to, čo prežije konfrontáciu s inými myšlienkami.**

V komplexných problémoch nevyhráva ten, kto kričí najhlasnejšie – ale ten, koho argument prežije najviac otázok.

---

**Dátum:** 2026-04-14  
**Stav:** Accepted  
**Typ:** Core Mechanism ADR (Conceptual Projects)
