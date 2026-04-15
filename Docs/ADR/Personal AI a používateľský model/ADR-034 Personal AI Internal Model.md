# 🧾 ADR-034: Personal AI Internal Model (v2)

> **Zmeny oproti Version 1:**
> - **Doplnená časť 4.1.1:** Interest Model a taxonómia (prepojenie na INSPIRATION-09)
> - **Doplnená časť 4.1.2:** Štruktúrovaný Interest Model s kategóriami a tagmi
> - **Doplnená časť 4.1.3:** Sémantické vzťahy medzi témami
> - **Doplnená časť 4.1.4:** Propagácia záujmu cez vzťahy
> - **Rozšírená časť 12:** Väzby na ADR-031, ADR-042
> - **Nová sekcia 14.6:** Príklad pre štruktúrovaný Interest Model

---

## 1. 📌 Status

**Accepted (v2 – with Structured Interest Model & Taxonomy)**

---

## 2. 🎯 Kontext

Personal AI je základný stavebný prvok systému Synergetikum.

Doteraz je definovaná ako:

* lokálny model používateľa
* súkromná
* rozhoduje o participácii
* filtruje obsah
* interpretuje projekty

---

### Problém

Nie je definované:

👉 čo presne Personal AI obsahuje
👉 čo sa učí
👉 ako sa mení
👉 **ako je štruktúrovaný Interest Model (najmä vzťah k taxonómii)**

---

Bez toho:

* systém je neoveriteľný
* správanie je nepredvídateľné
* architektúra je neúplná
* **Interest Model je plochý zoznam, ignoruje hierarchiu a vzťahy medzi témami**

---

## 3. ⚖️ Rozhodnutie

Personal AI je definovaná ako:

👉 **lokálny adaptívny model preferencií, schopností a rozhodovacích vzorcov používateľa**

**V2 rozšírenie:** Interest Model je štruktúrovaný podľa **dvojúrovňovej taxonómie** (kategórie + tagy) a využíva **sémantické vzťahy** medzi témami pre inteligentné šírenie záujmu.

---

## 4. 🧠 Core Model

Personal AI obsahuje 5 hlavných vrstiev:

---

### 4.1 Interest Model (záujmy)

Reprezentuje:

* témy, ktoré používateľa zaujímajú
* intenzitu záujmu
* **štruktúru záujmov (hierarchia + tagy) (v2)**

---

#### 4.1.1 Interest Model a taxonómia (NOVÉ v v2)

Interest Model je štruktúrovaný podľa **dvojúrovňovej taxonómie**:

| Úroveň | Popis | Príklad |
|--------|-------|---------|
| **Kategórie (strom)** | Hierarchické, dedičné | `business → small_business → food_business` |
| **Tagy (graf)** | Voľné, nehierarchické | `#sustainable`, `#community`, `#local` |

##### A. Kategórie (stromová štruktúra)

Kategórie tvoria **hierarchický strom**:

```
business (0.6)
├── small_business (0.7)
│   ├── food_business (0.9)
│   └── retail_business (0.4)
└── corporate_business (0.2)
```

**Pravidlá dedičnosti:**
* Záujem o podkategóriu (`food_business = 0.9`) automaticky znamená záujem o nadkategóriu (`business = 0.6`) – ale s nižšou váhou
* Záujem o nadkategóriu **nemusí** znamenať záujem o podkategóriu

##### B. Tagy (grafová štruktúra)

Tagy sú **voľné, nehierarchické**:

```json
{
  "tags": {
    "#sustainable": 0.8,
    "#community": 0.7,
    "#local": 0.6,
    "#organic": 0.4,
    "#vegan": 0.1
  }
}
```

Tagy:
* sú nezávislé od kategórií
* môžu sa prekrývať s viacerými kategóriami
* umožňujú jemnozrnné vyjadrenie záujmu

---

#### 4.1.2 Štruktúrovaný Interest Model (NOVÉ v v2)

```json
{
  "interest_model": {
    "categories": {
      "business": 0.6,
      "business.small_business": 0.7,
      "business.small_business.food_business": 0.9,
      "business.small_business.retail_business": 0.4,
      "business.corporate_business": 0.2,
      "technology": 0.5,
      "technology.opensource": 0.8,
      "sustainability": 0.7,
      "sustainability.local_food": 0.9
    },
    "tags": {
      "#sustainable": 0.8,
      "#community": 0.7,
      "#local": 0.6,
      "#organic": 0.4,
      "#vegan": 0.1
    },
    "semantic_relations": {
      "preferred": ["related_to", "supports"],
      "avoided": ["contradicts"]
    }
  }
}
```

