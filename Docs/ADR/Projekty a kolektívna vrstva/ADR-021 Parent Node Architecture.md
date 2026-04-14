# ADR-021 — Parent Node Architecture

## Status

Proposed

---

## Kontext

Synergetikum je postavené na princípe:

* Personal AI je primárne lokálna (mobil / zariadenie používateľa)
* decentralizácia (P2P)
* súkromie (dáta neopúšťajú zariadenie)

Avšak vzniká praktický problém:

> Mobilné zariadenia majú obmedzený výkon, batériu a čas online pripojenia.

Niektoré operácie sú náročné:

* tréning modelu
* simulácie (ADR-018)
* agregácie dát
* dlhodobé learning cykly

---

## Rozhodnutie

> Synergetikum zavádza koncept **Parent Node** — sekundárne zariadenie používateľa (desktop / server / always-on device), ktoré rozširuje schopnosti Personal AI.

Parent Node:

* patrí používateľovi
* je dôveryhodný
* funguje ako rozšírenie, nie náhrada

---

## Core princíp

> Mobil = interakcia
> Parent Node = výpočtová hĺbka

---

## Úlohy Parent Node

### 1. Heavy computation

* tréning modelu
* batch learning
* simulácie scenárov

---

### 2. Data aggregation

* spracovanie historických dát
* dlhodobé vzory

---

### 3. Model refinement

* optimalizácia váh
* aktualizácia profilov

---

### 4. Sync coordination

* stabilnejší P2P sync
* cache a buffering

---

## Architektúra

### Mobile Node

* UI
* otázky
* odpovede
* lightweight inference

---

### Parent Node

* learning engine
* simulation engine
* storage

---

## Komunikácia

* secure channel (end-to-end encryption)
* sync interval alebo event-based

---

## Trust model

* Parent Node je 100% pod kontrolou používateľa
* nie je centrálna entita
* môže byť offline

---

## Deployment možnosti

* desktop app
* home server (Raspberry Pi, NAS)
* cloud VM (voliteľné, ale user-owned)

---

## Fallback

Ak Parent Node neexistuje:

* mobil funguje samostatne
* obmedzený výkon

---

## Riziká

### 1. Setup complexity

➡️ user nemusí vedieť nastaviť

### 2. Fragmentácia

➡️ rôzne konfigurácie

### 3. Security

➡️ potreba silného šifrovania

---

## Alternatívy

### Central server

❌ porušenie princípov

### Mobile-only

❌ limitovaný výkon

---

## Väzby na ADR

* ADR-000 — Core vision
* ADR-018 — Simulation Engine
* ADR-006 — Learning Model

---

## Zhrnutie

Parent Node umožňuje škálovať Personal AI bez straty decentralizácie.

---

## Finálna veta

> Synergetikum nezvyšuje výkon centralizáciou — ale rozšírením používateľa o vlastnú infraštruktúru.
