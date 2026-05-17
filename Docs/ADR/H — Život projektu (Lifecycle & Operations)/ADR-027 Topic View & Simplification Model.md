# 🧾 ADR-027: Project View & Simplification Model (v2)

> **Zmeny oproti Version 1:**
> - **Doplnená časť 3.11:** Tree Visualization v View Layer (prepojenie na ADR-025 v4)
> - **Doplnená časť 3.12:** Decision Trail v View Layer
> - **Doplnená časť 3.13:** Impact Highlights v View Layer
> - **Rozšírená časť 3.5:** Zhrnutie vetiev o kvalitu a impact argumentov
> - **Rozšírená časť 3.6:** Identifikácia konfliktov o kvalitou argumentov
> - **Rozšírená časť 6:** Väzby na ADR-025, ADR-028, ADR-042, ADR-045
> - **Nová sekcia 8.4 – 8.6:** Príklady pre tree visualization, decision trail a impact highlights

---

## 1. 📌 Status

**Accepted (v2 – with Tree Visualization & Decision Trail)**

---

## 2. 🎯 Kontext

Systém Personal AI Network umožňuje:

* vznik projektov (ADR-022)
* ich vetvenie (ADR-024)
* kolektívne prispievanie
* **argumentáciu a sledovanie vplyvu (ADR-025 v4)**

---

Výsledkom je, že projekt:

* obsahuje viacero variantov (branchov)
* má rozsiahlu históriu
* obsahuje množstvo argumentov a príspevkov
* **má stromovú štruktúru argumentov (ADR-025)**
* **sleduje impact jednotlivých argumentov na rozhodnutia (ADR-025)**

---

### Problém

Pre bežného používateľa:

👉 je takýto projekt neprehľadný a nepochopiteľný

---

Bez riešenia:

* používateľ sa stratí
* prestane systém používať
* systém stratí adopciu
* **používateľ neuvidí, ktoré argumenty sú kvalitné a ktoré ho ovplyvnili**

---

### Kľúčová otázka

👉
**Ako prezentovať komplexný, rozvetvený projekt tak, aby bol pochopiteľný pre každého používateľa bez straty podstaty – vrátane stromovej vizualizácie argumentov a rozhodovacej cesty?**

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Zavedenie „View Layer“

Systém zavádza:

👉 **Project View Layer**

---

### Definícia

Vrstva, ktorá:

* transformuje komplexný projekt
* do zjednodušeného, personalizovaného pohľadu
* **integrovaného so stromovou vizualizáciou argumentov (v2)**

---

---

## 🔄 3.2 Oddelenie vrstiev

Projekt má dve vrstvy:

---

### 🔹 Reality Layer (plný stav)

Obsahuje:

* všetky vetvy
* všetky príspevky
* celú históriu
* **kompletný argument graph (ADR-025)**
* **všetky impact metrics a decision trails (ADR-025)**

👉 kompletný, ale komplexný

---

---

### 🔹 View Layer (pre používateľa)

Obsahuje:

* vybrané relevantné vetvy
* zhrnutia
* interpretácie
* **zjednodušený argument tree pre aktuálnu vetvu (v2)**
* **rozhodovaciu cestu používateľa (v2)**
* **zvýraznené argumenty s vysokým impactom (v2)**

👉 zjednodušený, ale verný

---

---

## 🧠 3.3 Personalizovaný pohľad

---

View Layer je:

👉 generovaný Personal AI používateľa

---

To znamená:

* každý používateľ vidí projekt inak
* podľa svojho profilu a preferencií
* **vrátane toho, ktoré argumenty považuje za kvalitné (podľa vlastných váh)**

---

---

## 🔍 3.4 Výber relevantných vetiev

---

Systém vyberá:

👉 **limitovaný počet vetiev (napr. 3–5)**

---

Na základe:

* preferencií používateľa
* typu myslenia
* predchádzajúcich reakcií
* relevancie
* **kvality argumentov podporujúcich vetvu (ADR-025)**
* **impactu argumentov na projekt (ADR-025)**

---

---

## 🧾 3.5 Zhrnutie vetiev (Rozšírené v v2)

---

Každá vetva je prezentovaná ako:

---

### 📌 stručný popis

čo návrh rieši

---

### ⚖️ hlavné rozdiely

čo ju odlišuje od ostatných

---

### 📊 podpora

koľko ľudí ju podporuje

---

### 🤝 zapojenie