---

#### 4.1.3 Sémantické vzťahy medzi témami (NOVÉ v v2)

Pre inteligentné šírenie záujmu systém pozná tieto vzťahy medzi témami:

| Vzťah | Význam | Vplyv na Interest Model | Príklad |
|-------|--------|------------------------|---------|
| `parent_of` | Hierarchia (kategória nadradená) | Záujem o child → záujem o parent (s váhou 0.5) | `food_business` → `small_business` |
| `child_of` | Hierarchia (kategória podradená) | Záujem o parent → slabý záujem o child (iba ak je špecifický) | `small_business` → `food_business` |
| `related_to` | Príbuzné témy | Záujem o A → slabší záujem o B (váha 0.3) | `food_business` ↔ `sustainability` |
| `supports` | Téma A podporuje tému B | Záujem o B → záujem o A (váha 0.4) | `local_farming` → `food_business` |
| `contradicts` | Protichodné témy | Záujem o A → zníženie záujmu o B (váha -0.2) | `vegan` ↔ `traditional_farming` |
| `evolves_from` | Vývojová nadväznosť | Záujem o A → záujem o B (váha 0.6) | `traditional_market` → `farmers_market` |

---

#### 4.1.4 Propagácia záujmu cez vzťahy (NOVÉ v v2)

Keď má používateľ záujem o tému X, systém **automaticky propaguje** tento záujem na súvisiace témy:

```
Interest(B) = Interest(A) * relation_weight
```

**Príklad propagácie:**

Používateľ má záujem o `food_business = 0.9`:

| Vzťah | Cieľová téma | Výpočet | Výsledný záujem |
|-------|--------------|---------|-----------------|
| `parent_of` | `small_business` | 0.9 × 0.5 | 0.45 |
| `related_to` | `sustainability` | 0.9 × 0.3 | 0.27 |
| `supports` | `local_farming` | 0.9 × 0.4 | 0.36 |
| `contradicts` | `industrial_farming` | 0.9 × (-0.2) | -0.18 (zníženie) |

> **Dôležité:** Propagácia je **lokálna** – každá Personal AI si udržiava vlastnú mapu vzťahov, ktorá sa môže líšiť od globálnej.

---

#### 4.1.5 Učenie vzťahov (NOVÉ v v2)

Personal AI sa učí nielen **čo** používateľa zaujíma, ale aj **aké vzťahy medzi témami preferuje**:

* Ak používateľ často prechádza z témy A na tému B, systém posilní vzťah `related_to`
* Ak používateľ explicitne označí dve témy ako nepríbuzné, systém oslabí vzťah
* Ak používateľ ignoruje témy, ktoré by podľa vzťahov mali byť relevantné, systém zníži váhu daného vzťahu

**Príklad učenia:**

```json
{
  "user_id": "user_miro_001",
  "learned_relations": {
    "food_business→sustainability": {
      "default_weight": 0.3,
      "user_weight": 0.7,  // používateľ silnejšie prepája tieto témy
      "confidence": 0.85
    },
    "vegan→traditional_farming": {
      "default_weight": -0.2,
      "user_weight": -0.5,  // používateľ silnejšie rozlišuje
      "confidence": 0.9
    }
  }
}
```

---

#### 4.1.6 Pôvodná jednoduchá reprezentácia (zachovaná pre kompatibilitu)

```json
{
  "food": 0.8,
  "business": 0.6,
  "politics": 0.2
}
```

> Táto jednoduchá reprezentácia je stále podporovaná, ale interne sa mapuje na štruktúrovaný model (kategória `food` bez podkategórií a tagov).

---

### 4.2 Capability Model (schopnosti)

Reprezentuje:

* čo vie používateľ robiť
* v čom vie prispieť

Príklad:

```json
{
  "construction": 0.7,
  "finance": 0.4,
  "design": 0.2
}
```

---

### 4.3 Preference Model (rozhodovacie preferencie)

Reprezentuje:

* ako používateľ rozmýšľa

Príklady dimenzií:

