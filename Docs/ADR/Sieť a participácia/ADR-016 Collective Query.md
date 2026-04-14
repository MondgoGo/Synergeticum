# 🧾 ADR-016 — Collective Query System (Hardened v2)

---

## 1. 📌 Status

Proposed

---

## 2. 🎯 Kontext

Synergetikum umožňuje:

* klásť otázky
* zbierať odpovede
* transformovať kolektívne myslenie do argumentov a projektov

Na rozdiel od tradičných systémov:

* nechce byť fórum (vysoký friction)
* nechce byť central AI (strata reality)

---

### Kľúčová požiadavka

> **Systém musí zbierať autentické ľudské odpovede a zároveň minimalizovať náročnosť participácie.**

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Core princíp

---

> **Otázky sú distribuované ľuďom. Odpovede sú vytvárané ľuďmi. Personal AI modely pomáhajú, ale nenahrádzajú rozhodnutie.**

---

👉 systém je:

* human-first
* AI-assisted

---

---

## 🧩 3.2 Query ako objekt

---

Každá otázka (Query):

obsahuje:

* text otázky
* kontext (lokácia, doména, rozsah)
* úroveň (lokálna / koncepčná)
* typ (názor / rozhodnutie / participácia)

---

👉 Query je:

> vstup do kolektívneho myslenia systému

---

---

## 🌐 3.3 Distribúcia otázky

---

Otázka sa distribuuje:

* relevantným používateľom
* na základe:

  * kontextu
  * záujmov
  * lokality
  * predchádzajúcich interakcií

---

👉 princíp:

> **cielená distribúcia, nie broadcast**

---

---

## 🧠 3.4 Human response (hlavná vrstva)

---

Používateľ odpovedá:

* textovo
* výberom (signal)
* intentom (ochota zapojiť sa)

---

👉 toto je:

> **jediný zdroj pravdy pre učenie systému**

---

---

## 🤖 3.5 Úloha Personal AI modelu

---

Model:

✔️ môže:

* navrhnúť odpoveď
* predvyplniť odpoveď
* sumarizovať otázku
* vysvetliť kontext

---

❌ nesmie:

* odpovedať autonómne bez potvrdenia
* vytvárať odpovede bez vedomia používateľa

---

---

## ⚖️ 3.6 Human confirmation

---

> **Každá odpoveď musí byť explicitne alebo implicitne potvrdená používateľom.**

---

Formy:

* potvrdenie návrhu
* úprava návrhu
* manuálna odpoveď

---

---

## 🧠 3.7 Low-friction participation

---

Systém umožňuje:

* rýchlu odpoveď (1 klik)
* detailnú odpoveď (deep input)
* preskočenie bez penalizácie

---

👉 cieľ:

> maximalizovať participáciu bez tlaku

---

---

## 📡 3.8 Typy odpovedí

---

Používateľ môže poskytnúť:

### 🧠 Opinion

* názor / postoj

---

### 👍 Signal

* áno / nie / nezaujíma

---

### 🤝 Intent

* ochota zapojiť sa

---

### ❌ Skip

* ignorovanie

---

---

## 🧠 3.9 Learning layer (kľúčová vrstva)

---

Personal AI model sa učí z:

* odpovedí používateľa
* jeho reakcií
* jeho preferencií

---

👉 nikdy z:

* vlastných generovaných odpovedí

---

---

## 🧠 3.10 Kolektívna odpoveď

---

Systém agreguje:

* odpovede
* signály
* intenty

---

👉 výsledok:

> **mapa kolektívneho myslenia, nie jedna odpoveď**

---

---

## 🧠 3.11 Transformácia

---

Výstupy Query môžu viesť k:

* Topic
* Argument graph (ADR-025)
* Project (ADR-022)

---

---

## 🔒 3.12 Privacy princíp

---

* identita používateľa nie je exponovaná
* zdieľajú sa len odpovede
* model zostáva lokálny

---

👉 model funguje ako:

> **interpretačná vrstva, nie zdroj dát**

---

---

## 🧠 3.13 Vzťah k reciprocity (ADR-036)

---

* viac odpovedí → lepší model
* lepší model → lepšie výstupy

---

ALE:

> participácia ≠ moc

---

---

## 🧠 3.14 Vzťah k argumentácii (ADR-025)

---

* odpovede sa transformujú na argumenty
* vzniká argument graph

---

---

## 🧠 3.15 Vzťah k truth modelu (ADR-039)

---

* odpovede ≠ pravda
* sú vstupom do epistemického procesu

---

---

## 🧠 3.16 Vzťah k chaosu (ADR-040)

---

* systém toleruje nekvalitné odpovede
* degradácia je plynulá

---

---

## 🧠 3.17 Anti-feedback-loop ochrana

---

> **Model sa nesmie učiť zo svojich vlastných návrhov bez potvrdenia používateľa.**

---

👉 tým sa zabraňuje:

* driftu
* self-reinforcement

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitíva

---

### 🧠 Autenticita

* odpovede sú reálne

---

### 🔁 Učenie

* model sa učí z reality

---

### ⚡ Nízky friction

* AI pomáha, ale nenahrádza

---

---

## ❗ Negatíva / Trade-offs

---

### 🧠 Vyššia závislosť na používateľovi

* bez odpovedí → slabý model

---

### ⚙️ UX náročnosť

* treba dobre navrhnúť flow

---

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Model-to-model systém

* stráca realitu
* vytvára feedback loop

---

---

### ❌ Čisté fórum

* vysoký friction

---

---

### ❌ Central AI odpovede

* porušuje decentralizáciu

---

---

## 6. 🔗 Väzby na ďalšie ADR

---

* ADR-001 — Ownership
* ADR-002 — Human-in-the-loop
* ADR-025 — Argumentation
* ADR-030 — Sync
* ADR-036 — Reciprocity
* ADR-039 — Truth
* ADR-040 — Graceful Degradation

---

---

## 7. 📏 Pravidlá implementácie

---

Systém musí:

* distribuovať otázky selektívne
* umožniť rýchle odpovede
* umožniť AI asistenciu
* vyžadovať potvrdenie odpovede
* ukladať odpovede ako learning signal
* zabrániť self-learning loopu

---

---

# 🔥 8. Zhrnutie

---

👉
**Synergetikum zbiera odpovede od ľudí, nie od modelov – modely slúžia na to, aby ľuďom pomohli odpovedať a učili sa z ich rozhodnutí.**

---

---

# 🧭 Finálna veta

---

👉
**V Synergetiku AI nemyslí za človeka – pomáha človeku myslieť.**
