# 🧾 ADR-036: Reciprocity & Value Exchange Model (v2)

> **Zmeny oproti Version 1:**
> - **Doplnená časť: Shared Foundation Creation ako forma reciprocity (prepojenie na INSPIRATION-08)**
> - **Doplnená časť: Typy shared foundation modelov a ich recipročné hodnoty**
> - **Rozšírená časť: Vzťah k participácii o shared foundation**
> - **Rozšírená časť 6: Väzby na ADR-008, ADR-031, ADR-034**
> - **Nová sekcia 8.4: Príklad pre shared foundation creation**

---

## 1. 📌 Status

**Accepted (v2 – with Shared Foundation Reciprocity)**

---

## 2. 🎯 Kontext

Synergetikum je systém postavený na princípoch:

* **Personal ownership** (ADR-001)
* **Privacy-first architecture** (ADR-004)
* **Non-manipulation** (ADR-003)
* **Human-in-the-loop** (ADR-002)

Systém nemá:

* ❌ body, rebríčky, levely
* ❌ gamifikáciu ako motivátor
* ❌ penalizáciu za neparticipáciu

---

### Problém

> **Ako motivovať používateľov k participácii, zdieľaniu a spolupráci bez vytvárania externých stimulov (body, odmeny), ktoré by deformovali správanie?**

---

Bez riešenia:

* systém sa môže stať „tragédiou spoločnej pastviny“ (všetci čerpajú, nikto neprispieva)
* kvalita odpovedí a argumentov klesá
* používatelia necítia, že ich participácia má zmysel

---

### Kľúčová otázka

👉
**Ako vytvoriť vnútornú ekonomiku hodnoty, kde participácia prirodzene vedie k lepším výstupom pre participanta – bez bodov, rebríčkov a externých odmen?**

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Core princíp

> **Reciprocita nie je odmena. Je to prirodzený dôsledok participácie v systéme.**

Systém nepoužíva:

* body za príspevky
* rebríčky najaktívnejších
* odznaky za úspechy

Namiesto toho:

👉 **kto viac participuje, tomu systém lepšie slúži**

---

## 🔄 3.2 Ako reciprocita funguje

Reciprocita v Synergetiku je **implicitná a emergentná**:

| Akcia používateľa | Dôsledok v systéme |
|-------------------|---------------------|
| Odpovedá na otázky | Jeho Personal AI má lepší profil → relevantnejšie otázky |
| Hodnotí argumenty | Jeho preferencie sú presnejšie → lepšie odporúčania |
| Prispieva do projektov | Jeho Capability Model je bohatší → viac príležitostí |
| Zdieľa foundation model (v2) | Získava reciprocity credit → vyššia priorita jeho signálov |
| Ignoruje / neparticipuje | Jeho profil je menej presný → menej relevantné výstupy |

> **Nie je to penalizácia. Je to prirodzený dôsledok: systém má menej signálov, podľa ktorých by sa mohol učiť.**

---

## 🧠 3.3 Cost asymmetry

Kľúčový princíp:

> **Neparticipácia neznamená trest. Znamená absenciu benefitu, ktorý participácia prináša.**

| Úroveň participácie | Benefit od systému |
|---------------------|--------------------|
| Vysoká (odpovedá, hodnotí, tvorí) | Vysoko presné odporúčania, bohatý Interest Model |
| Stredná (občas odpovedá) | Stredne presné odporúčania |
| Nízka (len číta) | Všeobecné, nepersonalizované výstupy |
| Žiadna | Len základný fallback model (ADR-040) |

👉 Nikdy: „tvoj účet je obmedzený, lebo málo prispievaš“

---

## 🧩 3.4 Čo reciprocita NIE JE

Systém NESMIE:

* vytvárať viditeľné „skóre“ používateľa
* porovnávať používateľov medzi sebou
* penalizovať neparticipáciu
* vytvárať tlak na „udržanie skóre“