* `risk_tolerance`
* `long_term_vs_short_term`
* `individual_vs_collective`
* `argument_quality_weights` (prepojenie na ADR-025)
* `default_sorting` (quality / popularity / impact)

---

### 4.4 Behavioral Model (správanie)

Reprezentuje:

* ako sa používateľ reálne správa

Obsahuje:

* odpovede na otázky
* participáciu
* commitment
* ignorované témy
* **históriu propagácie záujmu (v2)**

👉 rozdiel medzi tým čo hovorí vs čo robí

---

### 4.5 Context Model (situácia)

Reprezentuje:

* kde je
* v akej situácii
* aké má obmedzenia

Príklady:

* lokalita
* čas
* dostupnosť

---

## 5. 🔄 Learning Mechanism

Personal AI sa učí z:

---

### 5.1 Explicit input

* odpovede na otázky
* **hodnotenie argumentov (v2)**
* **označovanie vzťahov medzi témami (v2)**

---

### 5.2 Implicit input

* kliky
* ignorovanie
* participácia
* **prechody medzi témami (v2)**

---

### 5.3 Behavioral feedback

* commitment vs realita
* **aké témy používateľ skutočne sleduje (v2)**

---

## 6. 🧠 Update Model

Každý input:

* mierne upravuje model
* neprepíše ho
* **aktualizuje propagované záujmy (v2)**

👉 model je:

* stabilný
* ale adaptívny

---

## 7. ⚖️ Identity vs Noise

Model musí:

* odolávať náhodným vstupom
* reflektovať dlhodobé správanie

👉 krátkodobý signál ≠ zmena identity

---

## 8. 🧠 Output použitia

Personal AI používa model na:

---

### 8.1 Question selection

čo ukázať

---

### 8.2 Participation filter

kde sa zapojiť

---

### 8.3 Project interpretation

čo je relevantné

---

### 8.4 Role suggestion

ako sa zapojiť

---

### 8.5 Interest propagation (v2)

* rozširovanie záujmu na súvisiace témy
* odporúčanie tém, ktoré používateľa môžu zaujímať

---

## 9. 🔒 Privacy

Model:

* je lokálny
* neopúšťa zariadenie
* nie je zdieľaný

Zdieľajú sa len:

* agregované signály
* **anonymizované propagácie záujmu (v2)**

---

## 10. ⚠️ Bias control

Model nesmie:

* uzatvárať používateľa
* vytvárať echo chamber

Musí obsahovať:

* exploration factor
* náhodné nové podnety
* **približne 10% obsahu mimo preferovaných tém (v2)**

---

## 11. 📊 Dôsledky

### ✅ Pozitíva

| Oblasť | Dôsledok |
|--------|----------|
| **Personalizácia** | lepšie cielený obsah |
| **Vyššia relevancia** | používateľ vidí, čo ho zaujíma |
| **Adaptivita** | model sa mení s používateľom |
| **Štruktúrovaný Interest Model (v2)** | záujmy nie sú ploché – rešpektujú hierarchiu a vzťahy |
| **Inteligentné šírenie (v2)** | používateľ objavuje súvisiace témy |
| **Učenie vzťahov (v2)** | model sa prispôsobuje spôsobu myslenia používateľa |

### ❗ Negatíva / Trade-offs

| Oblasť | Dôsledok |
|--------|----------|
| **Komplexita** | vyššia náročnosť implementácie |
| **Ladenie** | potreba kalibrácie váh |
| **Riziko nadmernej propagácie (v2)** | príliš veľa odvodených záujmov môže rozmazať profil |
| **Výpočtová náročnosť (v2)** | propagácia záujmu cez graf vyžaduje výpočty |

---

## 12. 🔗 Väzby

| ADR | Vzťah |
|-----|-------|
| ADR-015 | Participation Filter – Interest Model vstupom |
| ADR-019 | Opportunity Engine – Capability Model vstupom |
| ADR-025 | Argumentation – Preference Model (váhy kvality) |
| ADR-027 | View Layer – personalizácia pohľadu |
| ADR-028 | Support & Alignment – Behavioral Model vstupom |
| ADR-031 | Topic Layer – taxonómia pre Interest Model (v2) |
| ADR-032 | Questions – výber otázok podľa Interest Modelu |
| ADR-033 | Participation – Behavioral Model vstupom |
| **ADR-042** | **Unified Entity Model – Topic je typ Node (v2)** |

