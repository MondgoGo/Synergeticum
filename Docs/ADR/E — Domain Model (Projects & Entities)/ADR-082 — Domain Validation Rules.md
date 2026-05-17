# 🧾 ADR-082: Domain Validation Rules

## 1. 📌 Status

**Accepted** (revízia 2.0)

---

## 2. 🎯 Kontext

Synergetikum má jednotný model entity – `Node` (ADR-042). Node môže byť rôznych typov: `Project`, `Topic`, `Argument`, `Branch`, `Person`, `Question`, `Event`.

ADR-042 definuje **technickú validáciu** (povinné polia, enumy, existujúce referencie). To však nestačí.

V reálnej prevádzke vznikajú **doménovo nezmyselné objekty**, ktoré sú technicky korektné, ale logicky chybné:

| Príklad | Prečo je to zlé |
|---------|-----------------|
| Argument, ktorý nepatrí do žiadneho projektu ani topicu | "Visiaci" argument bez kontextu |
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

## 4. 🧠 Typy violácií (Hard / Soft / Sync)

V distribuovanom P2P systéme **nie je validácia nikdy len "hard stop"**. Rozlišujeme tri typy violácií:

| Typ | Správanie | Príklad |
|-----|-----------|---------|
| **Hard violation** | Reject – operácia sa nevykoná | Branch bez projektu (B1) – nemôže existovať |
| **Soft violation** | Warning + flag – uloží sa, ale označí | Argument bez evidence (A5) – je slabý, ale môže existovať |
| **Sync violation** | Accept + quarantine – uloží sa, ale nie je aktívny | Node z remote peera, ktorý je nevalidný – počkáme na doplnenie dát |

### 4.1 Príklad rozdelenia podľa typu

| Pravidlo | Typ | Zdôvodnenie |
|----------|-----|-------------|
| P1: Active projekt musí mať 2 participantov | Soft | Môže byť dočasný stav počas syncu |
| P5: Topic musí odkazovať späť na projekt | Hard | Bez toho je referenčná integrita porušená |
| A1: Argument musí patriť do projektu alebo topicu | Hard | Visiaci argument nemá zmysel |
| A5: Evidence aspoň 10 znakov | Soft | Je to kvalita, nie existencia |
| B4: Branch nemôže byť rodičom sám seba | Hard | Logická nemožnosť |

---

## 5. 🧠 Validation modes (Strict / Tolerant / Revalidation)

V závislosti od kontextu (lokálna zmena vs sync) sa používajú rôzne režimy validácie:

| Mód | Kedy | Správanie |
|-----|------|-----------|
| **Strict** | Lokálne vytvorenie / update | Vyžaduje existenciu všetkých referencií. Hard violation → reject |
| **Tolerant** | Sync z remote peera | Ak referencia neexistuje, označiť ako `pending` a skúsiť neskôr. Soft violation → warning |
| **Revalidation** | Background integrity pass | Overuje existujúce entity, neblokuje operácie |

### 5.1 Príklad pre sync (Tolerant mode)

```text
Pri syncu:
1. Prijmi Node aj keď referencie chýbajú
2. Označ ho ako ValidationState.PendingReferences
3. Spusti background job na dohľadanie chýbajúcich Node-ov
4. Ak po X dňoch stále chýbajú → mark as ValidationState.Orphaned
```

---

## 6. 🧠 Domain Invariants vs Operational Policies

Je dôležité rozlišovať medzi **domain invariantmi** (pravdy domény) a **operational policies** (business pravidlá, ktoré sa môžu meniť):

| Typ | Príklad | Charakter | Kde žijú |
|-----|---------|-----------|----------|
| **Domain invariant** | Argument musí patriť do projektu | Bez toho to nie je argument | `Domain/Validation/DomainRules.cs` |
| **Operational policy** | Person nemôže byť owner dvoch aktívnych projektov | Rozhodnutie, nie pravda | `Application/Policies/ProjectPolicies.cs` |

> Policy rules majú **nižšiu prioritu** a môžu byť **prepísateľné** (napr. admin, trusted circle).

---

## 7. 🧠 Validation Result ako first-class object

Výsledok validácie nie je len `bool + zoznam chýb`. Je to **first-class object** s metadátami:

