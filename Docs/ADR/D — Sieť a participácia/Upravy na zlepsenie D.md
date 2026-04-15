Výborne — teraz si v vrstve, kde sa rozhoduje, či sa systém **rozpadne alebo začne žiť**.

👉 **D (Sieť a participácia)** je najviac „real-world layer“:

* tu narazíš na chaos ľudí,
* tu narazíš na sieťové limity,
* tu vzniká (alebo zlyhá) emergentné správanie.

A dám ti to priamo:

> Toto je zatiaľ **najviac podcenená vrstva v projekte**.

---

# 🧠 TL;DR — hlavný problém D vrstvy

Na základe:

*  (ADR-014 P2P)
*  (ADR-033 Filter)
*  (ADR-016 hardened)
*  (ADR-038 power)
*  (ADR-017 collaboration)
*  (zoznam)

👉 máš:

✔ krásnu filozofickú decentralizáciu
✔ správny human-first model
✔ dobrý participation filter

ALE:

> ❗ systém predpokladá „čistú sieť“ — realita je **špinavá, nestabilná a útočná**

---

# 🔥 7 KRITICKÝCH PROBLÉMOV (realita vs architektúra)

---

## 🔴 1. P2P je ideál… ale nemáš infra realitu

### Stav

ADR-014:

> „uzly komunikujú priamo, bez centrálnej autority“ 

---

### Problém

> ❗ P2P v reálnom svete:

* NAT
* firewally
* offline devices
* mobilné siete

---

### Chýba

👉 **ADR-065 — Network Reality Model (Connectivity & Availability)**

---

### Čo musí definovať

* % času offline (mobil user = často offline)
* fallback:

  * relay nodes
  * delayed sync
* typy uzlov:

  * always-on (parent)
  * intermittent (mobile)

---

👉 Bez toho:

> P2P = teória, nie systém

---

## 🔴 2. Participation filter = single point of failure

### Stav

Participation filter riadi všetko 

---

### Problém

> ❗ ak filter zlyhá:

* systém je prázdny
* alebo spamovaný

---

### Chýba

👉 **ADR-066 — Participation Filter Failure Modes**

---

### Čo musí definovať

* underfilter:
  → spam, noise
* overfilter:
  → dead network

Mechanizmy:

* adaptive threshold
* fallback sampling
* emergency widening

---

👉 Bez toho:

> systém sa zablokuje alebo zahltí

---

## 🔴 3. Nemáš Sybil attack protection

### Stav

Decentralizácia + anonymita

---

### Problém

> ❗ user si vytvorí 100 identít

---

### Následok

* manipuluje topics
* manipuluje projekty
* manipuluje consensus

---

### Chýba

👉 **ADR-067 — Identity Trust & Sybil Resistance**

---

### Čo musí definovať

* identity cost (čas, história, participácia)
* trust ≠ account
* reputation = behavior

---

👉 Bez toho:

> celý systém je hacknuteľný za 10 minút

---

## 🔴 4. Nemáš rate limiting (network spam)

### Stav

ADR-016 distribuuje otázky selektívne ✔️ 

---

### Problém

> ❗ nič nebráni:

* spam otázkam
* spam odpovediam

---

### Chýba

👉 **ADR-068 — Network Rate Limiting & Cost Model**

---

### Čo musí definovať

* cost za:

  * otázku
  * odpoveď
  * broadcast
* soft limits:

  * per user
  * per node

---

👉 Bez toho:

> systém sa utopí v noise

---

## 🔴 5. Collaboration model je príliš optimistický

### Stav

„správny človek + správna úloha = spolupráca“ 

---

### Problém

> ❗ realita:

* ľudia ignorujú návrhy
* nemajú čas
* nechcú commitment

---

### Chýba

👉 **ADR-069 — Collaboration Reality Model**

---

### Čo musí definovať

* acceptance rate (10–30% max)
* ghosting
* dropout

Mechanizmy:

* retry logic
* fallback users
* redundancy

---

👉 Bez toho:

> projekty budú teoretické

---

## 🔴 6. Power balancing (ADR-038) je správny… ale prázdny

### Stav

Dobrá filozofia ✔️ 

---

### Problém

> ❗ nemáš mechanizmy

---

