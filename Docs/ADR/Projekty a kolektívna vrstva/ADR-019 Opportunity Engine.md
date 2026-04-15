# 🧾 ADR-019: Opportunity Engine (v3 – with Calendar & Event Integration)

> **Zmeny oproti Version 2 (v2 – P2P & Decentralized Matching):**
> - **Doplnená časť 8.8:** Eventy ako príležitosti (prepojenie na INSPIRATION-11 a ADR-045)
> - **Doplnená časť 8.9:** Typy eventových príležitostí
> - **Doplnená časť 8.10:** Prepojenie eventových príležitostí na P2P signály
> - **Doplnená časť 8.11:** Time-aware matching – zohľadnenie časovej dostupnosti
> - **Rozšírená časť 7.3:** Kontext o kalendár a dostupnosť
> - **Rozšírená časť 21:** Väzby na ADR-045
> - **Nová sekcia 23.6 – 23.8:** Príklady pre event-based opportunities

---

## 1. 📌 Status

**Accepted (v3 – with Calendar & Event Integration)**

---

## 2. 🎯 Kontext

Synergetikum už definuje:

* otázky a perspektívy (ADR-009, ADR-010)
* personalizáciu (ADR-011)
* vznik tém a projektov (ADR-012)
* participáciu (ADR-013)
* spoluprácu a role/needs (ADR-017)
* simuláciu scenárov (ADR-018)
* **kalendár a eventy (ADR-045)**

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

### Dodatočný problém (v3)

Aj pri P2P matchingu chýba prepojenie na:

* **eventy a kalendár** – stretnutia, workshopy, deadliny
* **časovú dostupnosť** – kedy má používateľ reálne čas

> Ako môže byť event (stretnutie, workshop, deadline) sám o sebe príležitosťou na zapojenie?

👉 Systém potrebuje **prepojenie medzi Opportunity Engine a kalendárom (ADR-045)**.

---

## 3. ⚖️ Rozhodnutie

> Synergetikum obsahuje **decentralizovaný Opportunity Engine**, ktorý kontinuálne generuje, hodnotí a ponúka **Opportunity** objekty (príležitosti) spájajúce: používateľa × rolu × projekt × moment – **bez centrálneho indexu, výlučne cez P2P signály**.

Opportunity Engine je nadstavba nad Matching (ADR-017):

* Matching hovorí „kto by sa hodil“
* Opportunity Engine hovorí „**toto je teraz pre teba konkrétna príležitosť**"

**V2 rozšírenie:** Engine funguje **decentralizovane** – žiadny centrálny server nezbiera profily ani potreby. Všetky matchy vznikajú cez **anonymizované P2P signály**.

**V3 rozšírenie:** Engine je **prepojený s kalendárom (ADR-045)** – eventy (stretnutia, workshopy, deadliny) sú samostatným zdrojom príležitostí.

---

## 4. 🧠 Core princíp

> Správna príležitosť je časovo citlivé spojenie schopnosti, záujmu a potreby. V P2P sieti sa príležitosti neposielajú do centra – šíria sa ako signály medzi relevantnými uzlami. **Eventy sú prirodzeným zdrojom príležitostí – stretnutie je príležitosť na participáciu, workshop na učenie, deadline na rozhodnutie.**

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
| `Source` | Ako bol match nájdený (P2P signal / direct / circle / **event** - v3) |
| `EventId` (v3) | Ak je príležitosť založená na evente, odkaz na event |

---

## 6. 👥 Typy rolí (Role taxonomy)

Minimálny set pre MVP:

| Rola | Popis |
|------|-------|
| **Executor (Realizátor)** | vykoná konkrétnu úlohu |
| **Organizer (Organizátor)** | koordinuje ľudí/čas |
| **Analyst (Analytik)** | spracuje dáta/varianty |
| **Investor (Finančný/zdrojový)** | poskytne zdroje (peniaze, materiál, priestor) |
| **Participant (Účastník eventu)** (v3) | zúčastní sa stretnutia/workshopu |
| **Facilitator (Facilitátor)** (v3) | vedie stretnutie/workshop |

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
| **Events (v3)** | Aké eventy projekt organizuje |

### 7.3 Kontext (Rozšírené v v3)