---

## 🔄 3.5 Reciprocita v praxi

### Pre otázky a odpovede (ADR-009)

* Kto viac odpovedá, jeho Personal AI má lepšie nastavené váhy
* Jeho otázky sú relevantnejšie
* Dostáva odpovede skôr (priority queue)

### Pre argumenty a diskusiu (ADR-025)

* Kto viac hodnotí argumenty, jeho Quality Model je presnejší
* Vidí argumenty zoradené podľa jeho preferencií
* Jeho vlastné argumenty majú vyššiu šancu byť videné (nie vyhrávajú – len sú viditeľnejšie)

### Pre projekty (ADR-022)

* Kto viac prispieva do projektov, jeho Capability Model je bohatší
* Opportunity Engine (ADR-019) mu ponúka relevantnejšie príležitosti
* Jeho commitment signály majú vyššiu váhu (ADR-028)

---

## 🧠 3.6 Shared Foundation Creation ako forma reciprocity (NOVÉ v v2)

**Princíp:**

> **Vytvorenie shared foundation modelu je spoločensky hodnotný akt, ktorý generuje reciprocitu pre všetkých prispievateľov.**

### A. Čo je shared foundation

Shared foundation je spoločný základný model, ktorý môže byť:

| Typ | Popis | Príklad |
|-----|-------|---------|
| **Global minimal** | Základný model dodávaný so systémom | Jazykový model, základné kategórie |
| **Community foundation** | Model vytvorený z Trusted Circle | Zdravotný model pre komunitu diabetikov |
| **Domain foundation** | Model pre špecifickú oblasť | Poľnohospodársky model pre farmárov |
| **Project foundation** | Model špecifický pre jeden projekt | Model pre participatívny rozpočet mesta |

### B. Ako vzniká shared foundation

1. Skupina používateľov v Trusted Circle sa dohodne na zdieľaní
2. Ich Personal AI modely vygenerujú agregovaný checkpoint
3. Checkpoint prejde validation gate (ADR-007)
4. Nový foundation model je k dispozícii pre členov circle
5. Každý člen si ho môže prijať ako svoj "base" a naň stavať personal delta (ADR-008)

### C. Reciprocita za vytvorenie shared foundation

| Úroveň participácie na tvorbe | Reciprocita |
|-------------------------------|-------------|
| **Initiator** (inicioval vznik) | Vysoký reciprocity credit + jeho meno v metadata foundationu (ak súhlasí) |
| **Major contributor** (poskytol >10% dát) | Stredný reciprocity credit |
| **Minor contributor** (poskytol <10% dát) | Základný reciprocity credit |
| **Validator** (schválil checkpoint) | Základný reciprocity credit |
| **User** (len používa, netvoril) | Žiadny credit (foundation je spoločný majetok) |

### D. Čo reciprocity credit znamená

Prispievatelia, ktorí vytvorili shared foundation, získavajú:

| Benefit | Popis |
|---------|-------|
| **Higher signal priority** | Ich capability announcements majú vyššiu prioritu v P2P sieti |
| **Early access** | K novým verziám foundationu majú prístup skôr |
| **Influence on evolution** | Ich spätná väzba na foundation má vyššiu váhu |
| **Attribution** | Ich meno je uvedené v metadata foundationu (ak súhlasia) |

### E. Shared foundation a personal delta

Dôležité pravidlo:

> **Shared foundation je spoločný majetok. Každý člen circle si ho môže slobodne prijať alebo odmietnuť. Prijatie foundationu nevyžaduje reciprocity credit – foundation je dostupný všetkým.**

Reciprocita sa vzťahuje **len na proces tvorby**, nie na používanie.

### F. Prepojenie na ADR-008 (Foundation + Personal Delta)

Shared foundation je oddelený od personal delta:

```
Personal Model = Shared Foundation + Personal Delta
```

