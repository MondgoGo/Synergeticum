# 🧾 ADR-019: Opportunity Engine (v2 – P2P & Decentralized Matching)

> **Zmeny oproti Version 1 (pôvodný ADR-019):**
> - **Doplnená časť: P2P signály a decentralizované párovanie**
> - **Doplnená časť: Need ↔ Capability matching**
> - **Doplnená časť: Prepojenie na Participation Filter (ADR-015/033)**
> - **Doplnená časť: Prepojenie na Reciprocity (ADR-036)**
> - **Rozšírená časť: Vstupy do engine o Capability Model z ADR-034**
> - **Rozšírená časť: Architektonické implikácie o P2P vrstvu**
> - **Rozšírená časť: Väzby na ADR**

---

## 1. 📌 Status

**Accepted (v2 – P2P & Decentralized Matching)**

---

## 2. 🎯 Kontext

Synergetikum už definuje:

* otázky a perspektívy (ADR-009, ADR-010)
* personalizáciu (ADR-011)
* vznik tém a projektov (ADR-012)
* participáciu (ADR-013)
* spoluprácu a role/needs (ADR-017)
* simuláciu scenárov (ADR-018)

Chýba vrstva, ktorá z týchto informácií **proaktívne identifikuje konkrétne príležitosti zapojenia** pre používateľov.

> Ako systém zistí, kto je vhodný ako realizátor, investor, organizátor alebo analytik – a kedy mu to ponúkne?

### Dodatočný problém (v2)

Pôvodný ADR-019 predpokladá, že Opportunity Engine má prístup k:
* profilom všetkých používateľov
* všetkým projektom a ich potrebám

To v decentralizovanom prostredí **nie je možné** – nikde neexistuje centrálny index ľudí a potrieb.

> Ako má Opportunity Engine fungovať v P2P sieti, kde:
> * nikto nemá kompletný prehľad o všetkých používateľoch
> * profily sú lokálne a súkromné
> * neexistuje centrálny vyhľadávací index?

👉 Systém potrebuje **decentralizovaný P2P mechanizmus** pre párovanie potrieb a schopností.

---

## 3. ⚖️ Rozhodnutie

> Synergetikum obsahuje **decentralizovaný Opportunity Engine**, ktorý kontinuálne generuje, hodnotí a ponúka **Opportunity** objekty (príležitosti) spájajúce: používateľa × rolu × projekt × moment – **bez centrálneho indexu, výlučne cez P2P signály**.

Opportunity Engine je nadstavba nad Matching (ADR-017):

* Matching hovorí „kto by sa hodil“
* Opportunity Engine hovorí „**toto je teraz pre teba konkrétna príležitosť**"

**V2 rozšírenie:** Engine funguje **decentralizovane** – žiadny centrálny server nezbiera profily ani potreby. Všetky matchy vznikajú cez **anonymizované P2P signály**.

---

## 4. 🧠 Core princíp

> Správna príležitosť je časovo citlivé spojenie schopnosti, záujmu a potreby. V P2P sieti sa príležitosti neposielajú do centra – šíria sa ako signály medzi relevantnými uzlami.

---

## 5. 📋 Definícia Opportunity

Opportunity je explicitný objekt s atribútmi:

| Atribút | Popis |
|---------|-------|
| `OpportunityId` | Unikátny identifikátor |
| `ProjectId` | Projekt, ktorý potrebuje výpomoc |
| `RoleId` (Need) | Aká rola je potrebná |
| `UserId` (target) | Komu je príležitosť určená |
| `FitScore` | Ako dobre sedí (0-1) |
| `Urgency` | Naliehavosť (0-1) |
| `Impact` | Potenciálny dopad (0-1) |
| `Confidence` | Istota matchu (0-1) |
| `CreatedAt`, `ExpiresAt` | Časová platnosť |
| `Reasoning` | Prečo práve tento používateľ |
| `RequiredActions` | Čo presne treba spraviť |
| `Source` | Ako bol match nájdený (P2P signal / direct / circle) |