```text
ValidationResult {
    IsValid: bool
    Severity: Hard | Soft | Sync
    Source: DomainInvariant | OperationalPolicy | Technical
    Mode: Strict | Tolerant | Revalidation
    Errors: List<ValidationError>
    Warnings: List<ValidationWarning>
    RemediationHint: string (voliteľné)
    PendingReferences: List<string> (voliteľné)
}
```

---

## 8. 🧠 Continuous Validation (validácia ako proces)

Validácia nie je len **pri uložení**. Niektoré pravidlá sa môžu stať neskôr neplatnými aj bez zmeny objektu.

| Scenár | Problém |
|--------|---------|
| Projekt je Active (mal 2 participantov) | Jeden participant odíde → teraz má 1 |
| Branch bol validný | Rodičovská vetva sa zmení → cyklus |

### 8.1 Mechanizmy continuous validation

| Mechanizmus | Kedy | Rozsah |
|-------------|------|--------|
| **On-change validation** | Pri zmene objektu | Iba daný objekt |
| **Event-triggered revalidation** | Keď sa zmení referencovaný Node | Iba závislé objekty (scope-limited) |
| **Periodic revalidation (fallback)** | Každých 24h | Len `active` a `dormant` entity |

> Continuous validation **nesmie** znamenať neustály prepočet celého grafu. Používa sa event-triggered revalidation s obmedzeným scope.

---

## 9. 📋 Pravidlá podľa typu Node

### 9.1 Project pravidlá

| # | Pravidlo | Typ | Validácia | Chyba |
|---|----------|-----|-----------|-------|
| P1 | `Stage == Active` → musí mať aspoň 2 participantov (vrátane ownera) | Soft | `project.ParticipantIds.Count >= 2` | "Active projekt musí mať aspoň 2 participantov" |
| P2 | `Stage == Active` → musí mať aspoň 1 commitment signal | Soft | `project.CommitmentCount >= 1` | "Active projekt musí mať aspoň 1 commitment" |
| P3 | `Stage == Forming` → musí mať minimálny obsah | Soft | `hasContent == true` | "Forming projekt potrebuje initial_branch, goal_statement alebo success_criteria" |
| P4 | `Stage == Archived` → nemôže byť `Active` ani `Forming` | Hard | `stage != Active && stage != Forming` | "Archived projekt nemôže byť active ani forming" |
| P5 | `OriginTopicId != null` → Topic musí existovať a odkazovať späť | Hard | `topic.ProjectReference == project.Id` | "Topic neodkazuje späť na tento projekt" |
| P6 | `InitialBranchId != null` → Branch musí existovať | Hard | `branch.NodeType == NodeType.Branch` | "Initial branch neexistuje alebo nie je typu Branch" |

### 9.2 Topic pravidlá

| # | Pravidlo | Typ | Validácia | Chyba |
|---|----------|-----|-----------|-------|
| T1 | `Stage == converted_to_project` → musí mať `ProjectReference` | Hard | `!string.IsNullOrEmpty(projectReference)` | "Topic je converted_to_project, ale chýba odkaz na projekt" |
| T2 | `Stage == converted_to_project` → Projekt musí existovať | Hard | `project != null` | "Projekt, na ktorý Topic odkazuje, neexistuje" |
| T3 | `Stage != converted_to_project` → `ProjectReference` musí byť null | Hard | `projectReference == null` | "Topic nie je converted_to_project, ale má odkaz na projekt" |
| T4 | Topic musí mať aspoň 1 odpoveď (response) alebo byť vo fáze `new` | Soft | `responseCount > 0 || stage == New` | "Topic bez odpovedí môže byť len v stave New" |
| T5 | `OriginQuestionId` musí odkazovať na existujúcu Question Node | Hard | `question != null` | "Origin otázka neexistuje" |

### 9.3 Argument pravidlá