| Komponent | Popis |
|-----------|-------|
| **Location** | Geografická blízkosť (ak je relevantná) |
| **Time availability** | Kedy má používateľ čas |
| **Current load** | Koľko aktívnych commitmentov už má |
| **Calendar (v3)** | Aké eventy má používateľ už v kalendári |
| **Time conflicts (v3)** | Či je používateľ v čase eventu voľný |

### 7.4 História interakcií

* Čo používateľ prijal/odmietol v minulosti
* Ako dopadli jeho predchádzajúce commitmenty
* **Akých eventov sa zúčastnil (v3)**

---

## 8. 🔄 P2P signály a decentralizované párovanie

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
| **Event Broadcast (v3)** | Projekt → sieť | `{event_id, event_type, required_roles, start_time, location}` | Projekt je identifikovateľný |

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
    "preferred_roles": ["analyst", "organizer", "participant"],
    "blocked_roles": ["investor"],
    "min_fit_score": 0.6,
    "only_from_circles": true,
    "allow_public_broadcasts": false,
    "event_opportunities": true,  // NOVÉ v v3
    "min_advance_notice_hours": 24  // NOVÉ v v3
  }
}
```

### 8.7 Prepojenie na ADR-036 (Reciprocity)

Participácia cez Opportunity Engine generuje **reciprocitu** (ADR-036):

* Kto prijíma príležitosti a pomáha, dostáva neskôr kvalitnejšie príležitosti
* Kto odmieta alebo ignoruje, jeho signály majú nižšiu prioritu
* Nie je to penalizácia – je to prirodzené: systém sa učí, komu stojí za to posielať príležitosti
* **Účasť na evente generuje reciprocitu (v3)**

```json
// Reciprocita ovplyvňuje šírenie signálov
{
  "user_id": "user_miro_001",
  "reciprocity_score": 0.75,
  "signal_priority": 0.8,
  "incoming_opportunities_rate": "normal",
  "event_participation_count": 5  // NOVÉ v v3
}
```

---

### 8.8 Eventy ako príležitosti (NOVÉ v v3)

**Princíp:**

> **Event (ADR-045) nie je len položka v kalendári. Je to príležitosť na zapojenie – buď ako účastník, alebo ako organizátor/facilitátor.**

Event (ADR-045) môže byť zdrojom Opportunity. Keď projekt vytvorí event, Opportunity Engine automaticky:

1. Extrahuje z eventu potenciálne príležitosti
2. Porovná ich s profilmi používateľov (lokálne)
3. Vytvorí Opportunities pre relevantných používateľov

### 8.9 Typy eventových príležitostí (NOVÉ v v3)

| Typ eventu | Generovaná príležitosť | Rola | Typická urgency |
|------------|----------------------|------|-----------------|
| **Stretnutie** | "Projekt X organizuje stretnutie – môžeš sa zúčastniť ako participant" | Participant | Stredná |
| **Workshop** | "Workshop Y potrebuje facilitátora – tvoje schopnosti (8/10)" | Facilitator | Vysoká |
| **Deadline** | "Deadline pre rozhodnutie Z sa blíži – tvoj input je potrebný" | Decision maker | Vysoká |
| **Brigáda** | "Brigáda v projekte W potrebuje ľudí – tvoja lokalita je vhodná" | Executor | Stredná |
| **Diskusia** | "Diskusia k argumentu A – tvoj názor je cenný" | Contributor | Nízka |
| **Kickoff** | "Kickoff projektu P – pozvánka pre nových členov" | Observer/Participant | Nízka |

### 8.10 Prepojenie eventových príležitostí na P2P signály (NOVÉ v v3)

Eventy sa šíria ako špecializované dopytové signály:

```json
{
  "signal_type": "event_broadcast",
  "signal_id": "sig_event_001",
  "ttl": 3,
  "created_at": "2026-04-15T10:00:00Z",
  "content": {
    "event_id": "event_kickoff_001",
    "event_type": "meeting",
    "project_id": "proj_pizzeria_001",
    "start_time": "2026-05-01T16:00:00Z",
    "end_time": "2026-05-01T18:00:00Z",
    "location": {
      "type": "hybrid",
      "physical": "Komunitné centrum, Trnava",
      "online": "https://meet.example.com/pizzeria"
    },
    "required_roles": ["participant"],
    "preferred_capabilities": ["community_building"],
    "urgency": 0.6,
    "max_participants": 20
  }
}
```

Používateľova AI:

1. Prijme event broadcast
2. Porovná s Interest Modelom (zaujíma ho téma?)
3. Porovná s kalendárom (je voľný v danom čase?)
4. Porovná s preferenciami (chce dostávať eventové príležitosti?)
5. Ak match → vytvorí Opportunity

### 8.11 Time-aware matching (NOVÉ v v3)

Opportunity Engine zohľadňuje **časovú dostupnosť** používateľa:

| Faktor | Vplyv na FitScore |
|--------|-------------------|
| **Časová kolízia** | Ak je používateľ v čase eventu zaneprázdnený, FitScore = 0 |
| **Časová preferencia** | Ak event mimo preferovaného času, zníženie FitScore |
| **Doba predstihu** | Ak je event príliš skoro (<24h), zníženie (používateľ nemusí stihnúť) |
| **Dĺžka eventu** | Dlhé eventy vyžadujú vyšší commitment |

```json
{
  "user_id": "user_miro_001",
  "calendar": {
    "busy_slots": [
      {"start": "2026-05-01T14:00:00Z", "end": "2026-05-01T15:30:00Z"}
    ],
    "preferred_times": ["evening", "weekend"],
    "min_advance_notice_hours": 24
  },
  "event": {
    "start_time": "2026-05-01T16:00:00Z",
    "end_time": "2026-05-01T18:00:00Z",
    "advance_notice_hours": 360  // 15 dní vopred
  },
  "time_fit": {
    "conflict": false,
    "preferred_time_match": 0.7,  // večer je preferovaný
    "advance_notice_match": 1.0,   // dostatočne vopred
    "overall_time_score": 0.85
  }
}
```

---

## 9. 📊 Scoring model

Každá príležitosť má kompozitné skóre:

### 9.1 FitScore (Rozšírené v v3)

```
FitScore = f(
  capability_match,      // z ADR-034 Capability Model
  interest_alignment,    // z ADR-034 Interest Model
  participation_readiness, // z ADR-034 Participation Profile
  reliability,           // z histórie
  context_fit,           // lokalita, čas, atď.
  time_fit               // časová dostupnosť (NOVÉ v v3)
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

## 10. ⏰ Urgency & Impact (Rozšírené v v3)

| Atribút | Popis | Príklad |
|---------|-------|---------|
| **Urgency** | ako veľmi projekt potrebuje túto rolu teraz | "Potrebujeme organizátora na zajtrajšie stretnutie" → vysoká urgency |
| **Impact** | aký veľký posun spôsobí obsadenie roly | "Potrebujeme analýzu trhu" → stredná urgency, vysoký impact |
| **Event proximity (v3)** | ako skoro sa event koná | "Stretnutie zajtra" → vysoká urgency |

---

## 11. 🔄 Opportunity lifecycle (Rozšírené v v3)

```
1. Detection
   └── Projekt vytvorí Need (rola) ALEBO
   └── Projekt vytvorí Event (v3) ALEBO
   └── Používateľova AI zistí príležitosť z P2P signálu

2. Generation
   └── Vytvoria sa kandidátne Opportunities pre vhodných používateľov
   └── (lokálne, na základe prijatých signálov)

3. Scoring & Ranking
   └── Výpočet PriorityScore (lokálne)
   └── Zohľadnenie časovej dostupnosti (v3)

4. Delivery
   └── Zobrazenie používateľovi

5. User Action
   └── accept / decline / snooze / ignore

6. Outcome Tracking
   └── Či bola rola naplnená a s akým výsledkom
   └── Či sa používateľ zúčastnil eventu (v3)

7. Learning
   └── Aktualizácia profilov, váh a reciprocity
   └── Aktualizácia kalendára a preferencií (v3)
```

---

## 12. 🎨 Delivery (UX) (Rozšírené v v3)

### 12.1 Princípy

* nízky počet (1–3 príležitosti naraz)
* konkrétnosť (čo, prečo, koľko času)
* okamžitá akcia
* **transparentný pôvod** (či to prišlo z P2P signálu, z trusted circle alebo z eventu)

### 12.2 Formát (príklad pre event)

```
┌─────────────────────────────────────────────────┐
│  📅 Príležitosť pre teba (event)                │
│                                                 │
│  Event: Kickoff stretnutie - Komunitná pizzéria │
│  Dátum: 1.5.2026, 16:00-18:00                  │
│  Miesto: Komunitné centrum, Trnava              │
│                                                 │
│  Prečo ty: Zaujíma ťa community_food (8/10)    │
│  Časová kolízia: Žiadna (si voľný)             │
│  Zdroj: Trusted circle "Pizzériová parta"      │
│                                                 │
│  [Zúčastním sa]  [Nezaujíma]  [Odlož]  [Detail]│
└─────────────────────────────────────────────────┘
```

### 12.3 Akcie (Rozšírené v v3)

| Akcia | Význam | Dopad |
|-------|--------|-------|
| **Accept** | Záväzok splniť / zúčastniť sa | Zvyšuje reliability, generuje reciprocitu |
| **Decline** | Nie je záujem | Znižuje frekvenciu podobných ponúk |
| **Snooze** | Neskôr, teraz nie | Odloží na neskôr, bez penalizácie |
| **Details** | Viac info + scenáre (ADR-018) | Žiadny dopad na skóre |
| **Ignore** | Bez odozvy | Postupné znižovanie priority |
| **Add to calendar (v3)** | Pridať event do kalendára | Automatické potvrdenie dostupnosti |

---

## 13. 🛡️ Anti-overload mechanizmy

| Mechanizmus | Popis |
|-------------|-------|
| **Rate limiting** | Max N príležitostí/deň (nastaviteľné) |
| **Cooldown** | Po odmietnutí podobnej príležitosti nasleduje pauza |
| **Diversity** | Rôzne projekty, rôzne role, rôzne typy (aj eventy) |
| **Confidence gating** | Len dostatočne isté matchy (min FitScore) |
| **User throttling** | Používateľ môže znížiť frekvenciu |
| **Event spam protection (v3)** | Max M eventových príležitostí za deň |

---

## 14. 🚫 Anti-manipulation pravidlá

| Pravidlo | Popis |
|----------|-------|
| **No dark patterns** | Nevnucovať, žiadne opt-out pasce |
| **Transparent reasoning** | Každá príležitosť má „prečo práve ty“ |
| **Opt-out per role/project** | Používateľ môže vypnúť celé kategórie |
| **No fake urgency** | Urgency musí byť odvodená z reálnych dát projektu |
| **No central priority boosting** | Nikto nemôže zaplatiť za vyššiu prioritu signálov |
| **No fake event urgency (v3)** | Event urgency musí zodpovedať reálnemu dátumu |

---

## 15. 📖 Explainability (Rozšírené v v3)

Každá príležitosť obsahuje „Prečo“:

```
Prečo práve ty? (Event)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✓ Zaujímaš sa o tému "community_food"
✓ Si voľný v čase eventu (žiadna kolízia)
✓ Projekt je v tvojej lokalite (Trnava)
✓ V minulosti si sa zúčastnil 3 podobných eventov
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Zdroj: Trusted circle "Pizzériová parta"
Typ: Event (kickoff stretnutie)
```

---

## 16. 🔗 Vzťah k participácii (ADR-012/ADR-033) (Rozšírené v v3)

| Akcia | Dopad na Participation Level |
|-------|------------------------------|
| Accept + successful delivery | Zvyšuje ParticipationLevel |
| Consistent delivery | Zvyšuje ReliabilityScore |
| **Event attendance (v3)** | Zvyšuje ParticipationLevel |
| No-show / nedokončenie | Znižuje ReliabilityScore |
| Decline | Žiadny negatívny dopad (len informácia pre learning) |

---

## 17. 📈 Vzťah k projektom (ADR-022)

* Rýchlosť obsadzovania rolí ovplyvňuje **Project health**
* Neobsadené kritické role → riziko stagnácie
* Projekty s dobrou „match rate“ majú vyššiu prioritu pri šírení signálov
* **Eventy s nízkou účasťou** → signál pre zlepšenie (v3)

---

## 18. 🏗️ Architektonické implikácie

### 18.1 Entities (Rozšírené v v3)

| Entita | Popis |
|--------|-------|
| `Opportunity` | Príležitosť pre používateľa |
| `Need` (Role) | Potreba projektu |
| `UserCapability` | Schopnosti používateľa (ADR-034) |
| `ReliabilityScore` | Spoľahlivosť z histórie |
| `P2PSignal` | Anonymizovaný signál v sieti |
| `ReciprocityRecord` | História prijatých/odmietnutých príležitostí |
| `EventOpportunity` (v3) | Špeciálny typ príležitosti z eventu |
| `CalendarSlot` (v3) | Časová dostupnosť používateľa |

### 18.2 Services (Rozšírené v v3)

| Service | Popis |
|---------|-------|
| `OpportunityGenerationService` | Vytvára Opportunities z P2P signálov |
| `OpportunityScoringService` | Počíta FitScore a PriorityScore |
| `OpportunityDeliveryService` | Zobrazuje používateľovi |
| `OutcomeTrackingService` | Sleduje výsledky |
| `P2PSignalService` | Vysiela a prijíma anonymizované signály |
| `ReciprocityService` | Spravuje reciprocitu (ADR-036) |
| `EventOpportunityService` (v3) | Spracúva eventy na príležitosti |
| `TimeAwareMatchingService` (v3) | Zohľadňuje časovú dostupnosť |

### 18.3 Storage (Rozšírené v v3)

| Storage | Popis |
|---------|-------|
| `Opportunity store` | Krátkodobý, TTL (napr. 7 dní) |
| `History store` | Pre learning (anonymizovaný) |
| `Signal store` | Dočasné ukladanie P2P signálov |
| `Calendar store` (v3) | Lokálny kalendár používateľa |

### 18.4 P2P vrstva (Rozšírené v v3)

Opportunity Engine využíva P2P vrstvu z ADR-014:

* `BroadcastNeed(need, ttl)` – vyšle dopytový signál
* `AnnounceCapability(capability, ttl)` – vyšle ponukový signál (opt-in)
* `BroadcastEvent(event, ttl)` (v3) – vyšle event signál
* `OnSignalReceived(signal, sender)` – spracovanie prichádzajúceho signálu
* `MatchLocally(signal, userProfile)` – lokálne porovnanie

---

## 19. ⚠️ Riziká a mitigácie (Rozšírené v v3)

| Riziko | Popis | Mitigácia |
|--------|-------|-----------|
| **Overmatching** | príliš veľa návrhov → únava | Rate limiting, cooldown, user throttling |
| **Undermatching** | projekty bez ľudí → stagnácia | Šírenie signálov, parent node relay |
| **Skill misclassification** | nesprávny odhad schopností | Behavior-based learning, gradual trust |
| **Gaming the system** | falošný commitment | Reliability scoring, outcome tracking |
| **Signal spam** | príliš veľa signálov v sieti | TTL, rate limiting na úrovni siete |
| **Privacy leak** | signály môžu odhaliť informácie | Anonymizácia, lokálny match, žiadne centrálne logy |
| **Event spam (v3)** | príliš veľa eventových príležitostí | Rate limiting na eventy, min advance notice |
| **Time conflict (v3)** | navrhovanie eventov v čase, keď je používateľ busy | Kontrola kalendára pred matchom |

---

## 20. 🚫 Alternatívy (zamietnuté)

| Alternatíva | Dôvod zamietnutia |
|-------------|-------------------|
| **Manuálne hľadanie** | neškáluje, pasívne |
| **Broadcast „kto chce?“** | nízka kvalita a šum |
| **Centrálne priraďovanie** | porušuje decentralizáciu (ADR-001, ADR-004) |
| **Iba trusted circle matching** | príliš obmedzené, chýba objavovanie |
| **Ignorovanie eventov (v3)** | kalendár je pasívny, nie je zdrojom príležitostí |

---

## 21. 🔗 Väzby na ďalšie ADR (Rozšírené v v3)

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
| **ADR-045** | **Calendar & Events – eventy sú zdrojom príležitostí (v3)** |

---

## 22. 📏 Pravidlá implementácie (high-level) (Rozšírené v v3)

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
* **podporovať eventy ako zdroj príležitostí (v3)**
* **kontrolovať časovú dostupnosť pred vytvorením eventovej príležitosti (v3)**
* **rešpektovať min_advance_notice_hours (v3)**
* **umožniť používateľom pridávať eventy do kalendára pri accepte (v3)**

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
  "anonymized": true
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
    },
    "event_opportunities": true,  // NOVÉ v v3
    "min_advance_notice_hours": 24  // NOVÉ v v3
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
    "event_participation_count": 5,  // NOVÉ v v3
    "signal_priority_multiplier": 1.2,
    "incoming_priority_multiplier": 1.1
  }
}
```

### Príklad 6: Event broadcast (NOVÉ v v3)

```json
{
  "signal_type": "event_broadcast",
  "signal_id": "sig_event_001",
  "ttl": 3,
  "created_at": "2026-04-15T10:00:00Z",
  "content": {
    "event_id": "event_kickoff_001",
    "event_type": "meeting",
    "project_id": "proj_pizzeria_001",
    "project_topic": "community_food",
    "name": "Kickoff stretnutie - Komunitná pizzéria",
    "start_time": "2026-05-01T16:00:00Z",
    "end_time": "2026-05-01T18:00:00Z",
    "timezone": "Europe/Bratislava",
    "location": {
      "type": "hybrid",
      "physical": "Komunitné centrum, Trnava",
      "online": "https://meet.example.com/pizzeria"
    },
    "required_roles": ["participant"],
    "preferred_capabilities": ["community_building"],
    "urgency": 0.6,
    "max_participants": 20
  }
}
```

### Príklad 7: Event-based opportunity (NOVÉ v v3)

```json
{
  "opportunity_id": "opp_event_001",
  "event_id": "event_kickoff_001",
  "project_id": "proj_pizzeria_001",
  "role": "participant",
  "user_id": "user_miro_001",
  "fit_score": 0.85,
  "urgency": 0.6,
  "impact": 0.5,
  "confidence": 0.8,
  "priority_score": 0.68,
  "created_at": "2026-04-15T10:05:00Z",
  "expires_at": "2026-05-01T16:00:00Z",
  "reasoning": [
    "Zaujímaš sa o tému community_food (Interest=0.8)",
    "Si voľný v čase eventu (žiadna kolízia)",
    "Projekt je v tvojej lokalite (Trnava)",
    "V minulosti si sa zúčastnil 3 podobných eventov"
  ],
  "source": "event_broadcast",
  "time_fit": {
    "conflict": false,
    "preferred_time_match": 0.7,
    "advance_notice_match": 1.0,
    "overall_time_score": 0.85
  },
  "required_actions": [
    "Potvrdiť účasť na evente",
    "Pridať event do kalendára"
  ]
}
```

### Príklad 8: Time-aware matching – kontrola kolízie (NOVÉ v v3)

```json
{
  "user_id": "user_miro_001",
  "event_id": "event_kickoff_001",
  "calendar_check": {
    "event_start": "2026-05-01T16:00:00Z",
    "event_end": "2026-05-01T18:00:00Z",
    "busy_slots": [
      {"start": "2026-05-01T14:00:00Z", "end": "2026-05-01T15:30:00Z"}
    ],
    "has_conflict": false,
    "conflict_details": null,
    "available": true,
    "time_score": 0.95
  }
}
```

---

## 24. 🔥 Zhrnutie

> **Opportunity Engine premieňa potenciál spolupráce na konkrétne, časovo relevantné príležitosti pre jednotlivca – bez centrálneho indexu, výlučne cez anonymizované P2P signály.**

**V2 priniesla:**

* **P2P signály** – namiesto centrálneho indexu sa dopyt a ponuka šíria sieťou
* **Need ↔ Capability matching** – prepojenie medzi potrebami projektu a schopnosťami používateľa (ADR-034)
* **Prepojenie na Participation Filter** – používateľ dostane len relevantné príležitosti
* **Prepojenie na Reciprocity** – participácia generuje reciprocitu (ADR-036)
* **Decentralizácia** – žiadny centrálny server, žiadne centrálne logy

**V3 prináša:**

* **Eventy ako príležitosti** – stretnutia, workshopy, deadliny sú zdrojom príležitostí
* **Prepojenie na ADR-045** – kalendár a eventy
* **Time-aware matching** – zohľadnenie časovej dostupnosti používateľa
* **Event broadcast** – nový typ P2P signálu
* **Calendar integration** – pridávanie eventov do kalendára pri accepte

---

## 25. 🧭 Finálna veta

> **Synergetikum nečaká, kým sa ľudia zapoja — ukazuje im, kde a ako môžu vytvoriť hodnotu práve teraz. A robí to spôsobom, ktorý rešpektuje súkromie, decentralizáciu a autonómiu každého používateľa. A keď projekt organizuje stretnutie, je to príležitosť – nie len položka v kalendári.**

V P2P prostredí nie je možné mať centrálny zoznam „kto čo vie“. Namiesto toho sa príležitosti šíria ako signály – a každý používateľ sa sám rozhodne, či je daná príležitosť pre neho. A každý event je šancou na zapojenie – ak má používateľ čas a záujem.

---

**Dátum:** 2026-04-15  
**Stav:** Accepted  
**Typ:** Core Mechanism ADR (Opportunity Engine)  
**Verzia:** 3.0 (with Calendar & Event Integration)

---

## 📝 Zoznam zmien oproti Version 2

| Sekcia | Zmena |
|--------|-------|
| 1. Status | Zmenené na v3 |
| 2. Kontext | Doplnený dodatočný problém (prepojenie na kalendár a eventy) |
| 3. Rozhodnutie | Doplnené prepojenie na ADR-045 |
| 4. Core princíp | Doplnené o eventy ako príležitosti |
| 5. Definícia | Doplnené `Source: event` a `EventId` |
| 6. Role | Doplnené `Participant` a `Facilitator` |
| 7.2 | Doplnené `Events` do Project/Topic dát |
| 7.3 | Doplnené `Calendar`, `Time conflicts` do kontextu |
| 7.4 | Doplnené účasť na eventoch do histórie |
| 8.2 | Doplnený `Event Broadcast` typ signálu |
| 8.6 | Doplnené `event_opportunities` a `min_advance_notice_hours` do preferencií |
| 8.7 | Doplnené `event_participation_count` do reciprocity |
| 8.8 | **Nová sekcia:** Eventy ako príležitosti |
| 8.9 | **Nová sekcia:** Typy eventových príležitostí |
| 8.10 | **Nová sekcia:** Prepojenie eventových príležitostí na P2P signály |
| 8.11 | **Nová sekcia:** Time-aware matching |
| 9.1 | Doplnené `time_fit` do FitScore |
| 10. | Doplnené `Event proximity` |
| 11. | Doplnené event detection a time availability |
| 12.2 | Nový formát pre eventovú príležitosť |
| 12.3 | Doplnené `Add to calendar` akcia |
| 13. | Doplnené `Event spam protection` |
| 14. | Doplnené `No fake event urgency` |
| 15. | Nový formát explainability pre eventy |
| 16. | Doplnené `Event attendance` |
| 17. | Doplnené eventy s nízkou účasťou |
| 18.1 | Doplnené `EventOpportunity`, `CalendarSlot` |
| 18.2 | Doplnené `EventOpportunityService`, `TimeAwareMatchingService` |
| 18.3 | Doplnené `Calendar store` |
| 18.4 | Doplnené `BroadcastEvent` |
| 19. | Doplnené riziká `Event spam`, `Time conflict` |
| 20. | Doplnená alternatíva `Ignorovanie eventov` |
| 21. Väzby | Doplnené ADR-045 |
| 22. Implementácia | Doplnené pravidlá pre eventy, time-aware matching, kalendár |
| 23.6 | **Nová sekcia:** Príklad 6 – Event broadcast |
| 23.7 | **Nová sekcia:** Príklad 7 – Event-based opportunity |
| 23.8 | **Nová sekcia:** Príklad 8 – Time-aware matching |
| 24. Zhrnutie | Doplnené v3 princípy |
| 25. Finálna veta | Rozšírená o eventy ako príležitosti |