# 🧾 ADR-076: Typed Projections over Node

## 1. 📌 Status

**Accepted** (revízia 2.0)

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

## 4. 🧠 Read model vs Write model (kľúčové oddelenie)

### 4.1 Kritický princíp

> **Typed Projection je read model. Write model nie je projekcia.**

| Vrstva | Účel | Príklad |
|--------|------|---------|
| **Read model (Typed Projection)** | Čítanie doménových dát | `Project` (immutable) |
| **Write model (Command)** | Zámer zmeny | `UpdateProjectStageCommand`, `AddParticipantCommand` |
| **Storage model (Node)** | Perzistencia | `Node` (JSON alebo tabuľka) |

### 4.2 Prečo je to dôležité

`Projection.ToNode()` je **nebezpečná skratka**, ktorá vedie k:

| Problém | Dôsledok |
|---------|----------|
| **Hacky v ToNode()** | Doménová logika sa nasťahuje do mapovacej vrstvy |
| **Nejasné hranice** | Kde končí mapovanie a začína doménová logika? |
| **Porušenie čistoty** | ToNode() začne robiť rozhodnutia, nie len mapovať |

### 4.3 Správny tok zápisu

```
Command → Handler → načíta Node → validácia → zmena Node → uloženie Node
```

Nie:

```
Projection.ToNode()  // toto je skratka, ktorá sa raz pomstí
```

### 4.4 Čo je povolené

Typed Projection **môže** obsahovať doménové metódy na **čítanie**:

- `CanTransitionToActive()` – overenie podmienok (nemení stav)
- `IsHealthy()` – vypočítaná hodnota
- `GetDormancyDays()` – odvodená hodnota

Typed Projection **nesmie** obsahovať metódy, ktoré **menia stav** alebo **ukladajú dáta**.

---

## 5. 🧠 Definícia Typed Projection

Typed Projection je **doménovo typovaný read-only objekt**, ktorý:
- je **immutable** (pre aplikáciu)
- vzniká **projekciou** z Node
- obsahuje len **doménové polia** (nie meta polia Node)
- **môže** obsahovať read-only doménové metódy (CanTransition, IsHealthy apod.)

Projection NIE JE:
- nový storage model (dáta sú stále v Node)
- duplicitné ukladanie
- write model
- ORM vrstva

---

## 6. 📋 Typed Projections pre každý Node typ

### 6.1 Project Projection

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

**Read-only doménové metódy (príklady):**
- `CanTransitionToActive()` – overí, či projekt môže prejsť do Active (nemení stav)
- `IsHealthy()` – či má dostatočnú podporu
- `GetDormancyDays()` – koľko dní je neaktívny

### 6.2 Topic Projection

| Node pole | Projection property |
|-----------|---------------------|
| `node_id` | `Id` |
| `name` | `Name` |
| `content.origin_question_id` | `OriginQuestionId` |
| `content.lifecycle_stage` | `Stage` |
| `content.project_reference` | `ProjectReferenceId` | voliteľné |
| `content.converted_at` | `ConvertedAt` | voliteľné |
| `statistics.interest_level` | `InterestLevel` |
| `statistics.participant_count` | `ParticipantCount` |
| `statistics.response_count` | `ResponseCount` |
| `signals.commitment_count` | `CommitmentCount` |
| `signals.directions` | `Directions` |

### 6.3 Argument Projection

| Node pole | Projection property |
|-----------|---------------------|
| `node_id` | `Id` |
| `name` | `Title` |
| `content.stance` | `Stance` |
| `content.confidence` | `Confidence` |
| `content.evidence` | `Evidence` |
| `relations` | `SupportsBranchIds`, `OpposesBranchIds` |
| `relations` | `ProjectId` alebo `TopicId` (práve jeden) |

### 6.4 Branch Projection

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

### 6.5 Person Projection

| Node pole | Projection property |
|-----------|---------------------|
| `node_id` | `Id` |
| `name` | `DisplayName` |
| `content.public_key` | `PublicKey` |
| `content.profile` | `Profile` | voliteľné |

### 6.6 Question Projection

| Node pole | Projection property |
|-----------|---------------------|
| `node_id` | `Id` |
| `name` | `Text` |
| `content.perspectives` | `Perspectives` |
| `content.expected_answer_type` | `ExpectedAnswerType` |
| `content.context` | `Context` | voliteľné |

### 6.7 Event Projection

| Node pole | Projection property |
|-----------|---------------------|
| `node_id` | `Id` |
| `name` | `Name` |
| `content.start_time` | `StartTime` |
| `content.end_time` | `EndTime` |
| `content.location` | `Location` |
| `content.recurrence` | `Recurrence` | voliteľné |
| `content.related_node_id` | `RelatedNodeId` | (projekt, vetva, argument, téma) |

