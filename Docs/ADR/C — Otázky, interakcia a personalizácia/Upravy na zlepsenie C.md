Perfektné. Teraz už si v najdôležitejšej vrstve celého systému.

👉 **C (Otázky, interakcia, personalizácia)** je miesto, kde sa rozhodne:

* či ľudia vôbec budú používať systém,
* či vzniknú dáta,
* či vzniknú projekty.

A poviem ti rovno bez obalu:

> Toto je zatiaľ tvoja **najsilnejšia aj najnebezpečnejšia vrstva zároveň**.

---

# 🧠 TL;DR – čo je hlavný problém C vrstvy

Na základe:

*  (ADR-009)
*  (ADR-010)
*  (ADR-011)
*  (ADR-012)
*  (ADR-013)
*  (ADR-032)

👉 máš:

✔ veľmi silnú architektúru
✔ logickú pipeline (question → signal → topic → project)
✔ dobré princípy (no manipulation, personalization ≠ filtering)

ALE:

> ❗ je to navrhnuté ako **kognitívny systém**, nie ako **produkt pre unaveného človeka**

---

# 🔥 6 KRITICKÝCH PROBLÉMOV (realita vs dokumentácia)

---

## 🔴 1. „Question fatigue“ problém (toto ťa zabije ako prvé)

### Stav

ADR-009 a ADR-032 predpokladajú:

* veľa otázok
* premyslené odpovede
* engagement

👉 

---

### Realita

User:

* nechce odpovedať
* je unavený
* ignoruje

---

### Problém

> ❗ systém potrebuje odpovede → user ich nedáva

---

### Chýba

👉 **ADR-057 — Interaction Friction & Energy Model**

---

### Čo musí definovať

* koľko otázok denne je OK (1–3 max)
* čo keď user ignoruje
* čo je „low effort answer“

Mechanizmy:

* swipe / quick reactions
* skip ≠ failure
* pasívny signál (scroll, time)

---

👉 Bez toho:

> systém sa zmení na dotazník → smrť

---

## 🔴 2. „Answer vs Reality gap“

### Stav

Model stojí na odpovediach

---

### Problém

> ❗ ľudia hovoria niečo iné, než robia

---

### Príklad

* „chcem podnikať“
* nič neurobí

---

### Chýba

👉 **ADR-058 — Answer vs Behavior Reconciliation**

---

### Čo musí definovať

* explicit vs implicit signál
* behavior > deklarácia
* decay deklarácií bez akcie

---

👉 Bez toho:

> Personal AI bude klamstvo

---

## 🔴 3. Question quality = bottleneck celého systému

### Stav

ADR-032 rieši generovanie ✔️
👉 

---

### Problém

> ❗ nemáš mechanizmus kvality otázok

---

### Realita

* 70% otázok bude zlých
* AI bude generovať šum

---

### Chýba

👉 **ADR-059 — Question Quality & Evolution System**

---

### Čo musí definovať

* scoring otázok:

  * response rate
  * completion
  * downstream impact (topic/project)
* auto-killing slabých otázok
* mutácie (reformulation)

---

👉 Bez toho:

> garbage in → garbage system

---

## 🔴 4. Personalization → stále riskuje filter bubble

### Stav

Máš:

* exploration ✔️
* anti-bias ✔️
  👉 

---

### Problém

> ❗ nie je definované minimum reality exposure

---

### Chýba

👉 **ADR-060 — Reality Exposure Guarantee**

---

### Čo musí definovať

* min % non-aligned otázok (napr. 20%)
* forced perspective switching
* „discomfort layer“

---

👉 Bez toho:

> systém sa stane jemnou verziou TikTok algoritmu

---

## 🔴 5. Missing „moment of value“ (kritické pre adopciu)

### Stav

Pipeline:

question → answer → topic → project
👉 

---

### Problém

> ❗ value prichádza neskoro

---

### Realita

User chce:

> „čo z toho mám TERAZ?“

---

### Chýba

👉 **ADR-061 — Immediate Value Loop**

---

### Čo musí definovať

Po každej odpovedi user dostane:

* insight
* summary
* odporúčanie
* mini value

---

👉 Bez toho:

> user odíde pred topicom

---

## 🔴 6. Commitment model je správny… ale UX je slabý

### Stav

ADR-013 je výborný ✔️
👉 

---

### Problém

> ❗ commitment je kognitívne drahý

---

### Chýba

👉 **ADR-062 — Progressive Commitment UX**

---

