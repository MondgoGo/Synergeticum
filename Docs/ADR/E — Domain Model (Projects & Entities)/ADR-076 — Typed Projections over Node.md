# 🧾 ADR-076: Typed Projections over Node

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

Synergetikum má univerzálny model entity – `Node` (ADR-042). Všetko je Node: `Project`, `Topic`, `Argument`, `Branch`, `Person`, `Question`, `Event`.

Toto rozhodnutie je architektonicky čisté – jeden storage model, jeden sync mechanizmus, jednotné vzťahy.

V reálnej implementácii však vzniká zásadný problém:

> **Aplikácia nepracuje s „Node“. Pracuje s Projectmi, Topicmi, Argumentmi.**

Ak vývojár dostane `Node` objekt, nevie:
- či má `content` pre Project alebo pre Topic
- či má `attributes.status` alebo `lifecycle.stage`
- či má `participants` alebo `responses`

Výsledok je:
- kód plný `switch (node.NodeType)` a `if (node.Content is ProjectContent)`
- doménové pravidlá rozliate po celej aplikácii
- typová bezpečnosť je len na papieri, nie v kóde
- každá zmena vyžaduje úpravy na 10 miestach

---

## 3. ⚖️ Rozhodnutie

Systém zavádza **Typed Projections** – vrstvu, ktorá mapuje univerzálny `Node` na **doménovo typované objekty** (Project, Topic, Argument, Branch, Person, Question, Event).

> **Storage a sync pracujú s Node. Aplikácia pracuje s Typed Projections. Node je serializačný formát. Project je doménový objekt.**

Princíp:

```
┌─────────────────────────────────────────────────────────────┐
│                      Aplikačná vrstva                        │
│  (Project, Topic, Argument, Branch, Person, Question, Event)│
└─────────────────────────────────────────────────────────────┘
                              ▲
                              │ map / project
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      Storage / Sync vrstva                   │
│                         (Node)                               │
└─────────────────────────────────────────────────────────────┘
```

---

## 4. 🧠 Definícia Typed Projection

Typed Projection je **doménovo typovaný objekt**, ktorý:
- je **read-only** (pre aplikáciu)
- vzniká **projekciou** z Node
- obsahuje len **doménové polia** (nie meta polia Node)
- má **metódy pre doménovú logiku**

Projection NIE JE:
- nový storage model (dáta sú stále v Node)
- duplicitné ukladanie
- ORM vrstva

---

## 5. 📋 Typed Projections pre každý Node typ

### 5.1 Project Projection

| Node pole | Projection property | Poznámka |
|-----------|---------------------|----------|
| `node_id` | `Id` | |
| `name` | `Name` | |
| `content.problem_statement` | `ProblemStatement` | |
| `content.goal_statement` | `GoalStatement` | voliteľné |
| `content.success_criteria` | `SuccessCriteria` | voliteľné |
| `content.lifecycle_stage` | `Stage` | |
| `content.origin_topic_id` | `OriginTopicId` | voliteľné |
| `content.initial_branch_id` | `InitialBranchId` | voliteľné |
| `rights.owner` | `OwnerId` | |
| `statistics.participants` | `ParticipantIds` | |
| `statistics.commitment_count` | `CommitmentCount` | |
| `statistics.last_activity` | `LastActivityAt` | |

**Doménové metódy (príklady):**
- `CanTransitionToActive()` – či projekt môže prejsť do Active
- `IsHealthy()` – či má dostatočnú podporu
- `GetDormancyDays()` – koľko dní je neaktívny

### 5.2 Topic Projection

| Node pole | Projection property |
|-----------|---------------------|
| `node_id` | `Id` |
| `name` | `Name` |
| `content.origin_question_id` | `OriginQuestionId` |
| `content.lifecycle_stage` | `Stage` (New, Emerging, Active, Candidate, Converted, Faded) |
| `content.project_reference` | `ProjectReferenceId` | voliteľné |
| `content.converted_at` | `ConvertedAt` | voliteľné |
| `statistics.interest_level` | `InterestLevel` |
| `statistics.participant_count` | `ParticipantCount` |
| `statistics.response_count` | `ResponseCount` |
| `signals.commitment_count` | `CommitmentCount` |
| `signals.directions` | `Directions` |

