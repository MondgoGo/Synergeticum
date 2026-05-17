# ADR-I-003 — Event Processing & Scheduling Model

## 1. 📌 Status  
Proposed

---

## 2. 🎯 Kontext

Synergetikum je:

- event-driven systém  
- local-first  
- distribuovaný  
- mobile-first runtime (ADR-I-001)  

Väčšina systému funguje ako:

> user action → event → processing → state update → output

---

## ❗ Problém

Bez definovaného event modelu:

- vzniká:
  - nekontrolovaný event storm  
  - preťaženie CPU  
  - lag UI  

- systém:
  - spracováva všetko naraz  
  - nemá prioritu  
  - nie je predvídateľný  

---

👉 najčastejší failure mód:

> „systém robí príliš veľa vecí naraz“

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Core princíp

---

> **Systém spracováva udalosti postupne, prioritne a len v rozsahu, ktorý neohrozuje UX.**

---

👉 nie všetky eventy sú rovnaké  
👉 nie všetky eventy musia byť spracované okamžite  

---

---

## 🧩 3.2 Typy eventov

---

### 🔴 1. Interaction events (high priority)

- user odpoveď  
- klik  
- selection  

---

👉 charakter:

- musí byť okamžite spracovaný  
- ovplyvňuje UX  

---

---

### 🟡 2. System events (medium priority)

- model update  
- signal propagation  
- scoring  

---

👉 charakter:

- môže byť delayed  
- nemusí blokovať UI  

---

---

### 🟢 3. Background events (low priority)

- batch learning  
- analytics  
- recomputation  

---

👉 charakter:

- len keď device idle  
- môže byť odložený  

---

---

## 🧠 3.3 Scheduling model

---

### Priority queue

Eventy sú spracovávané podľa priority:

1. Interaction  
2. System  
3. Background  

---

---

### Time slicing

- spracovanie eventov má limit:
  - napr. 10–20 ms per slice  

---

👉 zabezpečuje:

- plynulosť UI  
- žiadne blokovanie  

---

---

### Deferred processing

- niektoré eventy:
  - sa spracujú neskôr  
  - alebo v batchi  

---

---

## 🧠 3.4 Event batching

---

### Princíp:

> viac eventov sa spojí do jednej operácie

---

Použitie:

- learning  
- scoring  
- propagation  

---

👉 cieľ:

- znížiť compute  
- zvýšiť efektivitu  

---

---

## 🧠 3.5 Event coalescing

---

### Princíp:

> podobné eventy sa zlúčia

---

Príklad:

- 10 zmien → 1 update  

---

👉 cieľ:

- zabrániť event explosion  

---

---

## 🧠 3.6 Event persistence

---

- eventy sú:
  - ukladané (event log)  
  - replayable  

---

👉 princíp:

> systém môže byť rekonštruovaný z eventov  

---

---

## 🧠 3.7 Failure handling

---

### Ak:

- event zlyhá  

---

Systém:

- retry (limitovaný)  
- fallback  
- log  

---

---

### Ak:

- queue je preplnená  

---

Systém:

- drop low-priority eventy  
- zachová high-priority  

---

---

## 🧠 3.8 Backpressure model

---

### Princíp:

> systém spomaľuje generovanie eventov, keď nestíha spracovanie

---

Použitie:

- limit otázok  
- limit odpovedí  
- limit distribúcie  

---

---

## 🧠 3.9 Sync eventov

---

- eventy sa:
  - posielajú medzi node-mi  
  - spracovávajú async  

---

👉 princíp:

> eventual consistency, nie immediate consistency  

---

---

## 🧠 3.10 Offline režim

---

- eventy sa:
  - ukladajú lokálne  
  - synchronizujú neskôr  

---

---

## 🚫 3.11 Anti-patterns

---

### ❌ Immediate processing všetkého

- preťaženie systému  

---

### ❌ No prioritization

- UI lag  

---

### ❌ Infinite recomputation

- compute explosion  

---

---

## 🧠 3.12 Trade-offs

---

## ✅ Pozitíva

- stabilita  
- plynulosť  
- kontrola nad compute  

---

## ❗ Negatíva

- eventual consistency  
- delayed updates  

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

- všetky ADR:
  - musia byť event-driven  

---

- všetky výpočty:
  - musia rešpektovať scheduling  

---

👉 ak nie:

> systém sa rozpadne na runtime úrovni  

---

---

## 5. 🔗 Väzby na ďalšie ADR

---

- ADR-I-001 — Runtime Execution Model  
- ADR-I-002 — Compute Budget  
- ADR-030 — Sync & Persistence  
- ADR-040 — Graceful Degradation  

---

---

# 🔥 6. Zhrnutie

---

👉  
**Systém nerobí všetko hneď — robí správne veci v správnom čase.**

---

---

# 🧭 Finálna veta

---

👉  
**Eventy riadia systém — scheduling rozhoduje, či prežije.**