Tu je **finálna verzia ADR-076**, upravená tak, aby bola **jasná pre architekta, implementovateľná, disciplinovaná a pripravená na reálny systém (nie len koncept)**.

Zachováva tvoju víziu, ale uzatvára všetky miesta, kde by sa to v praxi rozpadlo.

---

# 🧾 ADR-076 — Typed Projections over Node (Final v3.0)

## 1. 📌 Status

**Accepted (Final for architecture handoff)**

---

## 2. 🎯 Kontext

Synergetikum používa jednotný entity model `Node` (ADR-042):

* všetky entity sú Node (`Project`, `Topic`, `Argument`, `Branch`, `Person`, `Question`, `Event`)
* storage, sync a infra pracujú výhradne s Node

Tento model je architektonicky správny, ale:

> **Aplikácia nepracuje s Node. Pracuje s doménovými entitami.**

Bez projekčnej vrstvy vzniká:

* `switch(node.NodeType)` chaos
* slabá typová bezpečnosť
* doménová logika rozliata po aplikácii
* vysoké riziko regresií

---

## 3. ⚖️ Rozhodnutie

Systém zavádza **Typed Projections**:

> **Node = storage formát
> Projection = doménový read model**

```
Node (storage/sync)
        ↓
Typed Projection (doména)
        ↓
UI / AI / Application
```

---

## 4. 🧠 Architektonický model

### 4.1 Oddelenie vrstiev

| Vrstva           | Účel                |
| ---------------- | ------------------- |
| Node             | storage + sync      |
| Typed Projection | read model (doména) |
| Command layer    | write model         |
| Policy layer     | rozhodovanie        |

---

### 4.2 Kritický princíp

> **Typed Projection je read-only. Nikdy nevykonáva zápis.**

---

### 4.3 Write flow (jediný povolený)

```
Command → Handler → Load Node → Validate → Modify Node → Save Node
```

---

### 4.4 Zakázané

```
Projection.ToNode()   ❌
Projection.Save()     ❌
Projection mutates    ❌
```

---

## 5. 🧠 Boundary: Structural vs Policy logic

Typed Projection môže:

* overovať štrukturálne podmienky:

  * počet participantov
  * existencia fieldov
  * referenčná integrita
* počítať lokálne odvodené hodnoty:

  * days since last activity
  * participant count
  * basic readiness

Typed Projection NESMIE:

* aplikovať policy thresholdy (`interest > 0.6`)
* používať globálny kontext (veľkosť siete)
* robiť finálne rozhodnutia

> **Finálne rozhodnutia robí Command Handler / Policy layer**

---

## 6. 📋 Definícia Typed Projection

Typed Projection:

* immutable (pre aplikáciu)
* vytvorený z Node
* obsahuje len doménové dáta
* môže mať read-only metódy

Nie je:

* storage model
* ORM
* write model

---

## 7. 📦 Projection typy (strict naming)

Každý Node typ má maximálne 3 projekcie:

| Typ             | Použitie           |
| --------------- | ------------------ |
| `XxxSummary`    | zoznamy, dashboard |
| `XxxDetail`     | detail             |
| `XxxDomainView` | doménová logika    |

### Pravidlo

> Bez silného dôvodu NESMIE vzniknúť nový projection typ

---

## 8. 🔄 Projekčná vrstva

### 8.1 Mapper architektúra

```
ProjectionMapperRegistry
    ├── ProjectMapper
    ├── TopicMapper
    ├── ArgumentMapper
    ├── BranchMapper
    ├── PersonMapper
    ├── QuestionMapper
    └── EventMapper
```

---

### 8.2 Version-aware mapping

Mapper:

* podporuje viac verzií Node
* fallbackuje chýbajúce polia
* nikdy nemení Node

---

### 8.3 Mapping vs Migration

| Layer               | Zodpovednosť      |
| ------------------- | ----------------- |
| Mapper              | čítanie           |
| Migration (ADR-083) | transformácia dát |

> Mapper NESMIE robiť migráciu

---

## 9. ⚠️ Projection Result Contract

Každá projekcia vracia:

```text
ProjectionResult<T>
```

| Stav        | Význam                |
| ----------- | --------------------- |
| Success     | plná projekcia        |
| Partial     | chýbajú niektoré dáta |
| Unavailable | nedá sa vytvoriť      |

### Pravidlá

* Summary → musí byť dostupná čo najčastejšie
* Detail → môže byť Partial
* DomainView → môže byť Unavailable

---

## 10. 🧠 Consistency & Freshness model

> Projection NIE JE globálna pravda

Projection reprezentuje:

* lokálny snapshot
* momentálny stav

Môže obsahovať:

* `Version`
* `LastSyncedAt`
* `CompletenessScore` (optional)

---

## 11. ⚡ Partial projection availability

| Stav Node    | Správanie              |
| ------------ | ---------------------- |
| incomplete   | Summary OK             |
| missing data | Detail partial         |
| invalid      | projection unavailable |

---

## 12. 📊 Batch & performance model

Projection layer musí podporovať:

* batch mapping (Node[])
* lazy loading
* partial hydration

### Pravidlá

| Typ        | Náročnosť |
| ---------- | --------- |
| Summary    | lacná     |
| Detail     | stredná   |
| DomainView | drahá     |

---

## 13. 🔄 Command layer (write model)

### Príklady

* `CreateProjectCommand`
* `ActivateProjectCommand`
* `AddParticipantCommand`
* `RecordCommitmentCommand`

### Handler:

1. Load Node
2. Validate
3. Apply change
4. Save

---

## 14. 🔗 Zladenie s ADR-042 (Node)

Typed Projection interpretuje:

* payload → doména
* relations → väzby
* lifecycle → stav
* access → práva

> Node je bez významu bez projekcie

---

## 15. 🧠 UX contract (kritické)

UI musí vedieť:

| Stav        | UI reakcia          |
| ----------- | ------------------- |
| Success     | normálne zobrazenie |
| Partial     | degraded UI         |
| Unavailable | fallback / retry    |

---

## 16. 🚫 Čo sa NESMIE stať

* Node v UI
* Projection zapisuje
* Mapper robí business logiku
* neexistujú projection profily
* neexistuje failure contract
* N+1 mapping chaos

---

## 17. 🔥 Zhrnutie

> **Node je infra jazyk. Projection je doménový jazyk.
> Command je jazyk zmien.
> Policy je jazyk rozhodnutí.**

Ak sa tieto vrstvy zmiešajú:

→ systém sa rozpadne

Ak ostanú oddelené:

→ systém škáluje

---

## 18. 🧭 Finálna veta

> **Typed Projections nie sú len mapping vrstva. Sú ochranný filter medzi chaotickým distribuovaným svetom Node a stabilnou doménovou realitou aplikácie. Ak ich udržíš striktne read-only a oddelíš od write a policy logiky, získaš systém, ktorý je zároveň flexibilný aj stabilný.**

---

# 🔚 Výsledok

Toto je:

* ✅ pripravené pre architekta
* ✅ pripravené pre implementáciu
* ✅ odolné voči rastu systému
* ✅ kompatibilné s P2P realitou