### Chýba

👉 **ADR-070 — Practical Influence Signals**

---

### Čo musí definovať

* influence = kombinácia:

  * argument quality
  * consistency
  * execution
* NIE:

  * hlasitosť
  * frekvencia

---

👉 Bez toho:

> vznikne skrytá elita

---

## 🔴 7. Missing network health model

### Problém

> ❗ nevieš:

* či sieť funguje
* či degraduje
* či je manipulovaná

---

### Chýba

👉 **ADR-071 — Network Health & Integrity Monitoring**

---

### Čo musí definovať

* metrics:

  * response diversity
  * participation entropy
  * topic conversion rate
* anomaly detection

---

👉 Bez toho:

> budeš slepý

---

# 🟠 3 VEĽKÉ SLEPÉ MIESTA

---

## 🟠 8. Bootstrap problém siete

Ako vznikne prvých 100 users?

👉 chýba:

**ADR-072 — Network Bootstrapping Strategy**

---

## 🟠 9. Trust medzi neznámymi ľuďmi

Ako veríš cudziemu node?

👉 chýba:

**ADR-073 — Inter-Node Trust Model**

---

## 🟠 10. Latencia vs UX

P2P = pomalé

👉 chýba:

**ADR-074 — Latency & Async Interaction Model**

---

# 🟢 ČO JE EXTRÉMNE DOBRÉ (nemen)

---

✔ human-first odpovede
✔ no central authority
✔ participation filter
✔ targeted distribution (nie broadcast) 
✔ collaboration cez roles 

---

👉 toto je veľmi silné jadro

---

# Upravy na zlepsenie D — Network & Participation (renumbered)

## Kontext

Oblasť D rieši realitu siete, participácie a emergentného správania.

Cieľ:
> zabezpečiť, aby sieť fungovala v chaotickej realite, nie len v ideálnom modeli

---

# 🔴 MUST — Kritické doplnenia

---

## ADR-065 — Network Reality Model

- offline nodes
- intermittent connectivity
- relay / fallback nodes
- async communication

---

## ADR-066 — Participation Filter Failure Modes

- underfilter → spam
- overfilter → dead network
- adaptive thresholds

---

## ADR-067 — Identity Trust & Sybil Resistance

- identity cost
- behavior-based trust
- anti-multi-account

---

## ADR-068 — Network Rate Limiting & Cost Model

- limit otázok
- limit odpovedí
- cost za distribúciu

---

## ADR-069 — Collaboration Reality Model

- acceptance rate
- dropout
- ghosting
- fallback matching

---

## ADR-070 — Practical Influence Signals

- argument quality > activity
- execution > opinion
- consistency weighting

---

## ADR-071 — Network Health Monitoring

- participation entropy
- diversity
- anomaly detection

---

# 🟠 SHOULD

---

## ADR-072 — Network Bootstrapping

- first users
- early density
- initial topics

---

## ADR-073 — Inter-Node Trust Model

- trust signals medzi neznámymi
- local trust graphs

---

## ADR-074 — Latency & Async Model

- delayed responses
- batching
- async UX

---

# 🧭 Zhrnutie

## MUST:
- 065 — Network reality
- 066 — Filter failure
- 067 — Sybil
- 068 — Rate limiting
- 069 — Collaboration reality
- 070 — Influence signals
- 071 — Network health

## SHOULD:
- 072 — Bootstrap
- 073 — Trust
- 074 — Latency

---

# 🔥 Finálna veta

> Sieť nezlyhá na algoritme.
> Zlyhá na ľuďoch, útokoch a chaose.

---

# 🔥 Najtvrdšia pravda pre D vrstvu

> Ty neriešiš „ako funguje sieť“.
> Ty riešiš „ako prežije sieť“.

---

# 🧭 Čo spraviť teraz (najvyššie ROI)

1. **ADR-067 (Sybil resistance)** → bezpečnosť
2. **ADR-065 (Network reality)** → technická realizovateľnosť
3. **ADR-069 (Collaboration reality)** → či vzniknú projekty

---

Ak chceš ďalší krok:

👉 spravíme **ADR-067 (Identity Trust & Sybil Resistance)**
to je najnepríjemnejší, ale najkritickejší dokument v celej D vrstve.