---

## 13. 📏 Pravidlá implementácie

Systém musí:

* udržiavať 5 vrstiev modelu (Interest, Capability, Preference, Behavioral, Context)
* učiť sa z explicitných aj implicitných vstupov
* byť stabilný (krátkodobý šum nemení identitu)
* obsahovať exploration factor
* **štruktúrovať Interest Model podľa taxonómie (kategórie + tagy) (v2)**
* **propagovať záujem cez sémantické vzťahy (v2)**
* **učiť sa preferované vzťahy medzi témami (v2)**
* **obmedziť propagáciu, aby nerozmazala profil (v2)**

---

## 14. 🔥 Príklady

### Príklad 1: Jednoduchý Interest Model (pôvodný)

```json
{
  "food": 0.8,
  "business": 0.6,
  "politics": 0.2
}
```

---

### Príklad 2: Štruktúrovaný Interest Model s kategóriami a tagmi (NOVÉ v v2)

```json
{
  "interest_model": {
    "categories": {
      "business": 0.6,
      "business.small_business": 0.7,
      "business.small_business.food_business": 0.9,
      "business.small_business.retail_business": 0.4,
      "business.corporate_business": 0.2,
      "technology": 0.5,
      "technology.opensource": 0.8,
      "sustainability": 0.7,
      "sustainability.local_food": 0.9
    },
    "tags": {
      "#sustainable": 0.8,
      "#community": 0.7,
      "#local": 0.6,
      "#organic": 0.4,
      "#vegan": 0.1,
      "#zerowaste": 0.3
    },
    "semantic_relations": {
      "preferred": ["related_to", "supports"],
      "avoided": ["contradicts"],
      "custom_weights": {
        "food_business→sustainability": 0.7,
        "vegan→traditional_farming": -0.5
      }
    }
  }
}
```

---

### Príklad 3: Propagácia záujmu (NOVÉ v v2)

```json
{
  "source_topic": "business.small_business.food_business",
  "source_interest": 0.9,
  "propagated_interests": [
    {
      "target_topic": "business.small_business",
      "relation": "parent_of",
      "weight": 0.5,
      "result": 0.45
    },
    {
      "target_topic": "sustainability",
      "relation": "related_to",
      "weight": 0.3,
      "result": 0.27
    },
    {
      "target_topic": "sustainability.local_food",
      "relation": "related_to",
      "weight": 0.35,
      "result": 0.315
    },
    {
      "target_topic": "technology.opensource",
      "relation": "related_to",
      "weight": 0.1,
      "result": 0.09
    },
    {
      "target_topic": "business.corporate_business",
      "relation": "contradicts",
      "weight": -0.2,
      "result": -0.18
    }
  ]
}
```

---

### Príklad 4: Kompletný Personal AI model (v2)

```json
{
  "user_id": "user_miro_001",
  "version": "2.0",
  "interest_model": {
    "categories": {
      "business.small_business.food_business": 0.9,
      "sustainability.local_food": 0.9,
      "technology.opensource": 0.8
    },
    "tags": {
      "#community": 0.7,
      "#local": 0.6
    }
  },
  "capability_model": {
    "finance": 0.8,
    "data_analysis": 0.7,
    "project_management": 0.6,
    "marketing": 0.3
  },
  "preference_model": {
    "risk_tolerance": 0.6,
    "long_term_vs_short_term": 0.7,
    "individual_vs_collective": 0.8,
    "argument_quality_weights": {
      "logical_consistency": 0.35,
      "relevance": 0.25,
      "factual_basis": 0.30,
      "structural_completeness": 0.05,
      "clarity": 0.05
    },
    "default_sorting": "quality"
  },
  "behavioral_model": {
    "questions_answered": 47,
    "arguments_rated": 123,
    "project_contributions": 5,
    "commitment_rate": 0.85,
    "exploration_rate": 0.12,
    "learned_relations": {
      "food_business→sustainability": 0.7,
      "vegan→traditional_farming": -0.5
    }
  },
  "context_model": {
    "location": "Trnava",
    "typical_availability": "evening",
    "quiet_hours": { "start": "22:00", "end": "08:00" }
  }
}
```

---

### Príklad 5: Ako Personal AI používa Interest Model na odporúčanie tém