---

## 6. 👥 Typy rolí (Role taxonomy)

Minimálny set pre MVP:

| Rola | Popis |
|------|-------|
| **Executor (Realizátor)** | vykoná konkrétnu úlohu |
| **Organizer (Organizátor)** | koordinuje ľudí/čas |
| **Analyst (Analytik)** | spracuje dáta/varianty |
| **Investor (Finančný/zdrojový)** | poskytne zdroje (peniaze, materiál, priestor) |

Rozšíriteľné:

* Designer, Legal, Marketing, Community, Ops, atď.

---

## 7. 📥 Vstupy do engine

Opportunity Engine využíva:

### 7.1 Personal AI profil (ADR-034)

| Komponent | Popis |
|-----------|-------|
| **Capability Model** | Čo používateľ vie robiť (skills, skúsenosti) |
| **Interest Model** | Čo používateľa zaujíma (topics, oblasti) |
| **Participation Profile** | Ako sa používateľ typicky zapája |
| **Reliability Score** | Spoľahlivosť z minulých akcií |
| **Preference Model** | Aké typy príležitostí preferuje |

### 7.2 Project/Topic dáta

| Komponent | Popis |
|-----------|-------|
| **Needs** | Aké role projekt potrebuje |
| **Project state** | Stav projektu (ideation, active, etc.) |
| **Branch priorities** | Ktoré vetvy sú prioritné |
| **Argument dependencies** | Aké argumenty čakajú na rozhodnutie |

### 7.3 Kontext

| Komponent | Popis |
|-----------|-------|
| **Location** | Geografická blízkosť (ak je relevantná) |
| **Time availability** | Kedy má používateľ čas |
| **Current load** | Koľko aktívnych commitmentov už má |

### 7.4 História interakcií

* Čo používateľ prijal/odmietol v minulosti
* Ako dopadli jeho predchádzajúce commitmenty

---

## 8. 🔄 P2P signály a decentralizované párovanie (NOVÉ v v2)

### 8.1 Princíp

> **Žiadny centrálny index. Matchy vznikajú šírením anonymizovaných signálov medzi relevantnými uzlami.**

Systém nepoužíva centrálnu databázu „kto čo vie“. Namiesto toho:

1. **Dopytové signály (Need Broadcast)** – projekt alebo topic vyšle do siete anonymizovaný signál: „Hľadám niekoho s capability X pre rolu Y“.
2. **Ponukové signály (Capability Announcement)** – Personal AI používateľa môže (s jeho súhlasom) vysielať anonymizované signály: „Mám capability X, záujem o oblasti Z“.
3. **Matching v sieti** – Keď sa dopytový a ponukový signál stretnú na nejakom uzle (alebo cez relay), vznikne kandidátna príležitosť.

### 8.2 Typy P2P signálov

| Typ signálu | Smer | Obsah | Anonymita |
|-------------|------|-------|-----------|
| **Need Broadcast** | Projekt → sieť | `{role, required_capabilities, urgency, project_topic}` | Projekt je identifikovateľný (inak by sa nedalo prijať) |
| **Capability Announcement** | Používateľ → sieť | `{capability_vector, interest_topics, availability}` | Plne anonymný (žiadne ID) |
| **Interest Expression** | Používateľ → sieť | `{topic_id, intensity}` | Plne anonymný |
| **Match Proposal** | Uzol → používateľ | `{opportunity_id, fit_score, reasoning}` | Len pre konkrétneho používateľa |

### 8.3 Ako signály putujú sieťou

```
Projekt A (potrebuje analytika)
    │
    ▼
[Need Broadcast] → TTL=3
    │
    ├──→ Uzol X (relay) → ďalej
    │
    └──→ Uzol Y (Personal AI používateľa)
              │
              ▼
         Porovná s lokálnym Capability Modelom
              │
              ▼
         Ak match → vytvorí Opportunity (lokálne)
```