* Shared foundation sa mení zriedkavo a konzervatívne
* Personal delta je vždy nadradený – foundation sa mení len výnimočne
* Používateľ môže mať viacero foundationov pre rôzne kontexty

---

## 🔗 3.7 Vzťah k Opportunity Engine (ADR-019)

Reciprocita ovplyvňuje Opportunity Engine:

* Používateľ s vysokou reciprocitou dostáva **relevantnejšie** príležitosti (nie viac)
* Jeho vlastné potreby (needs) majú vyššiu prioritu pri šírení
* Nie je to „pay to win“ – je to „participate to get better service“

```json
// Príklad: Reciprocita ovplyvňujúca priority
{
  "user_id": "user_miro_001",
  "reciprocity": {
    "score": 0.75,
    "signal_priority_multiplier": 1.2,  // Jeho signály majú vyššiu váhu
    "incoming_priority_multiplier": 1.1  // Dostáva kvalitnejšie príležitosti
  }
}
```

---

## 🧠 3.8 Vzťah k participácii (ADR-012, ADR-015)

Reciprocita je prepojená s Participation Profile (ADR-034):

* Vyššia participácia → vyššia reciprocita → lepšie výstupy
* Ale: participácia nie je jediný faktor – dôležitá je aj **kvalita** (ADR-025)

---

## 4. 🧠 Dôsledky rozhodnutia

### ✅ Pozitíva

| Oblasť | Dôsledok |
|--------|----------|
| **Žiadna manipulácia** | Žiadne body, rebríčky, odmeny – neexistuje dôvod „hrať systém“ |
| **Prirodzená motivácia** | Používateľ vidí, že jeho participácia zlepšuje systém pre neho |
| **Súkromie** | Reciprocita nie je viditeľné skóre – je to interná metrika |
| **Spravodlivosť** | Kto viac dáva, tomu systém viac vracia – bez vytvárania elít |
| **Shared foundation ako hodnota (v2)** | Tvorba foundationov je incentivizovaná bez peňazí alebo bodov |
| **Komunitná tvorba (v2)** | Shared foundation modely vznikajú zdola, nie zhora |

### ❗ Negatíva / Trade-offs

| Oblasť | Dôsledok |
|--------|----------|
| **Pomalšia adopcia** | Bez viditeľných odmen môže byť ťažšie motivovať nových používateľov |
| **Neviditeľnosť** | Používateľ nevidí „čo získa“, keď prispieva (na rozdiel od bodov) |
| **Komplexita vysvetlenia** | Reciprocita je jemnejšia než jednoduché skóre |
| **Možná frustrácia** | Používateľ, ktorý veľa prispieva, nemusí vidieť okamžitý benefit |

---

## 5. 🚫 Alternatívy (zamietnuté)

### ❌ Gamifikácia (body, levely, odznaky)

* jednoduchá na pochopenie
* ale: deformuje správanie, vytvára tlak, porušuje ADR-003

### ❌ Kryptomena / tokeny

* decentralizované
* ale: prináša finančnú špekuláciu, externalizuje motiváciu

### ❌ Žiadna reciprocita (čistý altruizmus)

* čisté, jednoduché
* ale: systém zlyhá pri nízkej participácii (tragédia spoločnej pastviny)

### ❌ Iba shared foundation bez reciprocity (v2)

* jednoduchšie
* ale: nikto nemá motiváciu foundation vytvárať – vznikajú len globálne modely zhora

---

