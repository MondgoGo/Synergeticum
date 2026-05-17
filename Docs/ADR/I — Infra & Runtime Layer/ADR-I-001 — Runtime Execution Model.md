# ADR-I-001 — Runtime Execution Model

## 1. 📌 Status  
Proposed

---

## 2. 🎯 Kontext

Synergetikum je:

- decentralizovaný systém  
- personal AI per používateľ  
- local-first architektúra  

Základná otázka:

> **Kde presne beží inteligencia systému?**

Možnosti:

- mobile device  
- parent node (desktop / always-on)  
- hybrid kombinácia  

---

## ❗ Problém

Bez jasného runtime modelu:

- nie je jasné:
  - kde prebieha learning  
  - kde prebieha inference  
  - kde sa robí heavy compute  

- vzniká chaos:
  - duplicita výpočtov  
  - nekonzistentné správanie  
  - nepredvídateľná latencia  

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Core princíp

---

> **Personal AI je primárne lokálna (mobile-first), s voliteľným rozšírením cez parent node pre náročné operácie.**

---

👉 systém je:

- default: **mobile runtime**
- rozšírenie: **parent node assist**
- NIE: cloud-first

---

---

## 🧩 3.2 Runtime vrstvy

---

### 📱 1. Mobile Runtime (default)

---

#### Zodpovednosti:

- user interaction  
- lightweight inference  
- základné učenie  
- lokálny profil  
- rýchle rozhodnutia  

---

#### Charakter:

- always-available  
- low-latency  
- obmedzený compute  

---

👉 Mobile je:

> **primary execution environment**

---

---

### 🖥️ 2. Parent Node Runtime (voliteľný)

---

#### Zodpovednosti:

- heavy learning  
- dlhodobá analýza  
- simulácie  
- agregácie  
- model refinement  

---

#### Charakter:

- always-on (ideálne)  
- vyšší výkon  
- menej latency-sensitive  

---

👉 Parent node je:

> **compute extension, nie authority**

---

---

### 🔁 3. Hybrid režim

---

Systém dynamicky rozdeľuje úlohy:

| Operácia | Mobile | Parent |
|----------|--------|--------|
| quick answer | ✅ | ❌ |
| UI interaction | ✅ | ❌ |
| learning (short-term) | ✅ | ❌ |
| learning (long-term) | ❌ | ✅ |
| simulation | ❌ | ✅ |
| heavy analysis | ❌ | ✅ |

---

---

## 🧠 3.3 Learning model distribúcia

---

### Mobile:

- okamžité učenie  
- reakcia na input  
- krátkodobé signály  

---

### Parent node:

- batch learning  
- stabilizácia modelu  
- noise filtering  

---

---

## 🧠 3.4 Inference model

---

### Mobile:

- rýchle odpovede  
- low-cost model  
- fallback always available  

---

### Parent:

- hlboké analýzy  
- komplexné simulácie  

---

---

## 🧠 3.5 Sync medzi runtime vrstvami

---

- mobile → parent:
  - event stream  
  - learning signals  

- parent → mobile:
  - model updates  
  - refined state  

---

👉 princíp:

> **event-based sync, nie state overwrite**

---

---

## 🔒 3.6 Privacy & Security princíp

---

- parent node nemá implicitný prístup k raw dátam  
- všetko je:
  - šifrované  
  - user-controlled  

---

👉 parent node je:

> **rozšírenie používateľa, nie tretia strana**

---

---

## ⚠️ 3.7 Fallback správanie

---

### Ak parent node neexistuje:

- systém funguje plne na mobile  
- len:
  - nižšia kvalita analýzy  
  - pomalší learning  

---

👉 systém je:

> **degraded, nie broken**

---

---

## 🧠 3.8 Offline režim

---

- mobile funguje offline  
- všetky operácie:
  - lokálne  
  - queue-ované  

---

- sync:
  - až keď je dostupný parent alebo sieť  

---

---

## 🚫 3.9 Zakázané modely

---

### ❌ Cloud-first runtime

- porušuje ownership  
- porušuje privacy  

---

### ❌ Central inference

- porušuje decentralizáciu  

---

### ❌ Parent node ako autorita

- parent nesmie rozhodovať  
- parent nesmie override-núť user model  

---

---

## 🧠 3.10 Trade-offs

---

## ✅ Pozitíva

- vysoká kontrola používateľa  
- offline schopnosť  
- škálovateľnosť  

---

## ❗ Negatíva

- komplexnejšia implementácia  
- sync problémy  
- rozdiely medzi device capabilities  

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

- Mobile musí byť:
  - dostatočne silný na základné AI  

- Parent node:
  - voliteľný, ale výrazne zlepšuje kvalitu  

- UX musí:
  - skryť rozdiel medzi mobile a parent  

---

---

## 5. 🔗 Väzby na ďalšie ADR

---

- ADR-001 — Ownership  
- ADR-006 — Learning Model  
- ADR-030 — Sync & Persistence  
- ADR-021 — Parent Node Architecture  
- ADR-041 — Recovery Strategy  

---

---

# 🔥 6. Zhrnutie

---

👉  
**AI v Synergetiku beží primárne na mobile, parent node ju rozširuje, ale nikdy ju nepreberá.**

---

---

# 🧭 Finálna veta

---

👉  
**Používateľ vlastní AI preto, lebo ju môže spustiť aj bez akejkoľvek infraštruktúry.**