**Doménové metódy:**
- `CanTransitionToProject()` – či spĺňa podmienky na vznik projektu
- `IsFading()` – či interest klesá

### 5.3 Argument Projection

| Node pole | Projection property |
|-----------|---------------------|
| `node_id` | `Id` |
| `name` | `Title` |
| `content.stance` | `Stance` (Support, Oppose, Neutral) |
| `content.confidence` | `Confidence` |
| `content.evidence` | `Evidence` |
| `relations` | `SupportsBranchIds`, `OpposesBranchIds` |
| `relations` | `ProjectId` alebo `TopicId` (práve jeden) |

**Doménové metódy:**
- `IsAboutProject()` – či argument patrí projektu
- `IsAboutTopic()` – či argument patrí topicu
- `GetTargetId()` – vráti Id projektu alebo topicu

### 5.4 Branch Projection

| Node pole | Projection property |
|-----------|---------------------|
| `node_id` | `Id` |
| `name` | `Name` |
| `content.project_id` | `ProjectId` |
| `content.parent_branch_id` | `ParentBranchId` | voliteľné |
| `content.merge_status` | `MergeStatus` |
| `content.merged_into_branch_id` | `MergedIntoBranchId` | voliteľné |
| `statistics.support_score` | `SupportScore` |
| `statistics.alignment_count` | `AlignmentCount` |
| `statistics.commitment_count` | `CommitmentCount` |

**Doménové metódy:**
- `IsRoot()` – či nemá parent branch
- `IsMerged()` – či bol zlúčený
- `GetLineage()` – reťaz rodičov

### 5.5 Person Projection

| Node pole | Projection property |
|-----------|---------------------|
| `node_id` | `Id` |
| `name` | `DisplayName` |
| `content.public_key` | `PublicKey` |
| `content.profile` | `Profile` | voliteľné |

### 5.6 Question Projection

| Node pole | Projection property |
|-----------|---------------------|
| `node_id` | `Id` |
| `name` | `Text` |
| `content.perspectives` | `Perspectives` |
| `content.expected_answer_type` | `ExpectedAnswerType` |
| `content.context` | `Context` | voliteľné |

### 5.7 Event Projection

| Node pole | Projection property |
|-----------|---------------------|
| `node_id` | `Id` |
| `name` | `Name` |
| `content.start_time` | `StartTime` |
| `content.end_time` | `EndTime` |
| `content.location` | `Location` |
| `content.recurrence` | `Recurrence` | voliteľné |
| `content.related_node_id` | `RelatedNodeId` | (projekt, vetva, argument, téma) |

**Doménové metódy:**
- `IsUpcoming()` – či je v budúcnosti
- `IsOngoing()` – či práve prebieha
- `IsPast()` – či už skončil

---

## 6. 🔄 Projekčná vrstva (mapovanie)

Systém obsahuje **mapper**, ktorý konvertuje `Node` ↔ `TypedProjection`.

### 6.1 Smer Node → Projection (čítanie)

```text
Node (z databázy/syncing)
    ↓
GetNode(nodeId) → Node
    ↓
ProjectionFactory.CreateProjection(node)
    ↓
Project / Topic / Argument / Branch / Person / Question / Event
```

Pravidlá:
- Projection je **immutable** (read-only)
- Ak Node nemá povinné polia pre daný typ, projekcia zlyhá (`InvalidProjectionException`)
- Projekcia je **lacná** (žiadne ďalšie dotazy do DB, len preskupenie polí)

### 6.2 Smer Projection → Node (zápis)

```text
Project / Topic / Argument / ...
    ↓
Projection.ToNode()
    ↓
Node (s vyplnenými poliami)
    ↓
Repository.Save(node)
```