### 8.4 Ochrana súkromia v P2P signáloch

| Mechanizmus | Popis |
|-------------|-------|
| **Anonymizácia** | Capability announcements neobsahujú ID používateľa |
| **Lokálny match** | Rozhodnutie o matchi prebieha na zariadení používateľa |
| **Žiadne centrálne logy** | Nikto nevidí, kto si s kým vymenil signály |
| **Opt-in** | Používateľ musí explicitne povoliť vysielanie capability signálov |
| **TTL (Time to Live)** | Signály majú obmedzený dosah (napr. 3 skoky) |

### 8.5 Prepojenie na ADR-014 (Network Communication Model)

Opportunity Engine využíva P2P vrstvu definovanú v ADR-014:

* Signály sa šíria cez existujúcu peer-to-peer sieť
* Žiadne špeciálne „centrálne uzly“ pre matching
* Relaying môžu robiť bežné uzly (voliteľne, parent nodes môžu pomáhať)

### 8.6 Prepojenie na ADR-015/033 (Participation Filter)

Používateľ dostane **len relevantné príležitosti**:

* Participation filter (ADR-015) rozhoduje, ktoré projekty/témy sú pre používateľa relevantné
* Signály z nerelevantných projektov sa ani neporovnávajú s Capability Modelom
* Používateľ môže nastaviť, aké typy príležitostí chce dostávať

```json
{
  "user_id": "user_miro_001",
  "opportunity_preferences": {
    "enabled": true,
    "max_per_day": 3,
    "preferred_roles": ["analyst", "organizer"],
    "blocked_roles": ["investor"],
    "min_fit_score": 0.6,
    "only_from_circles": true,  // Iba z trusted circles
    "allow_public_broadcasts": false
  }
}
```

### 8.7 Prepojenie na ADR-036 (Reciprocity)

Participácia cez Opportunity Engine generuje **reciprocitu** (ADR-036):

* Kto prijíma príležitosti a pomáha, dostáva neskôr kvalitnejšie príležitosti
* Kto odmieta alebo ignoruje, jeho signály majú nižšiu prioritu
* Nie je to penalizácia – je to prirodzené: systém sa učí, komu stojí za to posielať príležitosti

```json
// Reciprocita ovplyvňuje šírenie signálov
{
  "user_id": "user_miro_001",
  "reciprocity_score": 0.75,
  "signal_priority": 0.8,  // Jeho capability announcements majú vyššiu prioritu
  "incoming_opportunities_rate": "normal"  // Dostáva primerané množstvo
}
```

---

## 9. 📊 Scoring model

Každá príležitosť má kompozitné skóre:

### 9.1 FitScore

```
FitScore = f(
  capability_match,      // z ADR-034 Capability Model
  interest_alignment,    // z ADR-034 Interest Model
  participation_readiness, // z ADR-034 Participation Profile
  reliability,           // z histórie
  context_fit            // lokalita, čas, atď.
)
```

### 9.2 PriorityScore

```
PriorityScore = f(
  FitScore,
  Urgency,    // ako veľmi projekt potrebuje túto rolu teraz
  Impact,     // aký veľký posun spôsobí obsadenie roly
  Confidence  // ako istý je tento match
)
```

Kľúč:

* **FitScore** = „či je to pre teba vhodné“
* **PriorityScore** = „či to máš vidieť teraz“

---

## 10. ⏰ Urgency & Impact

| Atribút | Popis | Príklad |
|---------|-------|---------|
| **Urgency** | ako veľmi projekt potrebuje túto rolu teraz | "Potrebujeme organizátora na zajtrajšie stretnutie" → vysoká urgency |
| **Impact** | aký veľký posun spôsobí obsadenie roly | "Potrebujeme analýzu trhu" → stredná urgency, vysoký impact |

---

## 11. 🔄 Opportunity lifecycle