| # | Pravidlo | Typ | Validácia | Chyba |
|---|----------|-----|-----------|-------|
| A1 | Argument musí patriť do projektu ALEBO topicu (nie obom, nie žiadnemu) | Hard | `(projectId != null) XOR (topicId != null)` | "Argument musí patriť práve do jedného projektu alebo topicu" |
| A2 | `Stance` nemôže byť `Support` aj `Oppose` zároveň | Hard | `stance != SupportAndOppose` | "Argument nemôže zároveň podporovať aj oponovať" |
| A3 | Argument nemôže podporovať aj oponovať tú istú vetvu | Hard | `!supports.Contains(branchId) || !opposes.Contains(branchId)` | "Argument nemôže zároveň podporovať a oponovať tú istú vetvu" |
| A4 | `Confidence` musí byť v rozsahu 0.0 – 1.0 | Hard | `confidence is >= 0 and <= 1` | "Confidence musí byť medzi 0 a 1" |
| A5 | Ak `Evidence` nie je prázdne, každá položka musí byť string min 10 znakov | Soft | `evidence.All(e => e.Length >= 10)` | "Evidence položka je príliš krátka (min 10 znakov)" |

### 9.4 Branch pravidlá

| # | Pravidlo | Typ | Validácia | Chyba |
|---|----------|-----|-----------|-------|
| B1 | Branch musí patriť do projektu | Hard | `projectId != null` | "Branch musí patriť do projektu" |
| B2 | Projekt musí existovať | Hard | `project != null` | "Projekt neexistuje" |
| B3 | `ParentBranchId != null` → Parent branch musí existovať a patriť do rovnakého projektu | Hard | `parentBranch.ProjectId == branch.ProjectId` | "Parent branch patrí do iného projektu" |
| B4 | Branch nemôže byť sám sebe rodičom | Hard | `branch.Id != parentBranchId` | "Branch nemôže byť rodičom sám seba" |
| B5 | Cyklus v lineage nie je povolený | Hard | `!HasCycle(branch)` | "Branch lineage obsahuje cyklus" |
| B6 | `MergeStatus` nemôže byť `Merged`, ak `MergedIntoBranchId` je null | Hard | `mergeStatus != Merged || mergedIntoBranchId != null` | "Merged branch musí mať cieľ" |

### 9.5 Person pravidlá

| # | Pravidlo | Typ | Validácia | Chyba |
|---|----------|-----|-----------|-------|
| PER1 | Person musí mať `PublicKey` (aspoň 44 znakov, base64) | Hard | `publicKey.Length >= 44` | "Chýba alebo je neplatný public key" |
| PER2 | `DisplayName` nemôže byť prázdny | Hard | `!string.IsNullOrWhiteSpace(displayName)` | "Display name nemôže byť prázdny" |
| PER3 | Jeden person nemôže byť owner dvoch aktívnych projektov | Operational Policy | `activeProjectsCount <= 1` | "Person môže byť owner max jedného aktívneho projektu" |

### 9.6 Question pravidlá

| # | Pravidlo | Typ | Validácia | Chyba |
|---|----------|-----|-----------|-------|
| Q1 | Otázka musí mať aspoň jednu perspektívu | Hard | `perspectives.Count > 0` | "Otázka musí mať aspoň jednu perspektívu" |
| Q2 | `ExpectedAnswerType` musí byť z definovaného enumu | Hard | `enum.IsDefined(expectedAnswerType)` | "Neplatný expected answer type" |

### 9.7 Event pravidlá

| # | Pravidlo | Typ | Validácia | Chyba |
|---|----------|-----|-----------|-------|
| E1 | `StartTime` musí byť v budúcnosti (pri vytvorení) | Soft | `startTime > DateTime.UtcNow` | "Udalosť nemôže byť v minulosti" |
| E2 | `EndTime` musí byť > `StartTime` | Hard | `endTime > startTime` | "EndTime musí byť po StartTime" |
| E3 | `Location` nemôže byť prázdny, ak je udalosť fyzická | Soft | `location.Length > 0 || isVirtual == true` | "Fyzická udalosť musí mať miesto" |

---

## 10. 🔄 Vzťahové pravidlá (naprieč typmi)

| # | Pravidlo | Typ | Validácia | Chyba |
|---|----------|-----|-----------|-------|
| R1 | Node nemôže referencovať sám seba (okrem špeciálnych prípadov) | Hard | `relation.targetId != node.id` | "Node nemôže referencovať sám seba" |
| R2 | Ak `relation_type = parent_of`, target musí existovať | Hard | `targetNode != null` | "Parent_of odkazuje na neexistujúci Node" |
| R3 | Ak `relation_type = forked_from`, source musí existovať | Hard | `sourceNode != null` | "Forked_from odkazuje na neexistujúci Node" |
| R4 | Žiadne dva Node-y nemôžu mať rovnaký `node_id` | Hard | unique constraint | "Duplicate node_id" |

