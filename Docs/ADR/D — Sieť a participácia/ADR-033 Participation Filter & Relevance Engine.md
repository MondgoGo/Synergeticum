# 🧾 ADR-033: Participation Filter & Relevance Engine

---

# 1. 📌 Status

**Proposed**

---

# 2. 🎯 Kontext

Synergetikum obsahuje:

* otázky (questions)
* topicy (topics)
* projekty (projects)
* participantov (users + Personal AI)

---

Každý uzol (človek + jeho Personal AI) by teoreticky mohol:

* vidieť všetko
* odpovedať na všetko
* zapájať sa do všetkého

---

### Problém

To vedie k:

* extrémnemu šumu
* preťaženiu systému
* nízkej kvalite odpovedí
* zníženej relevancii

---

👉 Systém potrebuje mechanizmus, ktorý rozhodne:

* kto čo vidí
* kto sa zapojí
* kto odpovedá

---

### Kľúčová otázka

👉
**Ako zabezpečiť, aby sa do otázok, topicov a projektov zapájali len relevantné uzly bez centrálneho riadenia?**

---

# 3. ⚖️ Rozhodnutie

Systém zavádza:

👉 **Participation Filter & Relevance Engine**

---

Tento mechanizmus:

* beží lokálne v Personal AI
* rozhoduje o zapojení uzla
* filtruje distribúciu
* optimalizuje kvalitu siete

---

# 4. 🧠 Základný princíp

👉
**Nie každý má odpovedať na všetko.**

---

Účasť je:

* selektívna
* kontextová
* adaptívna

---

---

# 5. 🧩 Participation Decision Model

Každý uzol si pre každú entitu (question/topic/project) vypočíta:

👉 **participation_score**

---

## Vstupy:

### 5.1 Relevance

* téma vs záujmy používateľa

---

### 5.2 Capability

* schopnosť prispieť (vedomosti, skillset)

---

### 5.3 Context fit

* lokálnosť (napr. mesto)
* situácia používateľa

---

### 5.4 Engagement history

* predchádzajúca aktivita

---

### 5.5 Cognitive alignment

* spôsob myslenia vs typ otázky

---

---

## Výstup:

```text
participation_score ∈ [0,1]
```

---

## Thresholds:

* < 0.2 → ignorovať
* 0.2 – 0.5 → pasívne sledovať
* 0.5 – 0.7 → odporučiť
* > 0.7 → aktívne zapojiť

---

# 6. 🔄 Typy participácie

Participation Filter nerozhoduje len „áno / nie“.

Rozlišuje:

---

## 6.1 Passive

* len sledovanie

---

## 6.2 Reflective

* odpoveď / názor

---

## 6.3 Active

* diskusia
* argumenty

---

## 6.4 Committed

* záväzok
* realizácia

---

👉 toto je veľmi dôležité pre Project Layer

---

# 7. 🌐 Distribúcia

---

Participation Filter ovplyvňuje:

---

## 7.1 Incoming flow

Čo uzol dostane:

* otázky
* topicy
* projekty

---

## 7.2 Outgoing flow

Čo uzol pošle do siete:

* odpovede
* support
* argumenty

---

---

👉 Filter funguje obojsmerne

---

# 8. 🧠 Personal AI Role

Personal AI:

* počíta participation_score
* rozhoduje o zapojení
* vysvetľuje rozhodnutie používateľovi

---

Príklad:

👉 „Táto téma je pre teba relevantná, lebo sa týka tvojho mesta a máš skúsenosti s gastronómiou.“

---

---

# 9. ⚖️ Anti-Spam & Anti-Noise

---

Filter musí zabrániť:

* spam odpovediam
* nerelevantným vstupom
* „každý odpovedá na všetko“

---

Mechanizmy:

* relevance threshold
* history weighting
* low-impact users = nižšia distribúcia

---

---

# 10. 🔄 Adaptivita

Filter sa učí:

* z odpovedí používateľa
* z jeho správania
* z jeho rozhodnutí

---

👉 participation model je dynamický

---

# 11. ⚠️ Anti-Elitism Principle

---

Systém NESMIE:

* uzatvárať sa elitám
* blokovať nové hlasy

---

Musí zabezpečiť:

* občasné „exploration“ zapojenie
* nové perspektívy
* náhodný sampling

---

---

# 12. 🧠 Interaction with Topic Layer

---

Participation Filter:

* určuje, kto vidí topic
* určuje, kto odpovedá
* ovplyvňuje vznik projektu

---

👉 filter = gatekeeper reality

---

# 13. 🧠 Interaction with Project Layer

---

Participation Filter:

* určuje, kto sa zapojí do projektu
* ovplyvňuje support
* ovplyvňuje lifecycle vetiev

---

---

# 14. 📊 Dôsledky rozhodnutia

---

## ✅ Pozitíva

* menej šumu
* vyššia kvalita odpovedí
* škálovateľnosť
* personalizácia

---

## ❗ Negatíva

* komplexný model
* riziko „filter bubble“
* závislosť na AI kvalite

---

# 15. 🚫 Alternatívy

---

## ❌ Bez filtra

→ chaos

---

## ❌ Centrálne riadený filter

→ strata decentralizácie

---

---

# 16. 🔗 Väzby na ADR

* ADR-032: Question System
* ADR-031: Topic Layer
* ADR-022: Project Formation
* ADR-027: View Layer

---

# 17. 📏 Pravidlá implementácie

---

Systém musí:

* mať lokálny filter
* počítať participation_score
* podporovať rôzne typy participácie
* byť adaptívny
* zabrániť šumu

---

# 🔥 18. Zhrnutie

👉
**Participation Filter rozhoduje, kto sa zapojí do systému, a tým určuje kvalitu celej siete.**

---

# 🧭 Finálna veta

👉
**Nie všetci majú hovoriť do všetkého.
Ale každý musí mať šancu byť vypočutý tam, kde dáva zmysel.**

---
