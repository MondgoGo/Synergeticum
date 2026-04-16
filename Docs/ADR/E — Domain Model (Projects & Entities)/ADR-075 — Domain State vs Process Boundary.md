# 🧾 ADR-075: Domain State vs Process Boundary

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

V doterajších ADR pre projektovú vrstvu (Kapitola E) sa miešajú dve zásadne odlišné veci:

| Čo sa mieša | Príklad |
|-------------|---------|
| **Doménový stav** | Projekt má `name`, `problem_statement`, `participants`, `stage` |
| **Procesný tok** | Ako vzniká projekt z Topicu, ako sa mení stage, kto môže čo schváliť |

Toto miešanie vedie v praxi k:

- **Niejasným zodpovednostiam** – je zmena stage súčasťou entity alebo procesu?
- **Ťažko testovateľnému kódu** – doménová logika je prepletená s workflow
- **Nejasným hraniciam** – kde končí „čo projekt je“ a začína „čo sa s ním deje“
- **Duplicitnej logike** – rovnaké pravidlá sú v entite aj v procese

---

## 3. ⚖️ Rozhodnutie

Systém zavádza **explicitnú hranicu** medzi:

1. **Domain State** – čo projekt **JE** (štruktúra, atribúty, invarianty)
2. **Process** – čo sa s projektom **DEJE** (vznik, zmeny stage, transitiony, workflow)

> **Domain State je statická pravda o entite. Process je dynamická zmena tejto pravdy podľa pravidiel.**

Tieto dve vrstvy sú **oddelené**:
- Domain State nevie o Processe
- Process číta Domain State a rozhoduje o zmenách
- Zmeny sa aplikujú cez explicitné **commands**

---

## 4. 🧠 Domain State („čo projekt JE“)

Domain State je **čistá štruktúra** – žiadna logika okrem:
- gettery / property
- jednoduché vypočítavané hodnoty (derived fields)
- validácia invariantov (či je stav konzistentný)

### 4.1 Čo patrí do Domain State

| Vrstva | Príklady |
|--------|----------|
| **Identita** | `Id`, `NodeId`, `CreatedAt`, `CreatedBy` |
| **Atribúty** | `Name`, `ProblemStatement`, `GoalStatement` |
| **Stav** | `Stage` (Draft, Forming, Active, Dormant, Archived) |
| **Vzťahy** | `OwnerId`, `ParticipantIds`, `OriginTopicId` |
| **Agregované hodnoty** | `CommitmentCount`, `LastActivityAt` |

### 4.2 Čo NEPATRÍ do Domain State

| Nepatrí | Prečo |
|---------|-------|
| `CanTransitionToActive()` | To je procesné pravidlo, nie stav |
| `TryPublish()` | To je command, nie vlastnosť |
| `IsValidForTransition()` | Procesná logika |
| Notifikačné pravidlá | Infraštruktúra, nie doména |

### 4.3 Príklad Domain State (iba štruktúra)

```text
Project {
    Id: string
    Name: string
    ProblemStatement: string
    GoalStatement: string? (voliteľné)
    SuccessCriteria: string? (voliteľné)
    Stage: ProjectStage (Draft, Forming, Active, Dormant, Archived)
    OwnerId: string
    ParticipantIds: List<string>
    CommitmentCount: int
    OriginTopicId: string? (voliteľné)
    InitialBranchId: string? (voliteľné)
    CreatedAt: DateTime
    LastActivityAt: DateTime
}
```

---

## 5. 🔄 Process („čo sa s tým DEJE“)

Process je **sada explicitných commandov a transition pravidiel**, ktoré menia Domain State.

### 5.1 Princípy Process vrstvy

| Princíp | Popis |
|---------|-------|
| **Command-Query Separation** | Čítanie (Domain State) je oddelené od zápisu (Command) |
| **Explicitné transitiony** | Každá zmena stage je samostatný command |
| **Process neukladá stav** | Stav je len v Domain State |
| **Process môže byť odmietnutý** | Ak nie sú splnené preconditions |

### 5.2 Príklady Commandov

| Command | Vstup | Výstup (ak úspešný) |
|---------|-------|---------------------|
| `PublishProject` | `projectId` | Stage: Draft → Forming |
| `ActivateProject` | `projectId` | Stage: Forming → Active |
| `AddParticipant` | `projectId, userId` | Pridá userId do ParticipantIds |
| `RemoveParticipant` | `projectId, userId` | Odstráni userId (ak nie je owner) |
| `RecordActivity` | `projectId` | Aktualizuje LastActivityAt |
| `MarkAsDormant` | `projectId` | Stage: Active → Dormant |
| `ReviveProject` | `projectId` | Stage: Dormant → Forming |
| `ArchiveProject` | `projectId` | Stage: Dormant → Archived |

### 5.3 Preconditions (kedy command môže byť vykonaný)

Každý command má **preconditions** – podmienky, ktoré musia platiť na aktuálnom Domain State.

| Command | Preconditions |
|---------|---------------|
| `PublishProject` | Stage == Draft AND has minimal content (name, problem_statement, aspoň jeden z initial_branch/goal/success) |
| `ActivateProject` | Stage == Forming AND ParticipantIds.Count >= 2 AND CommitmentCount >= 1 |
| `AddParticipant` | Stage != Archived |
| `MarkAsDormant` | Stage == Active AND (LastActivityAt > 30 dní OR ParticipantIds.Count < 2) |
| `ReviveProject` | Stage == Dormant |
| `ArchiveProject` | Stage == Dormant AND LastActivityAt > 90 dní |

### 5.4 Postconditions (čo command zmení)