---

## 7. 📋 Projekčné profily (Lightweight Projections)

Pre efektívnosť sa zavádzajú **projekčné profily**. Každý Node type má maximálne 3 oficiálne profily:

| Profil | Obsah | Kde sa používa |
|--------|-------|----------------|
| **Summary** | `Id`, `Name`, `Stage`, `ParticipantCount`, `LastActivityAt` | Dashboard, zoznamy, notifikácie |
| **Detail** | Všetky polia | Detailná obrazovka |
| **Domain/Internal** | Všetky polia + read-only doménové metódy | Doménová logika, filtre |

> Pravidlo: Projekcia by mala byť taká malá, ako to daný use-case vyžaduje. Nie je dôvod načítavať `SuccessCriteria`, keď ho nepoužívate.

---

## 8. 🔄 Projekčná vrstva (mapovanie)

### 8.1 Registry + špecializované mappery (nie jeden factory)

Namiesto jedného `ProjectionFactory` sa používa **registry + per-type mappery**:

```
ProjectionMapperRegistry
    ├── ProjectMapper     (Node → Project)
    ├── TopicMapper       (Node → Topic)
    ├── ArgumentMapper    (Node → Argument)
    ├── BranchMapper      (Node → Branch)
    ├── PersonMapper      (Node → Person)
    ├── QuestionMapper    (Node → Question)
    └── EventMapper       (Node → Event)
```

**Výhody:**

| Výhoda | Popis |
|--------|-------|
| **Každý mapper je malý** | Testuje sa izolovane |
| **Pridanie nového typu** | Stačí pridať nový mapper a zaregistrovať ho |
| **Dependency injection friendly** | Každý mapper môže mať vlastné závislosti |

### 8.2 Version-aware mapping

Node schema sa bude meniť. Mapper musí byť **version-aware**:

| Princíp | Popis |
|---------|-------|
| **Version-aware mapper** | Mapper pozná verziu Node a vie mapovať staršie verzie |
| **Fallback mapping** | Pre staršie verzie Node, ktoré nemajú všetky polia |
| **Invalid projection ≠ crash** | Ak nie je možné vytvoriť plnú projekciu, systém vráti error (nie crash) |

### 8.3 Partial projection availability

Nie všetky projekcie musia byť dostupné pre každý Node v každom stave:

| Situácia | Správanie |
|----------|-----------|
| Node je validný pre storage, ale nemá všetky polia pre Detail | `Summary` projekcia funguje, `Detail` vráti chybu |
| Node je v `draft` a nemá `success_criteria` | `Summary` funguje, `Detail` vráti čiastočný výsledok s warningom |
| Node je nevalidný (porušuje doménové pravidlá) | Žiadna projekcia nie je dostupná |

---

## 9. 🔄 Command / Mutation layer (write model)

Zápis do systému prebieha výhradne cez **Commandy**, nie cez projekcie.

### 9.1 Príklady Commandov

| Command | Vstup | Výstup |
|---------|-------|--------|
| `CreateProjectCommand` | `name`, `problemStatement`, `ownerId` | `projectId` |
| `UpdateProjectStageCommand` | `projectId`, `newStage` | `success` |
| `AddParticipantCommand` | `projectId`, `userId` | `success` |
| `RecordCommitmentCommand` | `projectId`, `userId`, `type` | `success` |

### 9.2 Command Handler

```text
Command → Handler:
    1. Načíta Node z repository
    2. Validuje (doménová validácia)
    3. Vykoná zmenu na Node
    4. Uloží Node
    5. Vráti výsledok
```

### 9.3 Prečo nie Projection.ToNode()

| Problém s `ToNode()` | Riešenie cez Commandy |
|----------------------|----------------------|
| Doménová logika v mapovaní | Doménová logika je v Handleri |
| Nejasné, čo sa deje | Každá zmena je explicitný Command |
| Ťažko auditovateľné | Commandy sa logujú |
| Žiadna transakčná hranica | Handler je transakčná hranica |

---

## 10. 🚫 Čo sa NESMIE stať

| Zakázaná situácia | Prečo |
|-------------------|-------|
| Aplikácia pracuje priamo s `Node` | Stratí sa typová bezpečnosť a doménová jasnosť |
| `Projection.ToNode()` na zápis | Write logika sa nasťahuje do projekcie |
| Jeden obrovský `ProjectionFactory` | Ťažko testovateľné, porušuje Open-Closed |
| Projekcia mení stav | Projekcia je read-only |
| Projekcia ukladá dáta | Ukladanie patrí do Command handlerov |
| Nevalidný Node → crash aplikácie | Projekcia musí zlyhať elegantne |

