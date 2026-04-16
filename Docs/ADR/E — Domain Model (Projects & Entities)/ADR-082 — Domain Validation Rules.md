# 🧾 ADR-082: Domain Validation Rules

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

Synergetikum má jednotný model entity – `Node` (ADR-042). Node môže byť rôznych typov: `Project`, `Topic`, `Argument`, `Branch`, `Person`, `Question`, `Event`.

ADR-042 definuje **technickú validáciu** (povinné polia, enumy, existujúce referencie). To však nestačí.

V reálnej prevádzke vznikajú **doménovo nezmyselné objekty**, ktoré sú technicky korektné, ale logicky chybné:

| Príklad | Prečo je to zlé |
|---------|-----------------|
| Argument, ktorý nepatrí do žiadneho projektu ani topicu | "visiaci" argument bez kontextu |
| Branch bez parent lineage | Vetva bez rodiča – odkiaľ prišla? |
| Projekt v stave `Active` bez jediného participanta | Active, ale nikto v ňom nie je |
| Topic v stave `converted_to_project` bez projektovej referencie | Tvrdí, že sa z neho stal projekt, ale neukazuje na ktorý |
| Argument, ktorý zároveň podporuje aj oponuje tej istej vetve | Logicky nemožné |
| Person Node bez public key | Osoba, ktorá nemôže nič podpísať |
| Event v minulosti, ktorý nikto nevytvoril | Udalosť, ktorá sa "stala sama" |

Bez doménovej validácie:

- systém produkuje **nezmyselné dáta**
- `ParticipationFilter` a `Personal AI` sa učia z chýb
- používateľ vidí **nekonzistentné stavy**
- neskoršie opravy sú **drahé alebo nemožné**

---

## 3. ⚖️ Rozhodnutie

Systém zavádza **doménovú validačnú vrstvu** – sadu pravidiel, ktoré sú nad rámec technickej validity a zabezpečujú **logickú konzistentnosť domény**.

> **Technická validita hovorí "dá sa to uložiť". Doménová validita hovorí "dáva to zmysel".**

---

## 4. 🧠 Pravidlá podľa typu Node

### 4.1 Project pravidlá

| # | Pravidlo | Validácia | Chyba |
|---|----------|-----------|-------|
| P1 | `Stage == Active` → musí mať aspoň 2 participantov (vrátane ownera) | `project.ParticipantIds.Count >= 2` | "Active projekt musí mať aspoň 2 participantov" |
| P2 | `Stage == Active` → musí mať aspoň 1 commitment signal | `project.CommitmentCount >= 1` | "Active projekt musí mať aspoň 1 commitment" |
| P3 | `Stage == Forming` → musí mať minimálny obsah (branch, goal, alebo criteria) | `hasContent == true` | "Forming projekt potrebuje initial_branch, goal_statement alebo success_criteria" |
| P4 | `Stage == Archived` → nemôže byť `Active` ani `Forming` | `stage != Active && stage != Forming` | "Archived projekt nemôže byť active ani forming" |
| P5 | `OriginTopicId != null` → Topic musí existovať a byť v stave `converted_to_project` | `topic.ProjectReference == project.Id` | "Topic neodkazuje späť na tento projekt" |
| P6 | `InitialBranchId != null` → Branch musí existovať a byť typu `Branch` | `branch.NodeType == NodeType.Branch` | "Initial branch neexistuje alebo nie je typu Branch" |

### 4.2 Topic pravidlá

| # | Pravidlo | Validácia | Chyba |
|---|----------|-----------|-------|
| T1 | `Stage == converted_to_project` → musí mať `ProjectReference` | `!string.IsNullOrEmpty(projectReference)` | "Topic je converted_to_project, ale chýba odkaz na projekt" |
| T2 | `Stage == converted_to_project` → Projekt musí existovať | `project != null` | "Projekt, na ktorý Topic odkazuje, neexistuje" |
| T3 | `Stage != converted_to_project` → `ProjectReference` musí byť null | `projectReference == null` | "Topic nie je converted_to_project, ale má odkaz na projekt" |
| T4 | Topic musí mať aspoň 1 odpoveď (response) alebo byť vo fáze `new` | `responseCount > 0 || stage == New` | "Topic bez odpovedí môže byť len v stave New" |
| T5 | `OriginQuestionId` musí odkazovať na existujúcu Question Node | `question != null` | "Origin otázka neexistuje" |

