# ADR-017 — Collaboration Model

## Status

Proposed

---

## Kontext

Synergetikum už definuje:

* vznik tém a projektov (ADR-012)
* participáciu a commitment (ADR-013)
* personalizáciu (ADR-011)

Kľúčová otázka, ktorú tento ADR rieši:

> Ako sa zo siete individuálnych odpovedí stane reálna spolupráca medzi ľuďmi?

Inými slovami:
👉 kto sa má zapojiť
👉 do čoho sa má zapojiť
👉 akým spôsobom sa má zapojiť

Bez tohto modelu:

* projekty existujú, ale nikto ich nerobí
* ľudia majú záujem, ale nevedia čo robiť
* vzniká chaos alebo pasivita

---

## Rozhodnutie

> Spolupráca v Synergetiku vzniká automatickým párovaním ľudí, projektov a rolí na základe ich schopností, záujmov a miery commitmentu.

Systém nečaká, že ľudia sa "nejako nájdu".

👉 systém aktívne vytvára spojenia

---

## Core princíp

> Správny človek + správna úloha + správny moment = spolupráca

---

## 3 základné komponenty

### 1. Človek (User)

Reprezentovaný cez Personal AI:

* Interest Profile
* Capability Profile
* Participation Profile

---

### 2. Projekt (Project)

Obsahuje:

* cieľ
* potreby (čo treba spraviť)
* aktuálny stav
* vetvy

---

### 3. Rola (Role / Need)

Najdôležitejší koncept pre spoluprácu.

Rola reprezentuje:

* konkrétnu potrebu projektu

Príklady:

* "potrebujeme niekoho na návrh interiéru"
* "potrebujeme investora"
* "potrebujeme človeka na komunikáciu"

---

## Collaboration flow

### 1. Identifikácia potreby

Projekt obsahuje alebo generuje potreby (roles).

---

### 2. Matching

Systém hľadá:

Match =

* capability fit
* interest alignment
* participation readiness

---

### 3. Návrh zapojenia

Používateľ dostane návrh:

* "Toto by si vedel spraviť"
* "Chceš sa zapojiť?"

---

### 4. Commitment

Používateľ:

* prijme
* odmietne
* ignoruje

---

### 5. Aktivita

Ak prijme:

* stáva sa contributor / executor

---

## Typy spolupráce

### 1. Micro-contribution

* malá úloha
* nízky commitment

---

### 2. Ongoing contribution

* opakovaná aktivita

---

### 3. Ownership

* človek preberá časť projektu

---

## Matching princípy

### 1. Relevance first

* musí dávať zmysel pre používateľa

---

### 2. Low friction

* jednoduché zapojenie

---

### 3. Progressive commitment

* najprv malé kroky
* potom väčšie

---

## Anti-patterns

❌ manuálne hľadanie ľudí
❌ spamovanie všetkých
❌ "kto chce pomôcť?" bez kontextu

---

## UX princípy

Používateľ musí vidieť:

* "Toto je pre teba relevantné"
* "Toto vieš spraviť"
* "Toto projekt potrebuje"

---

## Architektonické implikácie

### Model

* ProjectNeeds
* Role
* UserCapability

---

### Services

* MatchingService
* CollaborationService

---

### Flow

* detection → matching → suggestion → commitment → execution

---

## Riziká

### 1. Overmatching

➡️ používateľ zahltený

### 2. Undermatching

➡️ projekty stagnujú

---

## Alternatívy

### Manuálne tímy

❌ neškáluje

### Social network approach

❌ slabý signal

---

## Väzby na ADR

* ADR-012 — Topics & Projects
* ADR-013 — Participation
* ADR-011 — Personalization

---

## Zhrnutie

Synergetikum nenecháva spoluprácu na náhodu.

👉 aktívne spája:

* ľudí
* projekty
* úlohy

---

## Finálna veta

> Spolupráca v Synergetiku nevzniká tým, že sa ľudia nájdu — ale tým, že systém ich správne prepojí.