## 6. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-001 | Ownership – reciprocita nie je centrálne skóre |
| ADR-003 | Non-Manipulation – reciprocita nie je viditeľná, nie je tlak |
| ADR-004 | Privacy-first – reciprocita je interná metrika |
| **ADR-008** | **Foundation + Personal Delta – shared foundation je oddelený od personal delta (v2)** |
| ADR-012 | Engagement – participácia zvyšuje reciprocitu |
| ADR-015 | Participation Filter – reciprocita ovplyvňuje relevantnosť |
| ADR-019 | Opportunity Engine – reciprocita ovplyvňuje priority |
| ADR-025 | Argumentation – kvalita argumentov je faktor |
| ADR-028 | Support & Alignment – commitment signály majú vyššiu váhu |
| ADR-031 | Topic Layer – shared foundation môže byť viazaný na tému (v2) |
| ADR-034 | Personal AI Internal Model – reciprocity score je súčasťou Participation Profile |
| ADR-038 | Power & Influence – reciprocita nesmie vytvárať mocenskú hierarchiu |
| ADR-040 | Graceful Degradation – nízka participácia = horšie, nie zlomené |

---

## 7. 📏 Pravidlá implementácie

Systém musí:

* sledovať participáciu internou metrikou (reciprocity score)
* používať reciprocitu **len interne** (nikdy nezobrazovať používateľovi)
* nikdy nevytvárať rebríčky alebo porovnania
* nikdy nepenalizovať nízku participáciu (len znížiť kvalitu výstupov)
* **umožniť skupinám používateľov vytvárať shared foundation modely (v2)**
* **priraďovať reciprocity credit prispievateľom shared foundation (v2)**
* **zvyšovať prioritu signálov prispievateľom shared foundation (v2)**
* **nikdy nevyžadovať reciprocity credit pre použitie shared foundation (v2)**

---

## 8. 🔥 Príklady

### Príklad 1: Reciprocita v otázkach

```json
{
  "user_id": "user_miro_001",
  "reciprocity": {
    "score": 0.68,
    "factors": {
      "questions_answered": 47,
      "arguments_rated": 123,
      "project_contributions": 5,
      "shared_foundation_contributions": 1  // NOVÉ v v2
    },
    "signal_priority": 1.15
  }
}
```

### Príklad 2: Vplyv na Opportunity Engine

```json
{
  "opportunity_id": "opp_001",
  "user_id": "user_miro_001",
  "fit_score": 0.82,
  "reciprocity_adjusted_priority": 0.94,  // 0.82 * 1.15
  "reasoning": "Vysoká reciprocita – tvoje signály majú vyššiu prioritu"
}
```

### Príklad 3: Čo používateľ vidí (a nevidí)

Používateľ **nevidí** svoje reciprocity score.

Používateľ **vidí**:

```
Prečo ti bola táto otázka odporučená?
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✓ Zaujímaš sa o tému "community_food"
✓ Odpovedal si na 5 podobných otázok
✓ Tvoje odpovede boli vyhodnotené ako kvalitné
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
→ Čím viac sa zapájaš, tým relevantnejšie otázky ti systém ponúka.
```

### Príklad 4: Shared Foundation Creation (NOVÉ v v2)

```json
{
  "foundation_id": "foundation_community_health_001",
  "type": "community_foundation",
  "name": "Zdravotný model pre diabetickú komunitu",
  "created_by_circle": "circle_diabetics_sk",
  "created_at": "2026-04-15T10:00:00Z",
  "contributors": [
    {
      "user_id": "user_miro_001",
      "contribution_level": "major_contributor",
      "data_percentage": 15.2,
      "reciprocity_credit_awarded": 0.25,
      "attribution_consent": true
    },
    {
      "user_id": "user_jana_001",
      "contribution_level": "initiator",
      "data_percentage": 5.0,
      "reciprocity_credit_awarded": 0.4,
      "attribution_consent": true
    },
    {
      "user_id": "user_peter_001",
      "contribution_level": "validator",
      "data_percentage": 0,
      "reciprocity_credit_awarded": 0.05,
      "attribution_consent": false
    }
  ],
  "statistics": {
    "total_contributors": 12,
    "total_data_points": 3450,
    "version": "1.0.0"
  },
  "reciprocity_impact": {
    "contributor_priority_multiplier": 1.3,
    "non_contributor_priority_multiplier": 1.0
  }
}
```

