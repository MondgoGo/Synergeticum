# 🧾 ADR-032: Question & Prompt Generation System

---

# 1. 📌 Status

**Proposed**

---

# 2. 🎯 Kontext

Synergetikum stojí na:

* otázkach (questions)
* odpovediach (responses)
* topicoch (topics)
* projektoch (projects)

---

Celý systém začína otázkou.

---

### Problém

Bez riadenia otázok systém degeneruje na:

* náhodný obsah
* šum
* nezmyselné diskusie
* nízku hodnotu

---

Ak sú otázky slabé:

* odpovede sú slabé
* topicy sú slabé
* projekty nevzniknú

---

### Kľúčová otázka

👉
**Ako generovať a distribuovať otázky tak, aby boli relevantné, hodnotné a viedli k mysleniu a akcii?**

---

# 3. ⚖️ Rozhodnutie

Systém zavádza:

👉 **riadený Question & Prompt Generation System**

---

Tento systém:

* generuje otázky z viacerých zdrojov
* personalizuje ich pre používateľa
* filtruje ich podľa relevance
* riadi ich distribúciu

---

# 4. 🧠 Typy otázok

Systém rozlišuje viac typov otázok:

---

## 4.1 User-originated

Otázky od používateľov:

* „Chýba tu pizzéria?“
* „Má zmysel robiť cowork?“

---

## 4.2 AI-generated (personal)

Personal AI generuje otázky pre konkrétneho používateľa:

* reflexné otázky
* rozhodovacie otázky
* rozširujúce otázky

---

## 4.3 AI-generated (collective)

Systém generuje otázky pre komunitu:

* „Má táto téma potenciál projektu?“
* „Aký je najväčší problém tejto oblasti?“

---

## 4.4 Derived questions

Vznikajú z:

* diskusie
* konfliktu
* argumentov

---

---

# 5. 🧩 Question Object Structure

Každá otázka obsahuje:

---

## 5.1 Identity

* question_id
* text
* author (user / system / AI)
* timestamp

---

## 5.2 Context

* origin (topic / project / personal)
* referencie (branch / argument / event)

---

## 5.3 Type

* exploratory
* decision
* commitment
* reflective

---

## 5.4 Scope

* personal
* local
* community
* system-wide

---

## 5.5 Signals

* relevance_score
* complexity_level
* engagement_score

---

# 6. 🔄 Question Lifecycle

---

## 6.1 created

otázka vznikla

---

## 6.2 distributed

bola poslaná do siete

---

## 6.3 answered

má odpovede

---

## 6.4 amplified

získava pozornosť

---

## 6.5 converted_to_topic

vznikol topic

---

## 6.6 faded

stratila význam

---

---

# 7. 🧠 Personalization Layer

Každý používateľ vidí iné otázky.

---

## Personal AI rozhoduje:

* čo ukázať
* v akom poradí
* v akej forme

---

## Kritériá:

* záujmy
* schopnosti
* historické odpovede
* kognitívny profil

---

👉 otázky sa nemenia
👉 mení sa ich poradie a framing

---

# 8. 🎯 Multi-Perspective Generation

Každá otázka môže mať viac perspektív:

---

Príklad:

**Origin question:**
„Má zmysel pizzéria?“

---

## Perspektívy:

* praktická → „Chodil by si tam?“
* realizačná → „Vieš sa zapojiť?“
* finančná → „Aký je návrat?“
* systémová → „Ako to ovplyvní komunitu?“

---

👉 Personal AI rozhoduje, ktorú perspektívu zobrazí ako prvú

---

# 9. 🌐 Distribution Model

---

Otázky sa distribuujú:

* cez P2P
* selektívne
* cez Participation Filter

---

Nie každý uzol dostane každú otázku.

---

# 10. 🧠 Úloha AI

AI:

* generuje otázky
* transformuje otázky
* vytvára perspektívy
* sumarizuje odpovede

---

AI NESMIE:

* manipulovať
* skrývať alternatívy
* určovať správnu odpoveď

---

# 11. ⚖️ Anti-Manipulation Principle

---

Systém musí zabezpečiť:

* transparentnosť pôvodu otázky
* možnosť vidieť originálnu verziu
* možnosť vidieť alternatívne formulácie

---

---

# 12. 📊 Dôsledky rozhodnutia

---

## ✅ Pozitíva

* kvalitnejšie diskusie
* vyššia relevancia
* viac projektov
* lepšia personalizácia

---

## ❗ Negatíva

* komplexný systém
* závislosť na AI kvalite
* riziko zlého nastavenia

---

# 13. 🚫 Alternatívy

---

## ❌ Neriadené otázky (chaos)

zamietnuté

---

## ❌ Centrálne riadené otázky

zamietnuté

---

---

# 14. 🔗 Väzby na ADR

* ADR-031: Topic Layer
* ADR-022: Project Formation
* ADR-015: Participation Filter
* ADR-027: View Layer

---

# 15. 📏 Pravidlá implementácie

---

Systém musí:

* generovať otázky z viacerých zdrojov
* personalizovať distribúciu
* podporovať multi-perspective
* sledovať lifecycle
* prepájať otázky → topic → project

---

# 🔥 16. Zhrnutie

👉
**Otázky sú vstupným bodom systému a ich kvalita určuje kvalitu celého Synergetika.**

---

# 🧭 Finálna veta

👉
**Ak systém kladie správne otázky, ľudia robia lepšie rozhodnutia.
Ak kladie zlé otázky, celý systém degeneruje.**

---