Pravidlá:
- Projekcia **negeneruje** nové ID (používa existujúce)
- Projekcia **nemení** polia, ktoré nie sú v jej doméne
- Zmeny sa ukladajú cez Node, nie priamo

---

## 7. 🚫 Čo sa NESMIE stať

| Zakázaná situácia | Prečo |
|-------------------|-------|
| Aplikácia pracuje priamo s `Node` | Stratí sa typová bezpečnosť a doménová jasnosť |
| Projection obsahuje polia `Node` | Porušuje separáciu vrstiev |
| Projection má metódy na ukladanie | Ukladanie patrí do repository, nie do projekcie |
| Projektovanie je pomalé | Musí byť O(1) – len preskupenie polí |
| Projekcie sú modifikovateľné | Len ToNode() vytvára nový Node na zápis |

---

## 8. 🧠 Výhody tohto prístupu

| Výhoda | Popis |
|--------|-------|
| **Typová bezpečnosť** | `Project` má `ProblemStatement`, nie `content["problem_statement"]` |
| **Doménová logika na jednom mieste** | Metódy ako `CanTransitionToActive()` sú v projekcii |
| **Storage nezávislý na doméne** | Node sa môže meniť bez zmeny aplikačného kódu |
| **Jednotný sync** | Sync engine pracuje s Node, nepotrebuje poznať doménu |
| **Testovateľnosť** | Projekcie sa dajú ľahko mockovať |
| **Čisté hranice** | Každá vrstva vie, čo robí |

---

## 9. 📍 Kde ktorá vrstva používa čo

| Vrstva | Používa |
|--------|---------|
| **UI (MAUI stránky)** | Typed Projections (Project, Topic, ...) |
| **View Model** | Typed Projections |
| **Doménová logika** | Typed Projections (metódy na nich) |
| **Participation Filter** | Typed Projections |
| **Personal AI** | Typed Projections |
| **Opportunity Engine** | Typed Projections |
| **Repository** | Node |
| **Sync engine** | Node |
| **Storage (SQLite)** | Node (ako JSON alebo tabuľky) |

---

## 10. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-042 | Node model – toto ADR je projekčná vrstva nad ním |
| ADR-077 | MVPj – Project projekcia implementuje MVPj pravidlá |
| ADR-078 | Topic→Project – Topic a Project projekcie |
| ADR-082 | Domain Validation – validácia beží na projekciách |
| ADR-027 | Project View – view vrstva pracuje s projekciami |
| ADR-015 | Participation Filter – filter pracuje s projekciami |

---

## 11. 🧪 Príklad použitia v aplikácii

```text
// UI vrstva nevidí Node, vidí Project
Project project = _projectRepository.Get(projectId);

// Doménová logika je na projekcii
if (project.CanTransitionToActive())
{
    project = project.WithStage(ProjectStage.Active);
    _projectRepository.Save(project.ToNode());
}

// View Model zobrazuje vlastnosti projekcie
Title = project.Name;
Description = project.ProblemStatement;
Status = project.Stage.ToString();
```

---

## 12. 🔥 Zhrnutie

> **Storage a sync poznajú Node. Aplikácia pozná Project, Topic, Argument. Typed Projections sú most medzi týmito svetmi.**

Tento ADR definuje:

- Čo je Typed Projection
- Projekcie pre každý typ Node (Project, Topic, Argument, Branch, Person, Question, Event)
- Ako prebieha mapovanie Node → Projection → Node
- Ktorá vrstva čo používa
- Čo sa nesmie stať

---

## 13. 🧭 Finálna veta

> **Node je výborný pre storage a sync. Ale pre aplikáciu je to cudzí jazyk. Typed Projections prekladajú tento jazyk do reči, ktorej rozumie každý vývojár – reči domény.**

---

**Dátum:** 2026-04-16  
**Stav:** Proposed  
**Typ:** Architecture ADR (Typed Projections)  
**Verzia:** 1.0