```
1. Detection
   └── Projekt vytvorí Need (rola) ALEBO
   └── Používateľova AI zistí príležitosť z P2P signálu

2. Generation
   └── Vytvoria sa kandidátne Opportunities pre vhodných používateľov
   └── (lokálne, na základe prijatých signálov)

3. Scoring & Ranking
   └── Výpočet PriorityScore (lokálne)

4. Delivery
   └── Zobrazenie používateľovi

5. User Action
   └── accept / decline / snooze / ignore

6. Outcome Tracking
   └── Či bola rola naplnená a s akým výsledkom

7. Learning
   └── Aktualizácia profilov, váh a reciprocity
```

---

## 12. 🎨 Delivery (UX)

### 12.1 Princípy

* nízky počet (1–3 príležitosti naraz)
* konkrétnosť (čo, prečo, koľko času)
* okamžitá akcia
* **transparentný pôvod** (či to prišlo z P2P signálu alebo z trusted circle)

### 12.2 Formát

```
┌─────────────────────────────────────────────────┐
│  🎯 Príležitosť pre teba                        │
│                                                 │
│  Projekt: Komunitná pizzéria                    │
│  Rola: Finančný analytik                        │
│  Prečo ty: Máš skúsenosti s finance (8/10)     │
│  Čas: 2-3 hodiny, tento týždeň                 │
│  Zdroj: Trusted circle "Pizzériová parta"      │
│                                                 │
│  [Prijmi]  [Odmietni]  [Odlož]  [Detail]      │
└─────────────────────────────────────────────────┘
```

### 12.3 Akcie

| Akcia | Význam | Dopad |
|-------|--------|-------|
| **Accept** | Záväzok splniť | Zvyšuje reliability, generuje reciprocitu |
| **Decline** | Nie je záujem | Znižuje frekvenciu podobných ponúk |
| **Snooze** | Neskôr, teraz nie | Odloží na neskôr, bez penalizácie |
| **Details** | Viac info + scenáre (ADR-018) | Žiadny dopad na skóre |
| **Ignore** | Bez odozvy | Postupné znižovanie priority |

---

## 13. 🛡️ Anti-overload mechanizmy

| Mechanizmus | Popis |
|-------------|-------|
| **Rate limiting** | Max N príležitostí/deň (nastaviteľné) |
| **Cooldown** | Po odmietnutí podobnej príležitosti nasleduje pauza |
| **Diversity** | Rôzne projekty, rôzne role |
| **Confidence gating** | Len dostatočne isté matchy (min FitScore) |
| **User throttling** | Používateľ môže znížiť frekvenciu |

---

## 14. 🚫 Anti-manipulation pravidlá

| Pravidlo | Popis |
|----------|-------|
| **No dark patterns** | Nevnucovať, žiadne opt-out pasce |
| **Transparent reasoning** | Každá príležitosť má „prečo práve ty“ |
| **Opt-out per role/project** | Používateľ môže vypnúť celé kategórie |
| **No fake urgency** | Urgency musí byť odvodená z reálnych dát projektu |
| **No central priority boosting** | Nikto nemôže zaplatiť za vyššiu prioritu signálov |

---

## 15. 📖 Explainability

Každá príležitosť obsahuje „Prečo“:

```
Prečo práve ty?
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✓ Máš skúsenosti s finance (8/10 z tvojho profilu)
✓ Zaujímal si sa o projekt "Komunitná pizzéria"
✓ Projekt je v tvojej lokalite (Trnava)
✓ V poslednom čase si prijal 3 podobné príležitosti
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Zdroj: Trusted circle "Pizzériová parta"
```

---

## 16. 🔗 Vzťah k participácii (ADR-012/ADR-033)

| Akcia | Dopad na Participation Level |
|-------|------------------------------|
| Accept + successful delivery | Zvyšuje ParticipationLevel |
| Consistent delivery | Zvyšuje ReliabilityScore |
| No-show / nedokončenie | Znižuje ReliabilityScore |
| Decline | Žiadny negatívny dopad (len informácia pre learning) |