### 4.3 Argument pravidlá

| # | Pravidlo | Validácia | Chyba |
|---|----------|-----------|-------|
| A1 | Argument musí patriť do projektu ALEBO topicu (nie obom, nie žiadnemu) | `(projectId != null) XOR (topicId != null)` | "Argument musí patriť práve do jedného projektu alebo topicu" |
| A2 | `Stance` nemôže byť `Support` aj `Oppose` zároveň | `stance != SupportAndOppose` | "Argument nemôže zároveň podporovať aj oponovať" |
| A3 | Argument nemôže podporovať aj oponovať tú istú vetvu | `!supports.Contains(branchId) || !opposes.Contains(branchId)` | "Argument nemôže zároveň podporovať a oponovať tú istú vetvu" |
| A4 | `Confidence` musí byť v rozsahu 0.0 – 1.0 | `confidence is >= 0 and <= 1` | "Confidence musí byť medzi 0 a 1" |
| A5 | Ak `Evidence` nie je prázdne, každá položka musí byť string min 10 znakov | `evidence.All(e => e.Length >= 10)` | "Evidence položka je príliš krátka (min 10 znakov)" |

### 4.4 Branch pravidlá

| # | Pravidlo | Validácia | Chyba |
|---|----------|-----------|-------|
| B1 | Branch musí patriť do projektu | `projectId != null` | "Branch musí patriť do projektu" |
| B2 | Projekt musí existovať | `project != null` | "Projekt neexistuje" |
| B3 | `ParentBranchId != null` → Parent branch musí existovať a patriť do rovnakého projektu | `parentBranch.ProjectId == branch.ProjectId` | "Parent branch patrí do iného projektu" |
| B4 | Branch nemôže byť sám sebe rodičom | `branch.Id != parentBranchId` | "Branch nemôže byť rodičom sám seba" |
| B5 | Cyklus v lineage nie je povolený | `!HasCycle(branch)` | "Branch lineage obsahuje cyklus" |
| B6 | `MergeStatus` nemôže byť `Merged`, ak `MergedIntoBranchId` je null | `mergeStatus != Merged || mergedIntoBranchId != null` | "Merged branch musí mať cieľ" |

### 4.5 Person pravidlá

| # | Pravidlo | Validácia | Chyba |
|---|----------|-----------|-------|
| PER1 | Person musí mať `PublicKey` (aspoň 44 znakov, base64) | `publicKey.Length >= 44` | "Chýba alebo je neplatný public key" |
| PER2 | `DisplayName` nemôže byť prázdny | `!string.IsNullOrWhiteSpace(displayName)` | "Display name nemôže byť prázdny" |
| PER3 | Jeden person nemôže byť owner dvoch aktívnych projektov? | (voliteľné, skôr warning) | – |

### 4.6 Question pravidlá

| # | Pravidlo | Validácia | Chyba |
|---|----------|-----------|-------|
| Q1 | Otázka musí mať aspoň jednu perspektívu | `perspectives.Count > 0` | "Otázka musí mať aspoň jednu perspektívu" |
| Q2 | `ExpectedAnswerType` musí byť z definovaného enumu | `enum.IsDefined(expectedAnswerType)` | "Neplatný expected answer type" |

### 4.7 Event pravidlá

| # | Pravidlo | Validácia | Chyba |
|---|----------|-----------|-------|
| E1 | `StartTime` musí byť v budúcnosti (pri vytvorení) | `startTime > DateTime.UtcNow` | "Udalosť nemôže byť v minulosti" |
| E2 | `EndTime` musí byť > `StartTime` | `endTime > startTime` | "EndTime musí byť po StartTime" |
| E3 | `Location` nemôže byť prázdny, ak je udalosť fyzická | `location.Length > 0 || isVirtual == true` | "Fyzická udalosť musí mať miesto" |

---

## 5. 🔄 Vzťahové pravidlá (naprieč typmi)