koľko ľudí sa chce zapojiť

---

### ⚠️ riziká

hlavné slabiny

---

### 🧠 priemerná kvalita argumentov (NOVÉ v v2)

agregované quality score všetkých argumentov podporujúcich vetvu

---

### 💥 priemerný impact (NOVÉ v v2)

agregovaný impact score argumentov vetvy na projekt

---

---

## ⚔️ 3.6 Identifikácia konfliktov (Rozšírené v v2)

---

Systém zvýrazňuje:

👉 hlavné konflikty medzi vetvami

---

Príklad:

* 1 poschodie vs 2 poschodia
* nízke náklady vs vysoká kvalita

**Rozšírenie v v2:** Konflikt je prezentovaný aj ako porovnanie kľúčových argumentov z každej strany, vrátane ich kvality a impactu.

```text
Konflikt: Nízke náklady (Vetva A) vs Vysoká kvalita (Vetva B)
├── Argument pre A: "Lacnejšie materiály = nižšia cena" (Q=0.85, I=0.7)
└── Argument pre B: "Kvalitné materiály = dlhšia životnosť" (Q=0.82, I=0.6)
```

---

---

## ❓ 3.7 Reflexná vrstva

---

Systém generuje otázky pre používateľa:

👉 „ktorý smer ti dáva väčší zmysel?“

👉 „čo je pre teba dôležitejšie?“

👉 **„Ktorý z týchto argumentov ťa najviac ovplyvnil?“ (NOVÉ v v2)**

---

---

## 🚫 3.8 Zákaz manipulácie

---

View Layer NESMIE:

* skryť existujúce vetvy
* preferovať jednu vetvu bez dôvodu
* meniť význam návrhov
* **skrývať kvalitné argumenty kvôli nízkej popularite (NOVÉ v v2)**
* **prezentovať popularitu ako indikátor kvality (NOVÉ v v2)**

---

---

## 🔍 3.9 Prístup k plnej realite

---

Používateľ musí mať možnosť:

👉 prepnúť na Reality Layer

👉 **zobraziť celý argument graph (nie len zjednodušený tree) (NOVÉ v v2)**

---

---

## 🧠 3.10 Adaptácia v čase

---

View Layer:

* sa mení podľa používateľa
* reaguje na jeho rozhodnutia
* učí sa jeho preferencie
* **aktualizuje decision trail po každom rozhodnutí (NOVÉ v v2)**

---

---

## 🌳 3.11 Tree Visualization v View Layer (NOVÉ v v2)

**Princíp:**

> **Používateľ nemusí vidieť celý argument graph – stačí mu zjednodušený strom pre aktuálnu vetvu, ktorý ukazuje kľúčové argumenty a ich vzťahy.**

### A. Čo používateľ vidí

V zjednodušenom view (default) používateľ vidí:

| Komponent | Popis |
|-----------|-------|
| **Argument Tree** | Hierarchická štruktúra argumentov pre aktuálnu vetvu (max 2–3 úrovne) |
| **Kvalita (Q)** | Quality score každého argumentu (0-1) |
| **Impact (I)** | Impact score každého argumentu (-1 až +1) |
| **Podpora** | Počet používateľov, ktorí s argumentom súhlasia (voliteľné) |

### B. Formát zobrazenia

```
🌿 Aktuálna vetva: Crowdfunding
│
├── ✅ Podporný argument: "Buduje komunitu" (Q=0.91, I=0.8) ⭐
│   └── 📖 8 people found this insightful
│
├── ❌ Protiargument: "Časovo náročné" (Q=0.65, I=0.2)
│   └── ⚠️ 2 people requested evidence
│
└── ❌ Protiargument: "Neistý výsledok" (Q=0.70, I=0.3)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 Priemer vetvy: Q=0.75 | I=0.43 | Support=67
```

### C. Interaktívne akcie

Používateľ môže:

| Akcia | Výsledok |
|-------|----------|
| **Rozbaliť/Zbaliť** | Zobraziť/skryť detailné argumenty (podargumenty, protiargumenty) |
| **Filtrovať** | Zobraziť len argumenty s Q > 0.7 |
| **Zoradiť** | Podľa kvality / impactu / popularity |
| **Zvýrazniť** | Argumenty, ktoré ovplyvnili jeho rozhodnutia (z decision trail) |
| **Zobraziť skryté drahokamy** | Argumenty s vysokou kvalitou (Q>0.8) ale nízkou popularitou (P<0.3) |
| **Detail** | Prepnúť na plný argument graph (Reality Layer) |

