# 🧾 ADR-040 — Graceful Degradation (princíp tolerancie voči chaosu)

---

## 1. 📌 Status

Proposed

---

## 2. 🎯 Kontext

Synergetikum je systém založený na:

* dobrovoľnej participácii
* decentralizovanom prostredí
* absencii centrálnej kontroly

To znamená, že musí fungovať v reálnych podmienkach, kde používatelia:

* sú unavení alebo rozptýlení
* odpovedajú nekonzistentne alebo náhodne
* ignorujú otázky
* poskytujú nízku kvalitu vstupov
* môžu byť manipulátori alebo trollovia

---

### Problém

> **Ako zabezpečiť, aby systém zostal funkčný a užitočný aj pri nízkej kvalite vstupov a chaotickom správaní používateľov?**

---

Bez riešenia:

* systém sa rozpadne pri prvom väčšom šume
* kvalita výstupov prudko klesne
* používatelia stratia dôveru

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Core princíp

---

> **Systém nesmie zlyhať náhle. Musí degradovať plynulo.**

---

To znamená:

* zhoršenie vstupov → postupné zhoršenie výstupov
* nie: zlyhanie alebo nefunkčnosť

---

---

## 🧩 3.2 Low-quality input tolerance

---

Systém:

* akceptuje neúplné odpovede
* akceptuje nekonzistentné odpovede
* akceptuje nízku participáciu

---

> **Kvalita vstupu nie je podmienkou fungovania systému.**

---

---

## 🧠 3.3 Partial knowledge operation

---

Systém funguje aj:

* s nekompletnými dátami
* s nejasnými preferenciami
* bez plného modelu používateľa

---

👉 namiesto zastavenia:

* pracuje s tým, čo má

---

---

## 🧠 3.4 Noise resilience

---

Systém je navrhnutý tak, aby:

* odolával náhodným vstupom
* odolával trollovaniu
* nebol ľahko manipulovateľný

---

> Šum znižuje kvalitu, ale nerozbíja systém.

---

---

## 🧠 3.5 Gradual degradation

---

Klesajúca kvalita vstupov vedie k:

| Stav                   | Výstup systému        |
| ---------------------- | --------------------- |
| vysoká kvalita vstupov | presné odporúčania    |
| stredná kvalita        | všeobecné odporúčania |
| nízka kvalita          | základné odpovede     |
| minimálna participácia | fallback režim        |

---

👉 nikdy:

* úplná strata funkcionality

---

---

## 🧠 3.6 Fallback režimy

---

Systém musí mať:

* základný režim bez personalizácie
* všeobecné odporúčania
* minimálnu funkčnosť

---

> **Používateľ vždy dostane nejakú hodnotu.**

---

---

## 🧠 3.7 Nezávislosť od ideálneho používateľa

---

Systém:

❌ nepredpokladá disciplinovaného používateľa
❌ nepredpokladá kvalitné odpovede
❌ nepredpokladá dlhodobú pozornosť

---

👉 je navrhnutý pre:

> **reálneho človeka v reálnom svete**

---

---

## 🧠 3.8 Neviditeľná adaptácia

---

Systém:

* sa prispôsobuje kvalite vstupov
* nevyžaduje explicitné nastavenie

---

👉 používateľ:

* nemusí riešiť „režim systému“

---

---

## 🧠 3.9 Prepojenie na Low-Friction Participation (ADR-000)

---

Graceful degradation je priamo prepojené s:

> **low-friction participation**

---

👉 pretože:

* nízka námaha → viac šumu
* systém musí tento šum tolerovať

---

---

## 🧠 3.10 Prepojenie na Argumentation (ADR-025)

---

* slabé argumenty neblokujú systém
* ale majú nižší dopad

---

👉 systém:

* nezakazuje vstupy
* ale prirodzene filtruje ich vplyv

---

---

## 🧠 3.11 Prepojenie na Power (ADR-038)

---

Graceful degradation:

* zabraňuje tomu, aby šum vytváral moc
* zabraňuje dominancii náhodných vstupov

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitíva

---

### 🔄 Robustnosť systému

* funguje aj pri chaose

---

### 👥 Prístupnosť

* nízka bariéra vstupu

---

### 🧠 Adaptivita

* systém sa prispôsobuje realite

---

---

## ❗ Negatíva / Trade-offs

---

### 📉 Nižšia presnosť

* pri slabých dátach

---

### ⚙️ Vyššia komplexita návrhu

* potreba adaptívnych mechanizmov

---

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Strict validation

* odmietanie nekvalitných vstupov

👉 vedie k nízkej participácii

---

---

### ❌ Hard failure model

* systém prestane fungovať pri nízkej kvalite

👉 neprijateľné

---

---

### ❌ Moderation-heavy model

* manuálne filtrovanie

👉 porušuje decentralizáciu

---

---

## 6. 🔗 Väzby na ďalšie ADR

---

* ADR-000 — Core principles (Low-friction participation)
* ADR-025 — Argumentation & Reasoning
* ADR-036 — Reciprocity
* ADR-037 — Identity
* ADR-038 — Power & Influence

---

---

## 7. 📏 Pravidlá implementácie (high-level)

---

Systém musí:

* fungovať s neúplnými dátami
* nezlyhať pri šume
* postupne znižovať kvalitu výstupov
* mať fallback režimy
* nevyžadovať ideálne správanie

---

---

# 🔥 8. Zhrnutie

---

👉
**Synergetikum je navrhnuté tak, aby fungovalo aj v chaose – nie len v ideálnych podmienkach.**

---

---

# 🧭 Finálna veta

---

👉
**Systém sa nehodnotí podľa toho, ako funguje pri dokonalých vstupoch, ale podľa toho, ako prežije zlé.**
