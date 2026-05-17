Super — toto je presne moment, kde sa rozhoduje, či systém bude **žiť alebo sa rozpadne pod vlastnou infraštruktúrou**.

Nebudem ti dávať „pekné zhrnutie“. Dám ti **ostrý reality audit Infra vrstvy (I)** + presné kroky.

---

# 🔥 0. TL;DR (čo je najväčší problém)

👉 Infra máte **veľmi dobre navrhnutú koncepčne**
ALE:

> ❗ **nie je jasné, kde presne sa systém rozbije v realite**

Chýbajú:

* tvrdé limity
* konfliktné scenáre
* failure módy medzi komponentmi

---

# 🔴 1. Najväčší problém: Sync vs Event vs Runtime NIE SÚ zosúladené

---

## Máš 3 silné ADR:

* Event model → 
* Runtime → 
* Sync → 

---

## ❗ Problém:

Každý je dobrý sám o sebe, ale:

> ❌ nie je definované, ako spolu reálne interagujú

---

### Konkrétny conflict:

* Event system chce:
  → batching, delay, drop low priority 

* Sync chce:
  → immutable audit log, všetko zaznamenané 

---

👉 otázka:

> čo sa stane, keď event dropneš?

---

### ❗ kritická diera:

* stratíš event?
* alebo ho musíš vždy zapísať?

---

## 🔧 Čo spraviť

👉 vytvoriť:

### **ADR-I-004 — Event Integrity vs Performance Model**

Definuje:

* ktoré eventy:

  * MUST persist (audit)
  * MAY drop (UX only)
* dual pipeline:

  * UX events vs audit events

---

---

# 🔴 2. Parent Node je „pekne popísaný“, ale nie reálne použiteľný

---

## ADR-021 je čistý koncept 

Ale:

> ❗ chýba realita používateľa

---

## Problém:

> Koľko % userov bude mať parent node?

Reálne:

* 80–90% → NIE

---

## Dôsledok:

Celý systém musí fungovať:

> **bez parent node ako normálny stav**

---

## ❗ aktuálne implicitne:

* parent node = dôležitý
* ale realita = optional edge case

---

## 🔧 Čo spraviť

### doplniť do ADR-I-001:

> explicitný režim:

```
Mode A: Mobile-only (default realita)
Mode B: Mobile + Parent (power users)
```

---

👉 a hlavne:

> čo presne NEFUNGUJE bez parent node

---

---

# 🔴 3. Sync model je silný, ale nebezpečný

---

## ADR-030 je veľmi kvalitné 

Ale:

> ❗ má „Git-level komplexitu“

---

## Riziko:

* audit log
* branching
* fork
* event sourcing

👉 to je:

> **enterprise-grade distributed system**

---

## ❗ problém:

Ty chceš:

* mobile app
* rýchly pilot

---

👉 konflikt:

> komplexita vs realita

---

## 🔧 Čo spraviť

### vytvoriť:

👉 **ADR-I-005 — Sync Complexity Reduction Strategy**

---

### definovať:

#### Fáza 1:

* NO fork
* NO full audit chain
* len:

  * simple event log
  * snapshot overwrite

---

#### Fáza 2:

* audit log light
* branch limited

---

#### Fáza 3:

* full system

---

👉 teraz máš všetko naraz → to je pasca

---

---

# 🔴 4. Recovery (ADR-041) je silné, ale UX-killer

---

## ADR-041 je technicky super 

Ale:

> ❗ je to crypto wallet complexity

---

## Problém:

* shards
* trusted circle
* seed

---

👉 realita:

> user to NIKDY nenastaví

---

## 🔧 Čo spraviť

### zjednodušiť default:

```
DEFAULT:
- encrypted backup (1 miesto)
- optional export

ADVANCED:
- shards
- circle
```

---

👉 inak:

> recovery existuje len na papieri

---

---

# 🔴 5. Event model nemá definované „čo sa deje keď user je lazy“

---

## Máš krásne:

* priority
* batching
* scheduling 

---

Ale chýba:

> ❗ real user behavior

---

## Scenár:

* user spamuje kliky
* user nič nerobí
* user dá random odpovede

---

## Otázka:

> ako sa event system správa?

---

## 🔧 Čo spraviť

### doplniť:

👉 do ADR-I-003:

* event noise handling
* user reliability scoring (len lokálne)

---

---

# 🔴 6. Chýba jedna brutálne dôležitá vec

---

## 👉 „System Load Boundaries“

---

Nikde nemáš:

* max events/sec
* max queue size
* max sync frequency

---

👉 bez toho:

> systém sa môže zabiť sám

---

## 🔧 Čo spraviť

### vytvoriť:

👉 **ADR-I-006 — System Limits & Load Boundaries**

---

---

# 🟡 7. Chýba: observability (bez dát)

---

## Máš privacy-first

ALE:

> ❗ ako debugneš systém?

---

## 🔧 čo treba:

👉 **ADR-I-007 — Observability without Raw Data**

---

---

# 🟢 8. Čo je výborné (NECHAJ TAK)

---

## ✔ Runtime model

→ čistý, realistický 

## ✔ Compute budget

→ toto ťa zachráni 

## ✔ Event model základ

→ správny smer 

## ✔ Sync filozofia

→ veľmi silná 

---

# 🧭 PRESNÝ AKČNÝ PLÁN

---

## 🔴 MUST (bez toho sa to rozbije)

1. **ADR-I-004 — Event Integrity vs Performance**
2. **ADR-I-005 — Sync simplification (fázy)**
3. **Doplniť Mobile-only mode do runtime**

---

## 🟡 SHOULD (výrazne pomôže)

4. **ADR-I-006 — System limits**
5. **Zjednodušiť recovery default**

---

## 🟢 LATER

6. Observability
7. Noise handling refinement

---

# 🔥 Najdôležitejšia veta

> Infra sa nepokazí tým, čo nevie.
> Pokazí sa tým, čo robí príliš dobre — až príliš komplikovane.

---

# 💬 Brutálne úprimný záver

👉 Máš infra architektúru na úrovni:

> senior distributed systems návrhu

ALE:

> ešte nemáš infra pre **prvý použiteľný produkt**

---

Ak chceš, ďalší krok:

👉 spravím ti:

**„Infra MVP profile“ — čo PRESNE nechať a čo zahodiť pre prvý pilot**

To je rozdiel medzi:

* projektom, ktorý existuje
* projektom, ktorý sa robí 2 roky a nikdy nespustí