### D. Prepojenie na ADR-025

Tree Visualization v View Layer je priamo odvodená z:

* Argument graph (ADR-025 3.6)
* Quality metrics (ADR-025 3.16)
* Impact metrics (ADR-025 3.19)

---

---

## 🗺️ 3.12 Decision Trail v View Layer (NOVÉ v v2)

**Princíp:**

> **Používateľ musí vidieť svoju rozhodovaciu cestu – ktoré argumenty ho ovplyvnili a ako sa jeho podpora vyvíjala v čase.**

### A. Čo používateľ vidí

V špecializovanom view („Moja rozhodovacia cesta“) používateľ vidí:

| Komponent | Popis |
|-----------|-------|
| **Timeline** | Časová os jeho rozhodnutí a zmien podpory |
| **Ovplyvňujúce argumenty** | Ktoré argumenty čítal a ako ovplyvnili jeho rozhodnutie |
| **Zmeny podpory** | Ako sa menila jeho podpora pre jednotlivé vetvy |
| **Konečné rozhodnutie** | Akú vetvu si vybral (ak už rozhodol) |

### B. Formát zobrazenia

```
Tvoja rozhodovacia cesta pre projekt "Komunitná pizzéria"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

15.4. 10:00  ⚖️  Začiatok: 3 vetvy (Úver, Crowdfunding, Investor)
              📊 Tvoja podpora: Úver 60% | CF 30% | Inv 10%

15.4. 14:30  📖  Čítal si argument "Buduje komunitu" (Q=0.91, I=0.8)
              ✅ Označil si ho ako "insightful"
              📊 Podpora: Úver 50% | CF 50% | Inv 0%

15.4. 16:00  📖  Čítal si argument "Strata kontroly pri investorovi" (Q=0.88, I=0.7)
              ✅ Označil si "ovplyvnilo moje rozhodnutie"
              📊 Podpora: Úver 40% | CF 60% | Inv 0%

15.4. 18:00  ✅  KONEČNÉ ROZHODNUTIE: Vetva "Crowdfunding"
              🔗 Ovplyvnené argumentmi: "Buduje komunitu", "Strata kontroly"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
💡 Spätná väzba: Tvoje rozhodnutie bolo ovplyvnené 2 kvalitnými argumentmi.
   Žiadny z argumentov nebol populárny (priemer P=0.35), ale mali vysokú kvalitu.
```

### C. Interaktívne akcie

Používateľ môže:

| Akcia | Výsledok |
|-------|----------|
| **Detail argumentu** | Zobraziť celý argument, ktorý ho ovplyvnil |
| **Porovnať s kolektívnym trailom** | Ako sa jeho rozhodovanie líši od priemeru projektu |
| **Exportovať trail** | Zdieľať svoju rozhodovaciu cestu (opt-in) |
| **Prepojiť na argument tree** | Zobraziť ovplyvňujúce argumenty v strome |

### D. Prepojenie na ADR-025

Decision Trail v View Layer je priamo odvodená z:

* Decision trail (ADR-025 3.21)
* Influencing arguments (ADR-025 3.21.A)
* Impact attestation (ADR-025 3.19.D)

---

---

## 💥 3.13 Impact Highlights v View Layer (NOVÉ v v2)

**Princíp:**

> **Používateľ by mal vidieť, ktoré argumenty majú najväčší vplyv na projekt – nie len ktoré sú najpopulárnejšie.**

### A. Čo používateľ vidí

V zjednodušenom view sú zvýraznené:

| Typ zvýraznenia | Popis |
|-----------------|-------|
| **Najvyšší impact** | Argumenty s impact score > 0.7 |
| **Najvyššia kvalita** | Argumenty s quality score > 0.85 |
| **Skryté drahokamy** | Vysoká kvalita (Q>0.8) + nízka popularita (P<0.3) |
| **Game changers** | Argumenty, ktoré spôsobili výrazný shift v podpore (>0.2) |

### B. Formát zobrazenia

```
--- Argumenty s najvyšším impactom ---
💥 "Buduje komunitu" (I=0.8, Q=0.91) – posunul podporu o +20%

--- Skryté drahokamy (vysoká kvalita, nízka popularita) ---
⭐ "Lokálne suroviny vytvárajú komunitný pocit" (Q=0.89, P=0.12, I=0.68)
   → Len 12% súhlasí, ale kvalita je vysoká. Možno prehliadaný argument.

--- Game changers (výrazný vplyv na projekt) ---
🔄 "Strata kontroly pri investorovi" – viedol k vytvoreniu novej vetvy
   → 3 ľudia explicitne potvrdili, že ich ovplyvnil
```