### Čo musí definovať

* micro-commitments:

  * „chcem sledovať“
  * „chcem pomôcť“
  * „chcem spraviť“
* postupný prechod

---

👉 Bez toho:

> máš binary jump → user neurobí nič

---

# 🟠 2 VEĽKÉ ARCHITEKTONICKÉ DIERY

---

## 🟠 7. Question selection vs attention economy

ADR-011 rieši selection ✔️
👉 

ALE:

> ❗ nerieši konkurenciu s realitou (TikTok, IG, práca)

---

👉 potrebuješ:

**ADR-063 — Attention Competition Model**

---

## 🟠 8. Missing failure modes v interakcii

Čo keď:

* user ignoruje všetko
* odpovedá náhodne
* odíde

---

👉 potrebuješ:

**ADR-064 — Interaction Failure Handling**

---

# 🟢 ČO JE EXTRÉMNE SILNÉ (nemen!)

---

## ✔️ Question-first model

👉 

## ✔️ Perspective system ako entry points

👉 

## ✔️ Personalization = ordering, nie filtering

👉 

## ✔️ Topic → Project pipeline

👉 

## ✔️ Participation gradient

👉 

---

👉 Toto je tvoj core edge. Toto nemen.

---

# Upravy na zlepsenie C — Questions, Interaction & Personalization (renumbered)

## Kontext

Oblasť C je jadro interakcie systému.

Cieľ:
> zabezpečiť, aby systém fungoval pre reálneho používateľa (unaveného, pasívneho, chaotického)

---

# 🔴 MUST — Kritické doplnenia

---

## ADR-057 — Interaction Friction & Energy Model

### Problém
Používateľ nechce odpovedať na otázky.

### Čo doplniť
- max 1–3 otázky denne
- low-effort odpovede (swipe, quick select)
- skip ≠ failure
- pasívne signály (time, scroll)

---

## ADR-058 — Answer vs Behavior Reconciliation

### Problém
Používateľ hovorí ≠ robí

### Čo doplniť
- explicit vs implicit signal
- behavior overrides deklaráciu
- decay nevalidovaných odpovedí

---

## ADR-059 — Question Quality & Evolution System

### Problém
Slabé otázky zničia systém

### Čo doplniť
- scoring:
  - response rate
  - engagement
  - downstream impact
- auto-removal slabých otázok
- reformulation loop

---

## ADR-060 — Reality Exposure Guarantee

### Problém
Filter bubble risk

### Čo doplniť
- min 20% non-aligned content
- forced perspective diversity
- anti-confirmation bias layer

---

## ADR-061 — Immediate Value Loop

### Problém
Value prichádza neskoro

### Čo doplniť
Po každej odpovedi:
- insight
- summary
- odporúčanie
- mini value

---

## ADR-062 — Progressive Commitment UX

### Problém
Commitment je príliš veľký skok

### Čo doplniť
- micro-commitments
- gradual escalation
- low-risk entry

---

# 🟠 SHOULD

---

## ADR-063 — Attention Competition Model

### Problém
Systém súťaží s inými aplikáciami

### Čo doplniť
- time-to-engage
- friction budget
- retention heuristics

---

## ADR-064 — Interaction Failure Handling

### Problém
Používateľ neinteraguje

### Čo doplniť
- fallback modes
- reset flows
- re-engagement triggers

---

# 🧭 Zhrnutie

## MUST:
- 057 — Friction
- 058 — Behavior vs Answer
- 059 — Question Quality
- 060 — Reality Exposure
- 061 — Immediate Value
- 062 — Commitment UX

## SHOULD:
- 063 — Attention
- 064 — Failure

---

# 🔥 Finálna veta

> Systém nevyhrá tým, že kladie správne otázky.
> Vyhrá tým, že ľudia na ne odpovedajú.

---

# 🔥 Najtvrdšia pravda pre C vrstvu

> Ty neriešiš „ako klásť otázky“.
> Ty riešiš „prečo by niekto odpovedal“.

A to sú dve úplne iné problémy.

---

# 🧭 Čo spraviť teraz (konkrétne)

1. **ADR-057 (Friction)** – úplne prvé
2. **ADR-061 (Immediate value)** – adopcia
3. **ADR-059 (Question quality)** – kvalita systému

---

Ak chceš, ďalší krok:

👉 spravíme spolu **ADR-057 (Interaction Friction)**
to je najviac „produktový“ dokument z celého projektu a rozhodne, či to prežije realitu.