### Príklad 5: Používateľ s viacerými shared foundation contributions

```json
{
  "user_id": "user_miro_001",
  "reciprocity": {
    "score": 0.82,
    "factors": {
      "questions_answered": 47,
      "arguments_rated": 123,
      "project_contributions": 5,
      "shared_foundation_contributions": [
        {
          "foundation_id": "foundation_community_health_001",
          "level": "major_contributor",
          "credit": 0.25
        },
        {
          "foundation_id": "foundation_domain_agriculture_001",
          "level": "minor_contributor",
          "credit": 0.1
        },
        {
          "foundation_id": "foundation_project_pizzeria_001",
          "level": "initiator",
          "credit": 0.4
        }
      ],
      "total_reciprocity_credits": 0.75
    },
    "signal_priority_multiplier": 1.35,
    "incoming_priority_multiplier": 1.25
  }
}
```

---

## 9. 🔥 Zhrnutie

> **Reciprocita v Synergetiku nie je odmena. Je to prirodzený dôsledok participácie: kto viac prispieva, tomu systém lepšie slúži. Nie je to súťaž – je to investícia do vlastnej skúsenosti.**

**V2 prináša:**

* **Shared Foundation Creation** – tvorba spoločných modelov je incentivizovaná reciprocitou
* **Typy shared foundation** – global minimal, community, domain, project
* **Reciprocity credit** – pre prispievateľov, nie pre používateľov
* **Higher signal priority** – pre tých, ktorí vytvárajú hodnotu pre komunitu
* **Attribution** – dobrovoľné uvedenie mena tvorcov

---

## 10. 🧭 Finálna veta

> **Synergetikum neodmeňuje tých, čo kričia najhlasnejšie. Slúži tým, ktorí ho pomáhajú budovať – a to tým, že im ukazuje svet, ktorý ich zaujíma.**

**V2 rozšírenie:** A tým, ktorí vytvárajú shared foundation pre celú komunitu, otvára dvere k ešte lepšej spolupráci – nie cez body alebo odmeny, ale cez prirodzenú prioritu v sieti.

---

**Dátum:** 2026-04-15  
**Stav:** Accepted  
**Typ:** Core Mechanism ADR (Reciprocity)  
**Verzia:** 2.0 (with Shared Foundation Reciprocity)

---

## 📝 Zoznam zmien oproti Version 1

| Sekcia | Zmena |
|--------|-------|
| 1. Status | Zmenené na v2 |
| 3.2 | Doplnené "Zdieľa foundation model" do tabuľky akcií |
| 3.6 | **Nová sekcia:** Shared Foundation Creation ako forma reciprocity |
| 3.6.A | Typy shared foundation (global minimal, community, domain, project) |
| 3.6.B | Proces vzniku shared foundation |
| 3.6.C | Reciprocita za vytvorenie (initiator, major, minor, validator, user) |
| 3.6.D | Benefity reciprocity credit (higher signal priority, early access, influence, attribution) |
| 3.6.E | Shared foundation ako spoločný majetok (žiadny credit za používanie) |
| 3.6.F | Prepojenie na ADR-008 (Foundation + Personal Delta) |
| 4. Pozitíva | Doplnené shared foundation ako hodnota, komunitná tvorba |
| 5. Alternatívy | Doplnené "iba shared foundation bez reciprocity" |
| 6. Väzby | Doplnené ADR-008, ADR-031 |
| 7. Implementácia | Doplnené pravidlá pre shared foundation creation |
| 8.1 | Doplnené `shared_foundation_contributions` do príkladu |
| 8.4 | **Nová sekcia:** Príklad 4 – Shared Foundation Creation |
| 8.5 | **Nová sekcia:** Príklad 5 – Používateľ s viacerými shared foundation contributions |
| 9. Zhrnutie | Doplnené v2 princípy |
| 10. Finálna veta | Rozšírená o shared foundation |