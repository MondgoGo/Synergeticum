# Upravy na zlepsenie B — Personal AI & User Model (renumbered)

## Kontext

Oblasť B (Personal AI a používateľský model) je architektonicky silná, ale optimalizovaná pre ideálneho používateľa.

Cieľ týchto úprav:

> posunúť dokumentáciu smerom k realite používania, nie k modelovej dokonalosti

---

# 🔴 MUST — Kritické doplnenia (blokujú realitu použitia)

---

## ADR-050 — Identity Noise & Instability Model

### Problém
Model predpokladá konzistentného používateľa.

Realita:
- používateľ je nekonzistentný
- mení nálady, kontexty, priority
- produkuje šum

### Čo doplniť

Typy nestability:
- mood drift
- context switching
- identity fragmentation

Mechanizmy:
- krátkodobý signál ≠ zmena identity
- trust horizon (napr. 14–30 dní)
- weight decay pre krátkodobé výkyvy

### Cieľ
Zabrániť tomu, aby model reflektoval náhodu ako identitu.

---

## ADR-051 — Cold Start & Trust Bootstrapping

### Problém
Cold start je podcenený.

Používateľ:
- nevie odpovedať
- nemá dôveru
- nevidí hodnotu

### Čo doplniť

Definovať:
- prvých 5 minút používateľa
- čo dostane bez dát
- minimálny value output bez personalizácie

Mechanizmy:
- default baseline model
- progressive profiling
- low-friction otázky

### Cieľ
Používateľ musí cítiť hodnotu skôr, než začne investovať energiu.

---

## ADR-052 — Model Integrity & Anti-Gaming

### Problém
Používateľ môže manipulovať model:
- odpovedá účelovo
- testuje systém
- „hrá hru“

### Čo doplniť

Mechanizmy:
- divergence detection (declared vs behavior)
- trust scoring signálov
- weighting podľa konzistencie

Dôležité:
- nepenalizuje sa user
- penalizuje sa signál

### Cieľ
Model musí reprezentovať realitu, nie optimalizačné správanie.

---

## ADR-053 — Contextual Intelligence Model

### Problém
Context model je príliš slabý.

Momentálny stav:
- location
- time

Realita:
- kontext ovplyvňuje 50% rozhodovania

### Čo doplniť

Typy kontextu:
- session context
- intent (explore / solve / passive)
- energy proxy (čas, interakcia, tempo)

Mechanizmy:
- krátkodobý override modelu
- context-aware question selection

### Cieľ
Model reaguje na situáciu, nie len na historické dáta.

---

## ADR-054 — Personal AI UX Contract

### Problém
Snapshot existuje, ale UX nie je definované.

Používateľ nerozumie:
- čo systém vie
- prečo mu niečo ukazuje

### Čo doplniť

Definovať:
- „5 second understanding“
- top interests (max 3–5)
- vysvetlenie: „prečo toto vidíš“

Zakázané:
- raw scoring
- komplexné modelové výstupy

### Cieľ
Používateľ musí rozumieť systému bez štúdia modelu.

---

# 🟠 SHOULD — Stabilita a udržateľnosť

---

## ADR-055 — Model Simplicity Constraints

### Problém
Model je príliš komplexný príliš skoro.

### Čo doplniť

Pravidlá pre MVP:
- max 5–7 dimenzií
- propagation max 1 hop
- vypnúť learned relations

### Cieľ
Zabrániť over-engineeringu a low ROI vývoju.

---

## ADR-056 — Model Failure & Recovery

### Problém
Nie je definované, čo keď model zlyhá.

### Scenáre:
- model je nesprávny
- používateľ ho odmieta
- systém sa netrafí

### Čo doplniť

Mechanizmy:
- soft reset
- partial reset (napr. interest only)
- explain error („prečo si to myslíme“)

### Cieľ
Zachovať dôveru aj pri chybe.

---

# 🧭 Zhrnutie

## MUST (blokujúce realitu):
- ADR-050 — Identity Noise
- ADR-051 — Cold Start
- ADR-052 — Anti-Gaming
- ADR-053 — Context Model
- ADR-054 — UX Contract

## SHOULD (stabilita):
- ADR-055 — Simplicity Constraints
- ADR-056 — Failure & Recovery

---

# 🔥 Finálna veta

> Personal AI nie je problém modelovania.
> Je to problém reality človeka.