### C. Prepojenie na ADR-025

Impact Highlights sú odvodené z:

* Impact score (ADR-025 3.19.A)
* Quality score (ADR-025 3.16)
* Popularity score (ADR-025 3.18)
* Support shift (ADR-025 3.19.C)

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitíva

---

### 🧩 Zrozumiteľnosť

* projekt je pochopiteľný
* **stromová vizualizácia robí argumenty prehľadnými (v2)**

---

### 📈 Škálovateľnosť

* systém zvládne komplexitu

---

### 🎯 Personalizácia

* každý vidí relevantné veci
* **každý vidí svoju vlastnú rozhodovaciu cestu (v2)**

---

### 🔍 Transparentnosť vplyvu

* používateľ vidí, ktoré argumenty ho ovplyvnili (v2)
* používateľ vidí, ktoré argumenty majú skutočný impact (v2)

---

### 🧠 Vzdelávací efekt

* používateľ sa učí z vlastnej rozhodovacej cesty (v2)
* používateľ objavuje „skryté drahokamy“ (v2)

---

---

## ❗ Negatíva / Trade-offs

---

### ⚠️ Riziko skreslenia

* používateľ nevidí všetko
* **zjednodušený strom môže vynechať dôležité súvislosti (v2)**

---

### 🧠 Závislosť na kvalite AI

* zlé zhrnutie = zlý pohľad
* **zlá extrakcia argumentov = zlý strom (v2)**

---

### 📊 Výpočtová náročnosť

* generovanie personalizovaného stromu vyžaduje výpočty (v2)
* **generovanie decision trailu vyžaduje sledovanie histórie (v2)**

---

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Zobraziť celý projekt všetkým

* transparentné
* ale nepoužiteľné

---

### ❌ Fixné zhrnutie pre všetkých

* jednoduché
* ale nepersonalizované

---

### ❌ Iba lineárny zoznam argumentov (bez stromu)

* jednoduchšie
* ale stráca sa hierarchia a vzťahy (v2)

---

### ❌ Žiadna decision trail (len konečné rozhodnutie)

* jednoduchšie
* ale používateľ nevidí, prečo sa rozhodol tak, ako sa rozhodol (v2)

---

---

## 6. 🔗 Väzby na ďalšie ADR

---

| ADR | Vzťah |
|-----|-------|
| ADR-011 | Personalization – personalizovaný výber vetiev a argumentov |
| ADR-022 | Project Formation – projekty, ktoré view layer zobrazuje |
| ADR-024 | Branching – vetvy, ktoré view layer vyberá |
| **ADR-025** | **Argumentation – tree visualization, decision trail, impact metrics (v2)** |
| ADR-028 | Support Model – podpora vetiev, zmeny v čase |
| **ADR-042** | **Unified Entity Model – Argument a Branch sú typy Node (v2)** |
| **ADR-045** | **Calendar & Events – decision trail môže byť prepojený s eventami (v2)** |

---

---

## 7. 📏 Pravidlá implementácie (high-level)

---

Systém musí:

* oddeliť reality a view layer
* generovať personalizované zhrnutia
* limitovať počet vetiev
* umožniť prístup k plnej verzii
* **poskytnúť stromovú vizualizáciu argumentov pre aktuálnu vetvu (v2)**
* **umožniť rozbaľovanie/zbaľovanie úrovní stromu (v2)**
* **poskytnúť filtrovanie argumentov podľa kvality (v2)**
* **zobraziť „skryté drahokamy“ (vysoká kvalita, nízka popularita) (v2)**
* **poskytnúť decision trail pre každého používateľa (v2)**
* **zobraziť zmeny podpory v čase (v2)**
* **zvýrazniť argumenty, ktoré ovplyvnili používateľovo rozhodnutie (v2)**
* **nikdy neskrývať kvalitné argumenty kvôli nízkej popularite (v2)**

---

---

## 8. 🔥 Príklady

---

### Príklad 1: Základný view – zoznam vetiev so zhrnutím