| Command | Postconditions (zmeny) |
|---------|------------------------|
| `PublishProject` | Stage = Forming |
| `ActivateProject` | Stage = Active |
| `AddParticipant` | ParticipantIds obsahuje userId, LastActivityAt = now |
| `RecordActivity` | LastActivityAt = now |
| `MarkAsDormant` | Stage = Dormant |
| `ReviveProject` | Stage = Forming, LastActivityAt = now |
| `ArchiveProject` | Stage = Archived |

---

## 6. 🔄 Vzťah medzi Topic a Project (ako príklad procesu)

Transition Topic → Project je **proces**, nie stav.

### 6.1 Domain State (Topic)

```text
Topic {
    Id: string
    Name: string
    Stage: TopicStage (New, Emerging, Active, Candidate, Converted, Faded)
    InterestLevel: float
    ParticipantCount: int
    CommitmentCount: int
    ProjectReferenceId: string? (voliteľné, len ak Converted)
}
```

### 6.2 Process command: `ConvertTopicToProject`

| Vstup | `topicId, confirmingParticipantIds` |
|-------|-------------------------------------|
| **Preconditions** | Topic.Stage == Candidate OR (InterestLevel > 0.6 AND CommitmentCount >= 3 AND ParticipantCount >= 2) |
| **Postconditions** | 1. Vytvorí sa nový Project v stave Forming 2. Topic.Stage = Converted 3. Topic.ProjectReferenceId = nový projekt.Id 4. Project.OriginTopicId = topic.Id |

Tento command je **jediné miesto** v systéme, kde vzniká Project z Topicu.

---

## 7. 🚫 Čo sa NESMIE stať

| Zakázaná situácia | Prečo |
|-------------------|-------|
| Domain State obsahuje metódy ako `CanTransition()` | To je procesná logika, nie stav |
| Process mení Domain State priamo (bez commandu) | Stráca sa auditovateľnosť a kontrola |
| Process číta alebo mení stav iného procesu | Každý proces je samostatný |
| Notifikačná logika v Domain State | Notifikácie sú infraštruktúra |
| Process vrstva má svoje vlastné „stavové premenné“ | Jediný stav je v Domain State |

---

## 8. 📍 Kde ktorá vrstva žije

| Vrstva | Umiestnenie | Zodpovednosť |
|--------|-------------|--------------|
| **Domain State** | `Domain/Projects/Project.cs` | Definícia štruktúry projektu |
| **Domain State** | `Domain/Topics/Topic.cs` | Definícia štruktúry topicu |
| **Process Commands** | `Domain/Processes/ProjectCommands.cs` | Commandy nad projektom |
| **Process Commands** | `Domain/Processes/TopicCommands.cs` | Commandy nad topicom |
| **Transition Logic** | `Domain/Processes/TopicToProjectTransition.cs` | Konverzia Topic → Project |
| **Repository** | `Infrastructure/Repositories/` | Ukladanie a načítanie Domain State |
| **Command Handler** | `Application/Handlers/` | Volá procesy a ukladá zmeny |

---

## 9. 🧠 Výhody oddelenia

| Výhoda | Popis |
|--------|-------|
| **Čistá doména** | Domain State je jednoduchá, testovateľná štruktúra |
| **Procesy sú explicitné** | Každá zmena je pomenovaný command |
| **Auditovateľnosť** | Každý command môže byť logovaný |
| **Jednoduchá testovateľnosť** | Domain State testuješ izolovane, Process testuješ s mockovaným stavom |
| **Znovupoužiteľnosť** | Rovnaký Process môže byť volaný z UI, API, aj sync enginu |
| **Jedno miesto pravdy** | Prechod Topic→Project je len na jednom mieste |

---

## 10. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-042 | Node – Domain State je mapovaný na Node cez Typed Projections |
| ADR-076 | Typed Projections – projekcie sú Domain State |
| ADR-077 | MVPj – definuje, kedy je Project v akom stave (preconditions) |
| ADR-078 | Topic→Project Transition – je to Process command |
| ADR-082 | Domain Validation – validuje Domain State, nie Process |
| ADR-022 | Project Formation – proces vzniku projektu (teraz explicitný) |
| ADR-024 | Branching – zmeny vetiev sú Process commandy |

---

## 11. 🧪 Príklad použitia v aplikácii

```text
// UI zavolá command
command = new ActivateProjectCommand(projectId)
result = commandHandler.Execute(command)

// Command handler:
// 1. Načíta Project Domain State z repository
// 2. Overí preconditions (Stage == Forming, participants >= 2, commitment >= 1)
// 3. Vytvorí nový Project Domain State so Stage = Active
// 4. Uloží ho cez repository
// 5. Vráti result (úspech/neúspech + dôvod)

// Domain State (Project.cs) neobsahuje žiadnu z týchto logík
```

---

## 12. 🔥 Zhrnutie

> **Domain State hovorí, čo projekt JE. Process hovorí, čo sa s ním DEJE. Tieto dve veci nežijú v jednej triede.**

Tento ADR definuje:

- **Explicitnú hranicu** medzi stavom a procesom
- Čo patrí do Domain State (štruktúra, atribúty, invarianty)
- Čo patrí do Process (commandy, preconditions, postconditions)
- Čo sa nesmie stať (miešanie, priame zmeny stavu)
- Kde ktorá vrstva žije v projekte

---

## 13. 🧭 Finálna veta

> **Keď zmiešaš stav a proces, vznikne trieda, ktorá je príliš veľká na to, aby si jej rozumel, a príliš krehká na to, aby si ju zmenil. ADR-075 ich oddeľuje skôr, než bude neskoro.**

---

**Dátum:** 2026-04-16  
**Stav:** Proposed  
**Typ:** Architecture ADR (Separation of Concerns)  
**Verzia:** 1.0