---

## 11. 📍 Kde a kedy validovať

| Miesto | Kedy | Mód | Typ validácie | Čo robiť s chybou |
|--------|------|-----|---------------|-------------------|
| **Repository.Save()** | Pred uložením do databázy | Strict | Domain + Operational | Hard violation → throw; Soft → warning do logu |
| **API endpoint** | Pri vytváraní/update | Strict | Domain + Operational | Vrátiť 4xx s chybami |
| **Sync engine** | Pred aplikovaním remote zmeny | Tolerant | Domain len (žiadne Operational) | Sync violation → quarantine; Hard → odmietnuť zmenu |
| **Periodická revalidácia** | Každých 24h | Revalidation | Domain + Operational | Zaznamenať do logu, notifikovať admina |
| **Event-triggered** | Pri zmene referencovaného Node | Revalidation | Domain len | Aktualizovať stav závislých objektov |

---

## 12. 🧪 Testovacie scenáre (pre pilot)

| Scenár | Typ | Očakávaný výsledok |
|--------|-----|---------------------|
| Vytvoriť Argument bez projektu aj topicu | Hard | Reject |
| Vytvoriť Branch bez projektu | Hard | Reject |
| Vytvoriť Topic s `converted_to_project` ale bez `ProjectReference` | Hard | Reject |
| Vytvoriť Person bez public key | Hard | Reject |
| Vytvoriť Event v minulosti | Soft | Uloží sa s warningom |
| Update projektu z Active na Forming, ale stále má 2 participantov | Soft | Valid, len warning |
| Sync prijme Node s chýbajúcou referenciou | Sync | Uloží sa ako `PendingReferences` |
| Po 7 dňoch referencia stále chýba | Sync | `Orphaned` |
| Participant odíde z Active projektu | Continuous | Po revalidácii → `Dormant` |
| Branch vytvorí cyklus v lineage | Hard | Reject |

---

## 13. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-042 | Node model – toto ADR validuje Node-y |
| ADR-077 | MVPj – projektové pravidlá (P1-P6) |
| ADR-078 | Topic→Project transition – topicové pravidlá (T1-T5) |
| ADR-024 | Branching – branch pravidlá (B1-B6) |
| ADR-025 | Argumentation – argument pravidlá (A1-A5) |
| ADR-030 | Sync – validácia pred synchronizáciou (Tolerant mode) |
| ADR-075 | Domain State vs Process Boundary – domain invariants vs operational policies |
| ADR-076 | Typed Projections – projekcie používajú validované Node-y |

---

## 14. 🔥 Zhrnutie

> **Technická validita hovorí "dá sa to uložiť". Doménová validita hovorí "dáva to zmysel". V P2P svete nie je validácia nikdy len "hard stop" – niekedy je to warning, niekedy sync problém, niekedy business policy, ktorá sa môže zmeniť.**

Tento ADR definuje:

- **Typy violácií** – Hard / Soft / Sync
- **Validation modes** – Strict / Tolerant / Revalidation
- **Domain Invariants vs Operational Policies** – oddelené vrstvy
- **Validation Result** – first-class object s metadátami
- **Continuous validation** – on-change, event-triggered, periodic
- **Pravidlá pre Project** (6 pravidiel s typmi)
- **Pravidlá pre Topic** (5 pravidiel s typmi)
- **Pravidlá pre Argument** (5 pravidiel s typmi)
- **Pravidlá pre Branch** (6 pravidiel s typmi)
- **Pravidlá pre Person, Question, Event**
- **Vzťahové pravidlá** (naprieč typmi)
- **Kedy a kde validovať**

---

## 15. 🧭 Finálna veta

> **Bez doménovej validácie je systém len syntakticky korektný, ale sémanticky hlúpy. Ale v distribuovanom svete nie je validácia nikdy "hard stop". Niekedy je to len nedokončená synchronizácia, niekedy slabá kvalita a niekedy skutočný invariantný problém. ADR-082 ich rozlišuje – a tým zaisťuje, že systém prežije chaos reálneho P2P sveta.**

---

**Dátum:** 2026-04-16  
**Stav:** Accepted  
**Typ:** Domain Validation ADR  
**Verzia:** 2.0 (revízia podľa pripomienok)