```json
{
  "user_id": "user_miro_001",
  "project_id": "proj_pizzeria_001",
  "view": {
    "branches": [
      {
        "branch_id": "branch_loan",
        "name": "Klasický úver",
        "summary": "Tradičné bankové financovanie",
        "support_count": 34,
        "avg_quality": 0.72,
        "avg_impact": 0.45,
        "main_risks": ["Úroky", "Zábezpeka"]
      },
      {
        "branch_id": "branch_crowdfunding",
        "name": "Crowdfunding",
        "summary": "Zbierka od komunity",
        "support_count": 67,
        "avg_quality": 0.85,
        "avg_impact": 0.65,
        "main_risks": ["Časová náročnosť", "Neistý výsledok"],
        "highlighted": true
      }
    ]
  }
}
```

---

### Príklad 2: Tree Visualization – argument tree pre vetvu (NOVÉ v v2)

```json
{
  "user_id": "user_miro_001",
  "branch_id": "branch_crowdfunding",
  "argument_tree": {
    "root": {
      "argument_id": "arg_cf_root",
      "claim": "Crowdfunding je najlepšia cesta",
      "quality": 0.82,
      "impact": 0.70,
      "children": [
        {
          "argument_id": "arg_cf_001",
          "claim": "Buduje komunitu",
          "quality": 0.91,
          "impact": 0.80,
          "stance": "support",
          "insightful_count": 8,
          "children": []
        },
        {
          "argument_id": "arg_cf_002",
          "claim": "Časovo náročné",
          "quality": 0.65,
          "impact": -0.20,
          "stance": "oppose",
          "evidence_requests": 2,
          "children": []
        }
      ]
    },
    "filters_applied": {
      "min_quality": 0.0,
      "max_depth": 2
    }
  }
}
```

---

### Príklad 3: Decision Trail – používateľova rozhodovacia cesta (NOVÉ v v2)

```json
{
  "user_id": "user_miro_001",
  "project_id": "proj_pizzeria_001",
  "decision_trail": {
    "started_at": "2026-04-15T10:00:00Z",
    "decided_at": "2026-04-15T18:00:00Z",
    "events": [
      {
        "timestamp": "2026-04-15T10:00:00Z",
        "event_type": "initial",
        "support": {
          "branch_loan": 0.6,
          "branch_crowdfunding": 0.3,
          "branch_investor": 0.1
        }
      },
      {
        "timestamp": "2026-04-15T14:30:00Z",
        "event_type": "read_argument",
        "argument_id": "arg_cf_001",
        "argument_claim": "Buduje komunitu",
        "quality": 0.91,
        "impact": 0.80,
        "user_action": "marked_insightful",
        "new_support": {
          "branch_loan": 0.5,
          "branch_crowdfunding": 0.5,
          "branch_investor": 0.0
        }
      },
      {
        "timestamp": "2026-04-15T16:00:00Z",
        "event_type": "read_argument",
        "argument_id": "arg_investor_002",
        "argument_claim": "Strata kontroly pri investorovi",
        "quality": 0.88,
        "impact": 0.70,
        "user_action": "attested_influence",
        "new_support": {
          "branch_loan": 0.4,
          "branch_crowdfunding": 0.6,
          "branch_investor": 0.0
        }
      },
      {
        "timestamp": "2026-04-15T18:00:00Z",
        "event_type": "decision",
        "selected_branch": "branch_crowdfunding",
        "confidence": 0.85,
        "influencing_arguments": ["arg_cf_001", "arg_investor_002"]
      }
    ],
    "feedback": {
      "quality_over_popularity": true,
      "avg_influencing_argument_quality": 0.895,
      "avg_influencing_argument_popularity": 0.35
    }
  }
}
```

---

### Príklad 4: Impact Highlights – zvýraznené argumenty (NOVÉ v v2)

```json
{
  "project_id": "proj_pizzeria_001",
  "impact_highlights": {
    "highest_impact": [
      {
        "argument_id": "arg_cf_001",
        "claim": "Buduje komunitu",
        "impact_score": 0.80,
        "quality_score": 0.91,
        "support_shift": 0.20
      }
    ],
    "hidden_gems": [
      {
        "argument_id": "arg_local_003",
        "claim": "Lokálne suroviny vytvárajú komunitný pocit",
        "quality_score": 0.89,
        "popularity_score": 0.12,
        "impact_score": 0.68
      }
    ],
    "game_changers": [
      {
        "argument_id": "arg_investor_002",
        "claim": "Strata kontroly pri investorovi",
        "effect": "branch_created",
        "new_branch_id": "branch_crowdfunding"
      }
    ]
  }
}
```

