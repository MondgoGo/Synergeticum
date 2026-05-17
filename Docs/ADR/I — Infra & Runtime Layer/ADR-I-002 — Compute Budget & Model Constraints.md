# ADR-I-002 — Compute Budget & Model Constraints

## 1. 📌 Status  
Proposed

---

## 2. 🎯 Kontext

Synergetikum je:

- mobile-first systém  
- decentralizovaný  
- personal AI per používateľ  

Podľa ADR-I-001:

> AI beží primárne na mobile, parent node je len rozšírenie

---

## ❗ Problém

Bez definovaného compute budgetu:

- model sa stane:
  - príliš veľký  
  - príliš pomalý  
  - energeticky náročný  

- vznikne:
  - lag  
  - vysoká spotreba batérie  
  - zlá UX  

---

👉 najčastejší failure mód:

> „funguje na desktop prototype, nefunguje na mobile“

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Core princíp

---

> **Model a výpočty sú navrhované podľa limitov zariadenia, nie podľa ideálnej presnosti.**

---

👉 priorita:

1. responsiveness  
2. battery  
3. reliability  
4. až potom accuracy  

---

---

## 🧩 3.2 Compute budget — Mobile

---

### 📱 Latency budget

- UI response: **< 100 ms**
- AI inference (quick): **< 300 ms**
- background computation: **< 1–2 s (async)**

---

### 🔋 Energy budget

- žiadne:
  - continuous heavy compute  
  - background loops bez triggeru  

- learning:
  - event-driven  
  - batchovaný  

---

### 💾 Memory budget

- model musí:
  - fitnúť do RAM bez swapu  
  - bežať bez GC spike  

---

👉 princíp:

> **Mobile nikdy nesmie „cítiť AI“ ako záťaž**

---

---

## 🖥️ 3.3 Compute budget — Parent Node

---

### Zodpovednosti:

- heavy analysis  
- batch learning  
- simulation  

---

### Limity:

- môže byť vyšší compute  
- ale:
  - nesmie byť required  
  - nesmie blokovať mobile  

---

👉 princíp:

> **Parent node zlepšuje kvalitu, ale nikdy nie je nutný**

---

---

## 🧠 3.4 Model constraints

---

### 🔹 1. Model size

- musí byť:
  - kompaktný  
  - modulárny  

---

### 🔹 2. Model type

Preferované:

- jednoduché modely  
- heuristiky  
- lightweight ML  

---

Nežiadúce:

- veľké LLM lokálne  
- komplexné grafové inference v reálnom čase  

---

---

### 🔹 3. Incremental learning

- model sa učí:
  - postupne  
  - lokálne  

---

👉 zakázané:

- full retraining na mobile  

---

---

## 🧠 3.5 Scheduling princíp

---

### Mobile:

- priority:
  1. UI  
  2. interaction  
  3. AI  

---

### Background:

- len:
  - keď je device idle  
  - keď je charging  

---

---

## 🧠 3.6 Graceful degradation

---

Ak:

- device je slabý  
- batéria nízka  

---

Systém:

- znižuje:
  - model complexity  
  - frequency výpočtov  

---

👉 nikdy:

- nespadne  
- nezablokuje UI  

---

---

## 🧠 3.7 Offline-first constraint

---

- všetky kritické operácie:
  - musia fungovať offline  

---

👉 zakázané:

- závislosť na:
  - cloude  
  - parent node  

---

---

## 🚫 3.8 Anti-patterns

---

### ❌ „Desktop-first thinking“

- navrhovať model bez mobile limitov  

---

### ❌ „Max accuracy over UX“

- pomalý systém  
- vysoká spotreba  

---

### ❌ „Compute explosion“

- príliš veľa prepočtov  
- re-evaluation všetkého  

---

---

## 🧠 3.9 Trade-offs

---

## ✅ Pozitíva

- rýchly systém  
- použiteľný v realite  
- škálovateľný  

---

## ❗ Negatíva

- nižšia presnosť modelu  
- zjednodušenia  
- viac heuristík  

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

- model musí byť:
  - navrhnutý pre mobile  
- všetky ADR:
  - musia rešpektovať compute limity  

---

👉 ak nie:

> ADR je neimplementovateľné

---

---

## 5. 🔗 Väzby na ďalšie ADR

---

- ADR-I-001 — Runtime Execution Model  
- ADR-006 — Learning Model  
- ADR-011 — Personalization  
- ADR-040 — Graceful Degradation  

---

---

# 🔥 6. Zhrnutie

---

👉  
**Model nie je limitovaný tým, čo je možné, ale tým, čo je použiteľné.**

---

---

# 🧭 Finálna veta

---

👉  
**Ak AI spomaľuje používateľa, je to bug — nie feature.**