---

## 17. 📈 Vzťah k projektom (ADR-022)

* Rýchlosť obsadzovania rolí ovplyvňuje **Project health**
* Neobsadené kritické role → riziko stagnácie
* Projekty s dobrou „match rate“ majú vyššiu prioritu pri šírení signálov

---

## 18. 🏗️ Architektonické implikácie

### 18.1 Entities

| Entita | Popis |
|--------|-------|
| `Opportunity` | Príležitosť pre používateľa |
| `Need` (Role) | Potreba projektu |
| `UserCapability` | Schopnosti používateľa (ADR-034) |
| `ReliabilityScore` | Spoľahlivosť z histórie |
| `P2PSignal` | Anonymizovaný signál v sieti |
| `ReciprocityRecord` | História prijatých/odmietnutých príležitostí |

### 18.2 Services

| Service | Popis |
|---------|-------|
| `OpportunityGenerationService` | Vytvára Opportunities z P2P signálov |
| `OpportunityScoringService` | Počíta FitScore a PriorityScore |
| `OpportunityDeliveryService` | Zobrazuje používateľovi |
| `OutcomeTrackingService` | Sleduje výsledky |
| `P2PSignalService` | Vysiela a prijíma anonymizované signály |
| `ReciprocityService` | Spravuje reciprocitu (ADR-036) |

### 18.3 Storage

| Storage | Popis |
|---------|-------|
| `Opportunity store` | Krátkodobý, TTL (napr. 7 dní) |
| `History store` | Pre learning (anonymizovaný) |
| `Signal store` | Dočasné ukladanie P2P signálov |

### 18.4 P2P vrstva (NOVÉ v v2)

Opportunity Engine využíva P2P vrstvu z ADR-014:

* `BroadcastNeed(need, ttl)` – vyšle dopytový signál
* `AnnounceCapability(capability, ttl)` – vyšle ponukový signál (opt-in)
* `OnSignalReceived(signal, sender)` – spracovanie prichádzajúceho signálu
* `MatchLocally(signal, userProfile)` – lokálne porovnanie

---

## 19. ⚠️ Riziká a mitigácie

| Riziko | Popis | Mitigácia |
|--------|-------|-----------|
| **Overmatching** | príliš veľa návrhov → únava | Rate limiting, cooldown, user throttling |
| **Undermatching** | projekty bez ľudí → stagnácia | Šírenie signálov, parent node relay |
| **Skill misclassification** | nesprávny odhad schopností | Behavior-based learning, gradual trust |
| **Gaming the system** | falošný commitment | Reliability scoring, outcome tracking |
| **Signal spam** | príliš veľa signálov v sieti | TTL, rate limiting na úrovni siete |
| **Privacy leak** | signály môžu odhaliť informácie | Anonymizácia, lokálny match, žiadne centrálne logy |

---

## 20. 🚫 Alternatívy (zamietnuté)

| Alternatíva | Dôvod zamietnutia |
|-------------|-------------------|
| **Manuálne hľadanie** | neškáluje, pasívne |
| **Broadcast „kto chce?“** | nízka kvalita a šum |
| **Centrálne priraďovanie** | porušuje decentralizáciu (ADR-001, ADR-004) |
| **Iba trusted circle matching** | príliš obmedzené, chýba objavovanie |

---

