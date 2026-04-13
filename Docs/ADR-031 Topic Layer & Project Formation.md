# 🧾 ADR-031: Topic Layer & Project Formation

---

# 1. 📌 Status

**Proposed**

---

# 2. 🎯 Kontext

Synergetikum pracuje s:

* otázkami (questions)
* odpoveďami (responses)
* projektmi (projects)

---

### Problém

Priamy prechod:

```text
otázka → projekt
```

je:

* príliš skokový
* nerealistický
* vedie k chaosu alebo rigidite

---

V reálnom svete existuje medzi tým:

👉 diskusia
👉 zvažovanie
👉 kolektívne myslenie

---

### Kľúčová otázka

👉
**Ako modelovať prechod od otázky k projektu tak, aby bol prirodzený, škálovateľný a použiteľný?**

---

# 3. ⚖️ Rozhodnutie

Systém zavádza novú vrstvu:

👉 **Topic Layer (Emerging Topic Objects)**

---

Táto vrstva reprezentuje:

> **kolektívne diskusné objekty vznikajúce okolo otázok, ktoré môžu, ale nemusia viesť k projektu**

---

# 4. 🧠 Definícia Topic Object

Topic je:

* distribuovaný objekt
* vznikajúci z otázky
* obsahujúci odpovede a signály
* bez nutnosti záväzku

---

Topic NIE JE:

* projekt
* záväzok
* plán

---

Topic JE:

* explorácia
* diskusia
* zber perspektív

---

# 5. 🧩 Štruktúra Topic Object

Topic obsahuje:

---

## 5.1 Identity

* topic_id
* origin_question
* autor otázky
* timestamp

---

## 5.2 Responses

* odpovede používateľov
* agregované odpovede
* confidence / relevance

---

## 5.3 Aggregates

* interest_level
* engagement_score
* response_count

---

## 5.4 Signals

* early_commitment
* feasibility_estimate
* potential_project_type

---

## 5.5 Lifecycle

* new
* emerging
* active
* candidate_for_project
* converted_to_project
* faded

---

# 6. 🔄 Lifecycle Topicu

---

## 6.1 new

* otázka vznikla

---

## 6.2 emerging

* začínajú odpovede

---

## 6.3 active

* diskusia rastie

---

## 6.4 candidate_for_project

* vzniká záujem
* objavuje sa commitment

---

## 6.5 converted_to_project

* vzniká projekt

---

## 6.6 faded

* záujem klesol

---

👉 väčšina topicov skončí v `faded`

---

# 7. 🚀 Project Formation

Projekt vzniká z topicu, nie priamo z otázky.

---

## Trigger conditions (príklad)

* interest_level > threshold
* commitment > threshold
* existujú konkrétni participanti
* existuje smer (branching candidate)

---

👉
Topic → Project

---

# 8. 🔗 Prepojenie Topic → Project

Projekt obsahuje:

* `origin_topic_id`

---

Topic obsahuje:

* referenciu na projekt (ak vznikol)

---

👉 Topic zostáva ako:

* historický kontext
* zdroj argumentov
* zdroj rozhodnutí

---

# 9. 🌐 Distribúcia Topicov

Topic:

* existuje distribuovane
* je replikovaný medzi relevantnými uzlami
* synchronizuje sa cez eventy

---

Synchronizujú sa:

* nové odpovede
* agregované signály
* lifecycle zmeny

---

# 10. 🧠 Úloha Personal AI

Personal AI:

* vyberá relevantné topicy
* filtruje šum
* sumarizuje diskusiu
* navrhuje zapojenie
* rozhoduje o participácii

---

👉 používateľ nikdy nevidí všetky topicy

---

# 11. ⚖️ Rozdiel Topic vs Project

| Vlastnosť | Topic  | Project |
| --------- | ------ | ------- |
| Záväzok   | ❌      | ✅       |
| Diskusia  | ✅      | ⚠️      |
| Akcia     | ❌      | ✅       |
| Stabilita | nízka  | vyššia  |
| Lifecycle | krátky | dlhší   |

---

# 12. 📊 Dôsledky rozhodnutia

---

## ✅ Pozitíva

### 12.1 Realistický model myslenia

Zodpovedá realite (ľudia najprv diskutujú).

---

### 12.2 Škálovanie systému

Zabraňuje preťaženiu projektovej vrstvy.

---

### 12.3 Lepší výber projektov

Projekt vzniká až po overení záujmu.

---

---

## ❗ Negatíva

### 12.4 Nová vrstva komplexity

Pridáva ďalší typ objektu.

---

### 12.5 Potreba dobrého filtrovania

Bez Personal AI vznikne šum.

---

# 13. 🚫 Alternatívy (zamietnuté)

---

## ❌ Priamy model (question → project)

* príliš rigidný
* nereflektuje realitu

---

## ❌ Len otázky bez ukladania

* stráca sa kolektívne myslenie
* nedá sa prejsť do projektu

---

---

# 14. 🔗 Väzby na ADR

* ADR-022: Project Formation
* ADR-023: Project Object Model
* ADR-030: Sync & Persistence
* ADR-015: Participation Filter

---

# 15. 📏 Pravidlá implementácie (high-level)

Systém musí:

* ukladať topic objekty distribuovane
* umožniť ich lifecycle
* agregovať odpovede
* detekovať vznik projektu
* prepojiť topic a project

---

# 🔥 16. Zhrnutie

👉
**Topic Layer je medzivrstva medzi otázkou a projektom, ktorá umožňuje kolektívne myslenie bez záväzku a slúži ako inkubátor projektov.**

---

# 🧭 Finálna veta

👉
**Projekty nevznikajú z otázok.
Vznikajú z diskusií, ktoré dosiahnu dostatočný záujem a záväzok.**

---