| # | Pravidlo | Validácia | Chyba |
|---|----------|-----------|-------|
| R1 | Node nemôže referencovať sám seba (okrem špeciálnych prípadov) | `relation.targetId != node.id` | "Node nemôže referencovať sám seba" |
| R2 | Ak `relation_type = parent_of`, target musí existovať | `targetNode != null` | "Parent_of odkazuje na neexistujúci Node" |
| R3 | Ak `relation_type = forked_from`, source musí existovať | `sourceNode != null` | "Forked_from odkazuje na neexistujúci Node" |
| R4 | Žiadne dva Node-y nemôžu mať rovnaký `node_id` | unique constraint | "Duplicate node_id" |

---

## 6. 🧠 Validácia v kóde (C#, minimálna kostra)

```csharp
namespace Synergetikum.Domain.Validation;

public interface IDomainValidator
{
    ValidationResult Validate(Node node);
}

public record ValidationResult
{
    public bool IsValid { get; init; }
    public List<string> Errors { get; init; } = new();
    public List<string> Warnings { get; init; } = new();
}

public class DomainValidator : IDomainValidator
{
    private readonly INodeRepository _repository;
    
    public DomainValidator(INodeRepository repository)
    {
        _repository = repository;
    }
    
    public ValidationResult Validate(Node node)
    {
        return node.NodeType switch
        {
            NodeType.Project => ValidateProject(node),
            NodeType.Topic => ValidateTopic(node),
            NodeType.Argument => ValidateArgument(node),
            NodeType.Branch => ValidateBranch(node),
            NodeType.Person => ValidatePerson(node),
            NodeType.Question => ValidateQuestion(node),
            NodeType.Event => ValidateEvent(node),
            _ => ValidationResult.Success()
        };
    }
    
    private ValidationResult ValidateProject(Node node)
    {
        var errors = new List<string>();
        
        // P1: Active musí mať aspoň 2 participantov
        if (node.GetStage() == ProjectStage.Active)
        {
            var participants = node.GetParticipants();
            if (participants.Count < 2)
                errors.Add("P1: Active projekt musí mať aspoň 2 participantov");
        }
        
        // P5: OriginTopicId -> Topic musí odkazovať späť
        var originTopicId = node.GetOriginTopicId();
        if (!string.IsNullOrEmpty(originTopicId))
        {
            var topic = _repository.GetById(originTopicId);
            if (topic == null)
                errors.Add("P5: Origin topic neexistuje");
            else if (topic.GetProjectReference() != node.Id)
                errors.Add("P5: Topic neodkazuje späť na tento projekt");
        }
        
        return new ValidationResult { IsValid = errors.Count == 0, Errors = errors };
    }
    
    private ValidationResult ValidateTopic(Node node)
    {
        var errors = new List<string>();
        
        // T1: converted_to_project musí mať ProjectReference
        if (node.GetStage() == TopicStage.ConvertedToProject)
        {
            var projectRef = node.GetProjectReference();
            if (string.IsNullOrEmpty(projectRef))
                errors.Add("T1: Topic je converted_to_project, ale chýba odkaz na projekt");
        }
        
        return new ValidationResult { IsValid = errors.Count == 0, Errors = errors };
    }
    
    private ValidationResult ValidateArgument(Node node)
    {
        var errors = new List<string>();
        
        // A1: Argument musí patriť práve do jedného projektu alebo topicu
        var projectId = node.GetProjectId();
        var topicId = node.GetTopicId();
        
        if (string.IsNullOrEmpty(projectId) && string.IsNullOrEmpty(topicId))
            errors.Add("A1: Argument musí patriť do projektu alebo topicu");
        
        if (!string.IsNullOrEmpty(projectId) && !string.IsNullOrEmpty(topicId))
            errors.Add("A1: Argument nemôže patriť do projektu aj topicu zároveň");
        
        return new ValidationResult { IsValid = errors.Count == 0, Errors = errors };
    }
    
    private ValidationResult ValidateBranch(Node node)
    {
        var errors = new List<string>();
        
        // B1: Branch musí patriť do projektu
        var projectId = node.GetProjectId();
        if (string.IsNullOrEmpty(projectId))
            errors.Add("B1: Branch musí patriť do projektu");
        
        // B4: Branch nemôže byť sám sebe rodičom
        var parentId = node.GetParentBranchId();
        if (parentId == node.Id)
            errors.Add("B4: Branch nemôže byť rodičom sám seba");
        
        return new ValidationResult { IsValid = errors.Count == 0, Errors = errors };
    }
    
    private ValidationResult ValidatePerson(Node node)
    {
        var errors = new List<string>();
        
        // PER1: Public key musí existovať
        var publicKey = node.GetPublicKey();
        if (string.IsNullOrEmpty(publicKey) || publicKey.Length < 44)
            errors.Add("PER1: Chýba alebo je neplatný public key");
        
        return new ValidationResult { IsValid = errors.Count == 0, Errors = errors };
    }
    
    private ValidationResult ValidateQuestion(Node node)
    {
        var errors = new List<string>();
        
        // Q1: Aspoň jedna perspektíva
        var perspectives = node.GetPerspectives();
        if (perspectives.Count == 0)
            errors.Add("Q1: Otázka musí mať aspoň jednu perspektívu");
        
        return new ValidationResult { IsValid = errors.Count == 0, Errors = errors };
    }
    
    private ValidationResult ValidateEvent(Node node)
    {
        var errors = new List<string>();
        
        // E1: StartTime v budúcnosti (pri vytvorení)
        var startTime = node.GetStartTime();
        if (startTime < DateTime.UtcNow)
            errors.Add("E1: Udalosť nemôže byť v minulosti");
        
        return new ValidationResult { IsValid = errors.Count == 0, Errors = errors };
    }
}
```