## 21. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-001 | Ownership – používateľ vlastní svoje capability signály |
| ADR-004 | Privacy-first – signály sú anonymizované, žiadne centrálne logy |
| ADR-011 | Personalization – preferencie príležitostí sú súčasťou Preference Modelu |
| ADR-012 | Engagement – prijatie príležitosti zvyšuje engagement |
| ADR-013 | Participation – vstup pre Participation Profile |
| ADR-014 | **Network Communication – P2P vrstva pre signály** |
| ADR-015/033 | **Participation Filter – filtrovanie relevantných príležitostí** |
| ADR-017 | Collaboration Model – nadstavba nad matching |
| ADR-018 | Simulation Engine – detailné scenáre pre príležitosti |
| ADR-022 | Project Formation – potreby projektov sú vstupom |
| ADR-025 | Argumentation – argumenty môžu generovať potreby |
| ADR-028 | Support & Alignment – prijatie príležitosti je signál commitmentu |
| ADR-034 | **Personal AI Internal Model – Capability Model, Interest Model** |
| ADR-036 | **Reciprocity – participácia generuje reciprocitu** |
| ADR-038 | Power & Influence – opportunity engine nesmie zvýhodňovať silných |
| ADR-040 | Graceful Degradation – nízka kvalita vstupov = horšie matchy |

---

## 22. 📏 Pravidlá implementácie (high-level)

Systém musí:

* umožniť projektom definovať potreby (role, required capabilities, urgency)
* umožniť používateľom (opt-in) vysielať anonymizované capability signály
* šíriť signály P2P sieťou s obmedzeným TTL
* **lokálne** porovnávať signály s profilom používateľa
* generovať Opportunities len pre relevantné matchy
* poskytnúť explainability (prečo práve táto príležitosť)
* rešpektovať user preferences (max za deň, preferované role, blokované role)
* sledovať outcomes a aktualizovať reliability a reciprocitu
* **nikdy neposielať surové profily do siete** (len anonymizované signály)

---

## 23. 🔥 Príklady

### Príklad 1: Projekt vyšle dopytový signál

```json
{
  "signal_type": "need_broadcast",
  "signal_id": "sig_001",
  "ttl": 3,
  "created_at": "2026-04-15T10:00:00Z",
  "content": {
    "project_id": "proj_pizzeria_001",
    "project_topic": "community_food",
    "role": "analyst",
    "required_capabilities": ["finance", "data_analysis"],
    "urgency": 0.8,
    "estimated_effort_hours": 3,
    "expires_at": "2026-04-22T10:00:00Z"
  }
}
```

### Príklad 2: Používateľova AI vyšle ponukový signál (opt-in)

```json
{
  "signal_type": "capability_announcement",
  "signal_id": "sig_002",
  "ttl": 2,
  "created_at": "2026-04-15T11:00:00Z",
  "content": {
    "capability_vector": {
      "finance": 0.85,
      "data_analysis": 0.70,
      "marketing": 0.30
    },
    "interest_topics": ["community_food", "sustainability"],
    "availability": {
      "hours_per_week": 5,
      "preferred_time": "evening"
    }
  },
  "anonymized": true  // Žiadne user ID
}
```

### Príklad 3: Lokálny match a vytvorenie Opportunity

```json
{
  "opportunity_id": "opp_001",
  "project_id": "proj_pizzeria_001",
  "role": "analyst",
  "user_id": "user_miro_001",
  "fit_score": 0.82,
  "urgency": 0.8,
  "impact": 0.7,
  "confidence": 0.75,
  "priority_score": 0.78,
  "created_at": "2026-04-15T11:05:00Z",
  "expires_at": "2026-04-22T10:00:00Z",
  "reasoning": [
    "Tvoj Capability Model má finance=0.85, data_analysis=0.70",
    "Projekt je v tvojej interest oblasti community_food",
    "Odhadovaný čas: 3 hodiny"
  ],
  "source": "p2p_signal",
  "required_actions": [
    "Spojiť sa s organizátorom projektu",
    "Analyzovať finančný plán"
  ]
}
```

### Príklad 4: Používateľove nastavenia

```json
{
  "user_id": "user_miro_001",
  "opportunity_preferences": {
    "enabled": true,
    "max_per_day": 2,
    "cooldown_minutes": 60,
    "preferred_roles": ["analyst", "executor"],
    "blocked_roles": ["investor"],
    "min_fit_score": 0.6,
    "only_from_circles": false,
    "allow_capability_broadcast": true,
    "signal_ttl": 2,
    "quiet_hours": {
      "enabled": true,
      "start": "21:00",
      "end": "08:00"
    }
  }
}
```