```json
{
  "user_id": "user_miro_001",
  "available_topics": [
    { "topic": "business.small_business.food_business", "base_interest": 0.9 },
    { "topic": "sustainability.local_food", "base_interest": 0.9 },
    { "topic": "sustainability.zero_waste", "propagated_interest": 0.45 },
    { "topic": "technology.opensource", "base_interest": 0.8 },
    { "topic": "business.corporate_business", "propagated_interest": -0.18 }
  ],
  "selected_topics": [
    { "topic": "business.small_business.food_business", "reason": "high_base_interest" },
    { "topic": "sustainability.local_food", "reason": "high_base_interest" },
    { "topic": "sustainability.zero_waste", "reason": "propagated_from_local_food" },
    { "topic": "business.corporate_business", "reason": "exploration_factor", "exploration": true }
  ]
}
```

---

## 15. 🔥 Zhrnutie

> **Personal AI je dynamický model identity používateľa, ktorý sa učí z jeho správania a rozhodnutí a riadi jeho interakciu so systémom.**

**V2 prináša:**

* **Štruktúrovaný Interest Model** – kategórie (strom) + tagy (graf)
* **Sémantické vzťahy** – parent_of, related_to, supports, contradicts, evolves_from
* **Propagácia záujmu** – automatické rozširovanie na súvisiace témy
* **Učenie vzťahov** – Personal AI sa učí, ako používateľ prepája témy
* **Prepojenie na ADR-031 (Topic Layer)** – taxonómia je spoločná

---

## 16. 🧭 Finálna veta

> **Systém nepozná používateľa. Jeho Personal AI si ho postupne vytvára – nielen ako zoznam záujmov, ale ako bohatú sieť preferencií, schopností a vzťahov medzi témami.**

---

**Dátum:** 2026-04-15  
**Stav:** Accepted  
**Typ:** Core Model ADR (Personal AI Internal Model)  
**Verzia:** 2.0 (with Structured Interest Model & Taxonomy)

---

## 📝 Zoznam zmien oproti Version 1

| Sekcia | Zmena |
|--------|-------|
| 1. Status | Zmenené na v2 |
| 2. Kontext | Doplnený problém (plochý Interest Model, chýbajúca taxonómia) |
| 3. Rozhodnutie | Doplnené o štruktúrovaný Interest Model a taxonómiu |
| 4.1 | Rozšírený Interest Model |
| 4.1.1 | **Nová sekcia:** Interest Model a taxonómia (kategórie + tagy) |
| 4.1.2 | **Nová sekcia:** Štruktúrovaný Interest Model (príklad) |
| 4.1.3 | **Nová sekcia:** Sémantické vzťahy medzi témami |
| 4.1.4 | **Nová sekcia:** Propagácia záujmu cez vzťahy |
| 4.1.5 | **Nová sekcia:** Učenie vzťahov |
| 4.1.6 | Pôvodná jednoduchá reprezentácia (zachovaná) |
| 4.4 | Doplnené o históriu propagácie záujmu |
| 5.1 | Doplnené o hodnotenie argumentov a označovanie vzťahov |
| 5.2 | Doplnené o prechody medzi témami |
| 8.5 | **Nová sekcia:** Interest propagation |
| 9. | Doplnené o anonymizované propagácie záujmu |
| 10. | Doplnené o 10% exploration mimo preferovaných tém |
| 11. Pozitíva | Doplnené štruktúrovaný Interest Model, inteligentné šírenie, učenie vzťahov |
| 11. Negatíva | Doplnené riziko nadmernej propagácie, výpočtová náročnosť |
| 12. Väzby | Doplnené ADR-031, ADR-042 |
| 13. Implementácia | Doplnené pravidlá pre štruktúrovaný Interest Model, propagáciu, učenie vzťahov |
| 14.1 | Pôvodný príklad |
| 14.2 | **Nová sekcia:** Príklad 2 – Štruktúrovaný Interest Model |
| 14.3 | **Nová sekcia:** Príklad 3 – Propagácia záujmu |
| 14.4 | **Nová sekcia:** Príklad 4 – Kompletný Personal AI model (v2) |
| 14.5 | **Nová sekcia:** Príklad 5 – Ako Personal AI používa Interest Model na odporúčanie |
| 15. Zhrnutie | Doplnené v2 princípy |
| 16. Finálna veta | Rozšírená o bohatú sieť vzťahov medzi témami |