---

## 7. 🚫 Čo sa NESMIE stať

| Zakázaná situácia | Prečo |
|-------------------|-------|
| Uloženie doménovo nevalidného Node-u | Systém by mal dáta, ktorým nerozumie |
| Ignorovanie validačných chýb | Maskovanie problému |
| Validácia len pri vytvorení | Dáta sa môžu zmeniť a stať sa nevalidnými |
| Doménová validácia v UI vrstve | Porušenie separácie zodpovedností |

---

## 8. 📍 Kde a kedy validovať

| Miesto | Kedy | Čo robiť s chybou |
|--------|------|-------------------|
| **Repository.Save()** | Pred uložením do databázy | Throw `DomainValidationException` |
| **API endpoint** | Pri vytváraní/update | Vrátiť 400 s chybami |
| **Sync engine** | Pred aplikovaním remote zmeny | Odmietnuť zmenu, logovať |
| **Periodická revalidácia** | Každých 24h | Zaznamenať do logu, notifikovať admina |

---

## 9. 🧪 Testovacie scenáre (pre pilot)

| Scenár | Očakávaný výsledok |
|--------|---------------------|
| Vytvoriť Argument bez projektu aj topicu | Validation failed: "A1" |
| Vytvoriť Branch bez projektu | Validation failed: "B1" |
| Vytvoriť Topic s `converted_to_project` ale bez `ProjectReference` | Validation failed: "T1" |
| Vytvoriť Person bez public key | Validation failed: "PER1" |
| Vytvoriť Event v minulosti | Validation failed: "E1" |
| Update projektu z Active na Forming, ale stále má 2 participantov | Valid, len warning |

---

## 10. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-042 | Node model – toto ADR validuje Node-y |
| ADR-077 | MVPj – projektové pravidlá (P1-P6) |
| ADR-078 | Topic→Project transition – topicové pravidlá (T1-T5) |
| ADR-024 | Branching – branch pravidlá (B1-B6) |
| ADR-025 | Argumentation – argument pravidlá (A1-A5) |
| ADR-030 | Sync – validácia pred synchronizáciou |

---

## 11. 🔥 Zhrnutie

> **Technická validita hovorí "dá sa to uložiť". Doménová validita hovorí "dáva to zmysel".**

Tento ADR definuje:

- **Pravidlá pre Project** (6 pravidiel)
- **Pravidlá pre Topic** (5 pravidiel)
- **Pravidlá pre Argument** (5 pravidiel)
- **Pravidlá pre Branch** (6 pravidiel)
- **Pravidlá pre Person, Question, Event**
- **Vzťahové pravidlá** (naprieč typmi)
- **Kedy a kde validovať**

---

## 12. 🧭 Finálna veta

> **Bez doménovej validácie je systém len syntakticky korektný, ale sémanticky hlúpy. ADR-082 zaisťuje, že naše dáta dávajú zmysel – nielen že sa dajú uložiť.**

---

**Dátum:** 2026-04-16  
**Stav:** Proposed  
**Typ:** Domain Validation ADR  
**Verzia:** 1.0