### Príklad 5: Reciprocita ovplyvňujúca priority

```json
{
  "user_id": "user_miro_001",
  "reciprocity": {
    "score": 0.75,
    "accepted_opportunities": 12,
    "successful_completions": 10,
    "declined_opportunities": 5,
    "ignored_opportunities": 2,
    "signal_priority_multiplier": 1.2,  // Jeho signály majú vyššiu váhu
    "incoming_priority_multiplier": 1.1  // Dostáva kvalitnejšie príležitosti
  }
}
```

---

## 24. 🔥 Zhrnutie

> **Opportunity Engine premieňa potenciál spolupráce na konkrétne, časovo relevantné príležitosti pre jednotlivca – bez centrálneho indexu, výlučne cez anonymizované P2P signály.**

**V2 prináša:**

* **P2P signály** – namiesto centrálneho indexu sa dopyt a ponuka šíria sieťou
* **Need ↔ Capability matching** – prepojenie medzi potrebami projektu a schopnosťami používateľa (ADR-034)
* **Prepojenie na Participation Filter** – používateľ dostane len relevantné príležitosti
* **Prepojenie na Reciprocity** – participácia generuje reciprocitu (ADR-036)
* **Decentralizácia** – žiadny centrálny server, žiadne centrálne logy

---

## 25. 🧭 Finálna veta

> **Synergetikum nečaká, kým sa ľudia zapoja — ukazuje im, kde a ako môžu vytvoriť hodnotu práve teraz. A robí to spôsobom, ktorý rešpektuje súkromie, decentralizáciu a autonómiu každého používateľa.**

V P2P prostredí nie je možné mať centrálny zoznam „kto čo vie“. Namiesto toho sa príležitosti šíria ako signály – a každý používateľ sa sám rozhodne, či je daná príležitosť pre neho.

---

**Dátum:** 2026-04-15  
**Stav:** Accepted  
**Typ:** Core Mechanism ADR (Opportunity Engine)  
**Verzia:** 2.0 (P2P & Decentralized Matching)

---

## 📝 Zoznam zmien oproti Version 1

| Sekcia | Zmena |
|--------|-------|
| 1. Status | Zmenené na v2 |
| 2. Kontext | Doplnený dodatočný problém (decentralizácia, P2P) |
| 3. Rozhodnutie | Doplnené „bez centrálneho indexu, výlučne cez P2P signály“ |
| 4. Core princíp | Doplnený o P2P |
| 5. Definícia | Doplnený `Source` atribút |
| 7.1 | Doplnený Capability Model a Interest Model z ADR-034 |
| 8. | **Nová sekcia:** P2P signály a decentralizované párovanie |
| 8.6 | **Nová sekcia:** Prepojenie na Participation Filter (ADR-015/033) |
| 8.7 | **Nová sekcia:** Prepojenie na Reciprocity (ADR-036) |
| 12.2 | Doplnený `Zdroj` do formátu notifikácie |
| 16. | Rozšírené o prepojenie na ADR-033 |
| 18.1 | Doplnené `P2PSignal` a `ReciprocityRecord` |
| 18.2 | Doplnené `P2PSignalService` a `ReciprocityService` |
| 18.4 | **Nová sekcia:** P2P vrstva |
| 19. | Doplnené riziká Signal spam a Privacy leak |
| 20. | Doplnená alternatíva „Iba trusted circle matching“ |
| 21. Väzby | Doplnené ADR-014, ADR-015/033, ADR-034, ADR-036 |
| 22. Implementácia | Doplnené pravidlá pre P2P signály a anonymizáciu |
| 23. | **Nové príklady:** P2P signály, lokálny match, reciprocita |
| 24. Zhrnutie | Doplnené v2 princípy |
| 25. Finálna veta | Rozšírená o P2P a súkromie |