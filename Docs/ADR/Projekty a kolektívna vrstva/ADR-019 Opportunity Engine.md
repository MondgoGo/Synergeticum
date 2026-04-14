# ADR-019 — Opportunity Engine

## Status

Proposed

---

## Kontext

Synergetikum už definuje:

* otázky a perspektívy (ADR-009, ADR-010)
* personalizáciu (ADR-011)
* vznik tém a projektov (ADR-012)
* participáciu (ADR-013)
* spoluprácu a role/needs (ADR-017)
* simuláciu scenárov (ADR-018)

Chýba vrstva, ktorá z týchto informácií **proaktívne identifikuje konkrétne príležitosti zapojenia** pre používateľov.

> Ako systém zistí, kto je vhodný ako realizátor, investor, organizátor alebo analytik – a kedy mu to ponúkne?

---

## Rozhodnutie

> Synergetikum obsahuje Opportunity Engine, ktorý kontinuálne generuje, hodnotí a ponúka **Opportunity** objekty (príležitosti) spájajúce: používateľa × rolu × projekt × moment.

Opportunity Engine je nadstavba nad Matching (ADR-017):

* Matching hovorí „kto by sa hodil“
* Opportunity Engine hovorí „**toto je teraz pre teba konkrétna príležitosť**"

---

## Core princíp

> Správna príležitosť je časovo citlivé spojenie schopnosti, záujmu a potreby.

---

## Definícia Opportunity

Opportunity je explicitný objekt s atribútmi:

* `OpportunityId`
* `ProjectId`
* `RoleId` (Need)
* `UserId` (target)
* `FitScore`
* `Urgency`
* `Impact`
* `Confidence`
* `CreatedAt`, `ExpiresAt`
* `Reasoning` (pre explainability)
* `RequiredActions` (čo presne spraviť)

---

## Typy rolí (Role taxonomy)

Minimálny set pre MVP:

* **Executor (Realizátor)** — vykoná konkrétnu úlohu
* **Organizer (Organizátor)** — koordinuje ľudí/čas
* **Analyst (Analytik)** — spracuje dáta/varianty
* **Investor (Finančný/zdrojový)** — poskytne zdroje (peniaze, materiál, priestor)

Rozšíriteľné:

* Designer, Legal, Marketing, Community, Ops, atď.

---

## Vstupy do engine

Opportunity Engine využíva:

* Personal AI profil:

  * Capability Profile
  * Interest Profile
  * Participation Profile
  * Reliability (z minulých akcií)
* Project/Topic dáta:

  * aktuálne Needs (roles)
  * stav projektu
  * vetvy a priority
* Kontext:

  * lokalita, čas, dostupnosť
* História interakcií:

  * čo používateľ prijal/odmietol

---

## Scoring model

Každá príležitosť má kompozitné skóre:

```
FitScore = f(
  capability_match,
  interest_alignment,
  participation_readiness,
  reliability,
  context_fit
)

PriorityScore = f(
  FitScore,
  Urgency,
  Impact,
  Confidence
)
```

Kľúč:

* **FitScore** = „či je to pre teba“
* **PriorityScore** = „či to máš vidieť teraz“

---

## Urgency & Impact

* **Urgency**: ako veľmi projekt potrebuje túto rolu teraz (deadline, blokovanie)
* **Impact**: aký veľký posun spôsobí obsadenie roly

Príklad:

* „Potrebujeme organizátora na zajtrajšie stretnutie“ → vysoká urgency
* „Potrebujeme analýzu trhu“ → stredná urgency, vysoký impact

---

## Opportunity lifecycle

1. **Detection** — vznikne potreba (Role/Need)
2. **Generation** — vytvoria sa kandidátne Opportunities pre vhodných userov
3. **Scoring & Ranking** — výpočet PriorityScore
4. **Delivery** — zobrazenie používateľovi
5. **User Action** — accept / decline / snooze / ignore
6. **Outcome Tracking** — či bola rola naplnená a s akým výsledkom
7. **Learning** — aktualizácia profilov a váh

---

## Delivery (UX)

Princípy:

* nízky počet (1–3 príležitosti naraz)
* konkrétnosť (čo, prečo, koľko času)
* okamžitá akcia

Formát:

* „Toto vieš spraviť“
* „Projekt X potrebuje Y“
* „Odhad: 2 hodiny, tento týždeň“

Akcie:

* **Accept** (commit)
* **Decline** (signal)
* **Snooze** (neskôr)
* **Details** (viac info + scenáre z ADR-018)

---

## Anti-overload mechanizmy

* Rate limiting (max N príležitostí/deň)
* Cooldown po odmietnutí
* Diversity (rôzne projekty/role)
* Confidence gating (len dostatočne isté matchy)

---

## Anti-manipulation pravidlá

* nevnucovať (žiadny dark pattern)
* transparentné „prečo práve ty“
* možnosť opt-out per role/project

---

## Explainability

Každá príležitosť obsahuje „Prečo“:

* „Máš skúsenosti s X“
* „Zaujímal si sa o Y“
* „Projekt v tvojej lokalite potrebuje Z“

---

## Vzťah k participácii (ADR-013)

* Acceptance → zvyšuje ParticipationLevel
* Consistent delivery → zvyšuje Reliability
* No-show / nedokončenie → znižuje Reliability

---

## Vzťah k projektom (ADR-012)

* Rýchlosť obsadzovania rolí ovplyvňuje **Project health**
* Neobsadené kritické role → riziko stagnácie

---

## Architektonické implikácie

### Entities

* Opportunity
* Role (Need)
* UserCapability
* ReliabilityScore

### Services

* OpportunityGenerationService
* OpportunityScoringService
* OpportunityDeliveryService
* OutcomeTrackingService

### Storage

* Opportunity store (krátkodobý, TTL)
* History store (pre learning)

---

## Riziká

### 1. Overmatching

* príliš veľa návrhov → únava

### 2. Undermatching

* projekty bez ľudí → stagnácia

### 3. Skill misclassification

* nesprávny odhad schopností

### 4. Gaming the system

* falošný commitment

Mitigácia:

* behavior-based learning
* reliability scoring
* gradual trust

---

## Alternatívy

### Manuálne hľadanie

❌ neškáluje

### Broadcast „kto chce?“

❌ nízka kvalita a šum

### Centrálne priraďovanie

❌ porušenie decentralizácie

---

## Väzby na ADR

* ADR-017 — Collaboration Model
* ADR-013 — Participation Model
* ADR-011 — Personalization
* ADR-018 — Simulation Engine

---

## Zhrnutie

Opportunity Engine premieňa potenciál spolupráce na konkrétne, časovo relevantné príležitosti pre jednotlivca.

---

## Finálna veta

> Synergetikum nečaká, kým sa ľudia zapoja — ukazuje im, kde a ako môžu vytvoriť hodnotu práve teraz.
