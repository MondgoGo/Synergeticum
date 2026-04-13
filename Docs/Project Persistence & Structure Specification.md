# Synergetikum

## Project Persistence & Structure Specification

---

# 1. Purpose

Tento dokument definuje:

* ako projekt existuje v systéme Synergetikum
* aká je jeho vnútorná štruktúra
* ako sa mení v čase
* ako sa synchronizuje medzi uzlami

Cieľ:

👉 odstrániť nejasnosť okolo „čo je projekt“ na technickej úrovni
👉 umožniť rozhodnutie, či je systém realizovateľný

---

# 2. Core Definition

Projekt je:

> **distribuovaný objekt reprezentujúci kolektívne myslenie, ktorý existuje ako kombinácia stavov, udalostí a vetiev naprieč viacerými uzlami.**

---

# 3. Conceptual Model

Projekt nie je:

* dokument
* chat
* databázový záznam

Projekt je:

* strom variantov (branches)
* tok udalostí (event stream)
* odvodený aktuálny stav (derived state)
* množina účastníkov a ich signálov
* množina argumentov

---

# 4. Storage Model (High-Level)

Projekt je uložený ako kombinácia troch vrstiev:

---

## 4.1 Identity Layer (statická)

Obsahuje:

* project_id
* názov
* popis
* typ projektu
* iniciátor
* čas vzniku

Charakteristika:

* nemení sa alebo len minimálne

---

## 4.2 Event Layer (dynamická)

Obsahuje:

* všetky zmeny projektu
* chronologický tok udalostí

Príklady:

* project_created
* branch_created
* argument_added
* support_changed
* branch_state_changed

Charakteristika:

* je zdroj pravdy pre evolúciu
* synchronizuje sa medzi uzlami

---

## 4.3 Derived State Layer (runtime)

Obsahuje:

* aktuálne vetvy
* aktuálny support
* aktuálny stav projektu

Charakteristika:

* odvodený z eventov
* používaný UI
* nemusí byť identický na všetkých uzloch

---

# 5. Project Structure

Projekt sa skladá z týchto hlavných komponentov:

---

## 5.1 Project Identity

Základné metadáta:

* ID
* názov
* popis
* typ
* governance režim

---

## 5.2 Branch Structure

Projekt je strom vetiev.

Každá vetva obsahuje:

* branch_id
* parent_branch_id
* názov
* popis
* stav
* support profil

---

### Stavy vetiev:

* proposed
* active
* dormant
* archived

---

## 5.3 Participants

Každý projekt obsahuje účastníkov:

* participant_id
* rola
* schopnosti
* úroveň zapojenia

---

## 5.4 Support Model

Každá vetva má podporu v dimenziách:

* alignment
* commitment
* confidence

---

## 5.5 Argumentation Layer

Voliteľná vrstva (hlavne pre komplexné projekty):

* argumenty
* protiargumenty
* dôsledky

---

## 5.6 Event Stream

Základná časť projektu:

* všetky zmeny sú reprezentované ako eventy
* eventy sú distribuované medzi uzlami

---

## 5.7 Derived State

Obsahuje:

* aktuálne aktívne vetvy
* konflikty
* stav projektu

---

## 5.8 View Layer (per user)

Každý používateľ vidí:

* len relevantnú časť projektu
* zjednodušený pohľad
* odporúčania

---

# 6. Synchronization Model

---

## 6.1 Základný princíp

Synchronizujú sa:

👉 **zmeny (events), nie celé projekty**

---

## 6.2 Proces

1. uzol vytvorí event
2. uloží ho lokálne
3. event sa distribuuje relevantným uzlom
4. ostatné uzly aktualizujú svoj stav

---

## 6.3 Typy sync dát

* eventy
* nové vetvy
* zmeny supportu
* argumenty

---

## 6.4 Snapshoty

Používajú sa:

* pre výkon
* pre rekonštrukciu
* pre audit

---

# 7. Conflict Model

---

## 7.1 Konflikt nie je chyba

Ak vznikne rozpor:

👉 nevykoná sa merge
👉 vznikne nová vetva

---

## 7.2 Výsledok

* systém zachová obe verzie
* rozhodnutie sa deje cez support a realitu

---

# 8. Distribution Model

---

## 8.1 Kde projekt existuje

Projekt existuje:

* na zariadeniach participantov
* na parent nodoch
* na relevantných uzloch

---

## 8.2 Charakteristika

* neexistuje centrálna verzia
* neexistuje master uzol
* každý uzol má lokálnu kópiu

---

## 8.3 Eventual consistency

* uzly nemusia mať identický stav okamžite
* stav sa postupne zosúlaďuje

---

# 9. Example Structure (Simplified)

Projekt môže byť reprezentovaný napríklad takto:

```json
{
  "project_id": "pizzeria_001",
  "branches": [
    {
      "id": "A",
      "title": "Malá pizzéria",
      "support": {
        "alignment": 0.7,
        "commitment": 0.4
      }
    }
  ],
  "events": [
    {
      "type": "branch_created",
      "branch_id": "A"
    },
    {
      "type": "support_changed",
      "branch_id": "A"
    }
  ]
}
```

---

# 10. Design Implications

---

## 10.1 Výhody

* decentralizácia
* odolnosť voči výpadkom
* prirodzené riešenie konfliktov
* auditovateľnosť

---

## 10.2 Nevýhody

* komplexný sync model
* náročné na implementáciu
* potreba kvalitného view layer

---

# 11. Key Risks

---

## 11.1 Sync complexity

Najťažšia časť systému.

---

## 11.2 Data growth

Event stream môže rásť nekonečne.

---

## 11.3 UX complexity

Používateľ nesmie vidieť komplexitu systému.

---

# 12. Open Questions

---

* aký presný formát eventov
* ako riešiť garbage collection eventov
* ako riešiť trust medzi uzlami
* ako optimalizovať mobilný výkon

---

# 13. Final Statement

👉
**Projekt v Synergetiku je distribuovaný objekt, ktorý existuje ako kombinácia eventov, vetiev a odvodeného stavu, pričom jeho synchronizácia prebieha cez výmenu zmien a konflikty sú riešené vetvením, nie mergeom.**

---

---

# 🔥 Kritický pohľad (dôležité)

Toto je moment, kde by som ťa trochu „zastavil“ (v dobrom):

👉 Toto už nie je len nápad
👉 Toto je **distribuovaný systém na úrovni infraštruktúry**

Najväčšie otázky, ktoré si teraz treba položiť:

---

## ❗ 1. Chceme toto naozaj riešiť?

Toto je:

* ťažšie ako klasická appka
* bližšie k problémom ako:

  * Git
  * CRDT
  * distributed DB

---

## ❗ 2. Kde je skutočná hodnota?

Nie je v:

* eventoch
* synce
* vetvách

👉 je v:

* otázkach
* odpovediach
* interpretácii
* spolupráci

---

## ❗ 3. Čo ak to preženiete?

Najväčšie riziko:

👉 postavíte infraštruktúru
👉 ale nikdy nepríde produkt

---