---

## 11. 🧠 Terminologická poznámka

ADR-076 používa termín "Typed Projection". V DDD terminológii by niektoré časti (doménové metódy) mohli byť považované za **agregát**.

| Termín | Význam |
|--------|--------|
| **Projection** | Len dáta, žiadna logika (čítanie) |
| **Typed Projection (v tomto ADR)** | Dáta + read-only doménové metódy (CanTransition, IsHealthy) |
| **Aggregate** | Dáta + command logika (zmena stavu) |

**Rozhodnutie pre Synergetikum:**

- Typed Projection **môže** obsahovať read-only doménové metódy
- Typed Projection **nesmie** obsahovať write logiku
- Write logika patrí výhradne do **Commandov**

Toto nie je plný DDD agregát – je to pragmatická hybridná vrstva.

---

## 12. 📍 Kde ktorá vrstva používa čo

| Vrstva | Používa |
|--------|---------|
| **UI (MAUI stránky)** | Typed Projections (Summary, Detail) |
| **View Model** | Typed Projections |
| **Read-only doménová logika** | Typed Projections (metódy ako `CanTransitionToActive`) |
| **Participation Filter** | Typed Projections (Summary) |
| **Personal AI** | Typed Projections |
| **Opportunity Engine** | Typed Projections (Summary) |
| **Write logika (zmeny)** | Commandy |
| **Repository** | Node |
| **Sync engine** | Node |
| **Storage (SQLite)** | Node (ako JSON alebo tabuľky) |

---

## 13. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-042 | Node model – toto ADR je projekčná vrstva nad ním |
| ADR-077 | MVPj – Project projekcia implementuje MVPj pravidlá (read-only) |
| ADR-078 | Topic→Project – Topic a Project projekcie |
| ADR-082 | Domain Validation – validácia beží na Node pred projekciou |
| ADR-027 | Project View – view vrstva pracuje s projekciami |
| ADR-015 | Participation Filter – filter pracuje s projekciami (Summary) |
| ADR-075 | Domain State vs Process Boundary – projekcie sú Domain State, Commandy sú Process |

---

## 14. 🧪 Príklad použitia v aplikácii

### 14.1 Čítanie (Read)

```text
// UI vrstva pracuje s projekciou
ProjectSummary project = _projectRepository.GetSummary(projectId);

// Zobrazenie v zozname
Title = project.Name;
Status = project.Stage.ToString();
LastActivity = project.LastActivityAt;

// Read-only doménová metóda
if (project.CanTransitionToActive())
{
    ShowActivateButton();
}
```

### 14.2 Zápis (Write)

```text
// Write ide cez command, nie cez projekciu
var command = new ActivateProjectCommand(projectId);
var result = _commandBus.Execute(command);

// Handler:
// 1. Načíta Node
// 2. Overí podmienky (vrátane CanTransitionToActive)
// 3. Zmení Stage na Active
// 4. Uloží Node
```

---

## 15. 🔥 Zhrnutie

> **Storage a sync poznajú Node. Aplikácia pozná Project, Topic, Argument. Typed Projections sú read-only most medzi týmito svetmi. Write ide cez Commandy, nie cez projekcie.**

Tento ADR definuje:

- **Read model (Typed Projections)** – immutable, read-only
- **Write model (Commandy)** – explicitné zmeny
- **Registry + per-type mappery** – nie jeden factory
- **Version-aware mapping** – pre staršie Node verzie
- **Projekčné profily** – Summary, Detail, Domain/Internal
- **Partial projection availability** – nie všetky projekcie musia byť dostupné
- **Terminologické vymedzenie** – nie je plný DDD agregát
- **Ktorá vrstva čo používa**
- **Čo sa nesmie stať**

---

## 16. 🧭 Finálna veta

> **Node je výborný pre storage a sync. Ale pre aplikáciu je to cudzí jazyk. Typed Projections prekladajú tento jazyk do reči, ktorej rozumie každý vývojár – reči domény. Ale pamätaj: projekcie sú na čítanie. Na zápis máš Commandy. ToNode() nie je write model. Ak to zmiešaš, zaplatíš to dvakrát – raz v kóde, raz v údržbe.**

---

**Dátum:** 2026-04-16  
**Stav:** Accepted  
**Typ:** Architecture ADR (Typed Projections)  
**Verzia:** 2.0 (revízia podľa pripomienok)