---

### Príklad 5: Používateľské nastavenia view layer (NOVÉ v v2)

```json
{
  "user_id": "user_miro_001",
  "view_preferences": {
    "default_branch_limit": 3,
    "default_sorting": "quality",
    "show_hidden_gems": true,
    "tree_max_depth": 2,
    "tree_min_quality": 0.6,
    "decision_trail_enabled": true,
    "impact_highlights_enabled": true,
    "show_popularity": false
  }
}
```

---

---

## 9. 🔥 Zhrnutie

---

👉
**Komplexný projekt je transformovaný do personalizovaného, pochopiteľného pohľadu bez straty podstaty.**

**V2 prináša:**

* **Tree Visualization** – stromová vizualizácia argumentov pre aktuálnu vetvu
* **Decision Trail** – sledovanie rozhodovacej cesty každého používateľa
* **Impact Highlights** – zvýraznenie argumentov s najväčším vplyvom
* **Skryté drahokamy** – kvalitné, ale nepopulárne argumenty
* **Prepojenie na ADR-025** – impact metrics, quality metrics, decision trail

---

---

## 10. 🧭 Finálna veta

---

👉
**Používateľ nemusí pochopiť celý systém – stačí, ak pochopí svoju časť reality. A svoju časť reality vidí ako strom argumentov, svoju rozhodovaciu cestu a argumenty, ktoré skutočne zmenili smer projektu.**

---

**Dátum:** 2026-04-15  
**Stav:** Accepted  
**Typ:** Core Mechanism ADR (View Layer)  
**Verzia:** 2.0 (with Tree Visualization & Decision Trail)

---

## 📝 Zoznam zmien oproti Version 1

| Sekcia | Zmena |
|--------|-------|
| 1. Status | Zmenené na v2 |
| 2. Kontext | Doplnené argumentácia, impact, decision trail |
| 2. Problém | Doplnené o stratu prehľadu o argumentoch |
| 2. Kľúčová otázka | Rozšírená o stromovú vizualizáciu a decision trail |
| 3.1 | Doplnené integrovanie so stromovou vizualizáciou |
| 3.2 | Doplnené argument graph, impact metrics, decision trail do Reality Layer |
| 3.2 | Doplnené argument tree, decision trail, impact highlights do View Layer |
| 3.3 | Doplnené o vlastné váhy kvality |
| 3.4 | Doplnené o kvalitu a impact argumentov pri výbere vetiev |
| 3.5 | Doplnené avg_quality a avg_impact do zhrnutia vetiev |
| 3.6 | Doplnené porovnanie argumentov pri konfliktoch |
| 3.7 | Doplnená otázka o ovplyvňujúce argumenty |
| 3.8 | Doplnené zákaz skrývania kvalitných argumentov a popularity ako indikátora kvality |
| 3.9 | Doplnené zobrazenie celého argument graphu |
| 3.10 | Doplnené aktualizácia decision trailu |
| 3.11 | **Nová sekcia:** Tree Visualization v View Layer |
| 3.12 | **Nová sekcia:** Decision Trail v View Layer |
| 3.13 | **Nová sekcia:** Impact Highlights v View Layer |
| 4. Pozitíva | Doplnené stromová vizualizácia, decision trail, skryté drahokamy, vzdelávací efekt |
| 4. Negatíva | Doplnené riziko vynechania súvislostí, závislosť na extrakcii argumentov, výpočtová náročnosť |
| 5. Alternatívy | Doplnené "iba lineárny zoznam" a "žiadna decision trail" |
| 6. Väzby | Doplnené ADR-025, ADR-042, ADR-045 |
| 7. Implementácia | Doplnené pravidlá pre tree visualization, decision trail, impact highlights, skryté drahokamy |
| 8.1 | Pôvodný príklad (zoznam vetiev) |
| 8.2 | **Nová sekcia:** Príklad 2 – Tree Visualization |
| 8.3 | **Nová sekcia:** Príklad 3 – Decision Trail |
| 8.4 | **Nová sekcia:** Príklad 4 – Impact Highlights |
| 8.5 | **Nová sekcia:** Príklad 5 – Používateľské nastavenia |
| 9. Zhrnutie | Doplnené v2 princípy |
| 10. Finálna veta | Rozšírená o strom, decision trail a impact |