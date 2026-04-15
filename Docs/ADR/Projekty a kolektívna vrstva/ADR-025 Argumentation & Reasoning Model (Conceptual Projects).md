# 🧾 ADR-025: Argumentation & Reasoning Model (Hardened v4)

> **Zmeny oproti Version 3 (Hardened v3):**
> - **Doplnená časť 3.19:** Impact Metrics – meranie vplyvu argumentov na rozhodnutia
> - **Doplnená časť 3.20:** Tree Visualization – stromová vizualizácia argumentov
> - **Doplnená časť 3.21:** Decision Trail – sledovanie rozhodovacej cesty
> - **Rozšírená časť 3.9:** Impact Model – prepojenie na impact metrics
> - **Rozšírená časť 3.14:** View Layer – o vizualizáciu stromu a decision trail
> - **Rozšírená časť 6:** Väzby na ADR-027, ADR-044
> - **Nová sekcia 8.4 – 8.6:** Príklady pre impact metrics a tree visualization

---

## 1. 📌 Status

**Accepted (v4 – Hardened with Impact Metrics & Tree Visualization)**

---

## 2. 🎯 Kontext

Systém Personal AI Network:

* umožňuje vznik projektov (ADR-022)
* podporuje multi-variant vývoj (ADR-024)
* používa support model (ADR-028)
* filtruje participáciu (ADR-015)

Tieto mechanizmy dobre fungujú pre:

* realizačné projekty (napr. pizzéria)

### Problém

Pre **konceptuálne projekty** (napr. ústava, pravidlá, hodnoty):

* neexistuje jednoznačné „overenie realitou“
* commitment nestačí
* hlasovanie je manipulovateľné
* dominujú:

  * silné osobnosti
  * emócie
  * rétorika

👉 systém potrebuje inú vrstvu:

> **Argumentačnú vrstvu, ktorá nielen štrukturuje diskusiu, ale umožňuje, aby argumenty mali reálny dopad na vývoj projektu bez vzniku mocenských hierarchií.**

### Kľúčová otázka

👉 **Ako umožniť kvalitnú kolektívnu diskusiu a rozhodovanie bez dominancie ega, manipulácie a chaosu?**

### Dodatočný problém (v3)

Aj pri štruktúrovanej argumentácii hrozí riziko:

* **popularity bias** – populárny argument vyzerá ako „dobrý“
* **neoveriteľná kvalita** – nevieme rozlíšiť kvalitný argument od zlého
* **manipulácia davom** – veľa ľudí môže podporiť nekvalitný argument

👉 systém potrebuje **metriky kvality** a **oddelenie kvality od popularity**.

### Dodatočný problém (v4)

Aj pri kvalitných argumentoch vzniká problém:

* **netransparentný vplyv** – nevieme, ktorý argument skutočne ovplyvnil rozhodnutie
* **chýbajúca spätná väzba** – používateľ nevidí, prečo sa rozhodol tak, ako sa rozhodol
* **nemožnosť analýzy** – nedá sa zistiť, či rozhodnutie bolo racionálne

👉 systém potrebuje **metriky vplyvu** a **vizualizáciu rozhodovacej cesty**.

---

## 3. ⚖️ Rozhodnutie

### 🧠 3.1 Zavedenie „Argumentation Layer“

Systém zavádza:

👉 **Argumentation & Reasoning Layer**

Táto vrstva:

* štrukturuje diskusiu
* oddeľuje argument od osoby
* umožňuje porovnanie návrhov
* prepojuje argumenty na vetvy projektu
* **hodnotí kvalitu argumentov (v3)**
* **oddeľuje kvalitu od popularity (v3)**
* **meria vplyv argumentov na rozhodnutia (v4)**
* **poskytuje stromovú vizualizáciu argumentov (v4)**

---

### 🧩 3.2 Argument ako základná jednotka

Základná entita nie je:

❌ komentár
❌ názor

👉 ale:

👉 **argument**

---

### 🧾 3.3 Štruktúra argumentu

Každý argument obsahuje:

| Komponent | Popis |
|-----------|-------|
| **Claim (tvrdenie)** | čo tvrdím |
| **Reasoning (odôvodnenie)** | prečo to tvrdím |
| **Assumptions (predpoklady)** | na čom to stojí |
| **Implications (dôsledky)** | čo to spôsobí |
| **Evidence (dôkazy)** | čím to podkladám (nepovinné) |
| **Influenced Decisions (v4)** | zoznam rozhodnutí, ktoré tento argument ovplyvnil |
| **Impact Score (v4)** | miera vplyvu na vývoj projektu |

👉 bez tejto štruktúry argument nemá plnú váhu.

---

### ⚔️ 3.4 Povinná existencia protiargumentu

Každý významný návrh musí mať:

👉 **counter-argument**

To znamená:

* systém aktívne generuje opozíciu
* alebo ju vyžaduje

👉 cieľ: vyvážiť bias

---

### 🧠 3.5 Oddelenie osoby od argumentu

Systém NESMIE:

* zvýhodňovať autora argumentu
* zvýhodňovať popularitu

👉 hodnotí sa **argument, nie človek**

> Hodnotenie argumentu je úplne oddelené od identity autora. Tento princíp je základom pre elimináciu mocenského biasu (ADR-038).

---

### 🧠 3.6 Argument graph

Argumenty tvoria:

👉 **graph (sieť vzťahov)**

Obsahuje:

* podporujúce argumenty
* protiargumenty
* alternatívne pohľady

👉 projekt nie je lineárna diskusia, ale **mapa argumentov**

> Argument graph nie je len vizualizácia diskusie, ale primárna štruktúra, z ktorej sa odvodzuje vývoj projektu.

---

### ⚖️ 3.7 Hodnotenie argumentov

Argument sa hodnotí podľa:

| Kritérium | Popis | Hodnotí |
|-----------|-------|---------|
| **Koherencia** | je logicky konzistentný | AI + užívatelia |
| **Pokrytie** | zohľadňuje viac aspektov | AI + užívatelia |
| **Robustnosť** | odoláva protiargumentom | AI (analýza grafu) |
| **Praktické dôsledky** | je aplikovateľný | užívatelia |
| **Relevantnosť** | týka sa priamo témy | AI + užívatelia |
| **Podloženosť faktami** | má evidenciu alebo dáta | AI + užívatelia |

👉 NIE podľa počtu hlasov.

> Hodnotenie slúži na orientáciu v kvalite argumentov, nie na vytváranie hierarchie medzi používateľmi.

---

### 🤖 3.16 Metriky kvality argumentu

Systém používa **kombinované hodnotenie kvality** z dvoch zdrojov:

#### A. Automatické hodnotenie (lokálna AI)

Personal AI používateľa **lokálne** vyhodnocuje každý argument podľa:

| Metrika | Popis | Spôsob výpočtu |
|---------|-------|----------------|
| **Logická konzistencia** | argument si neprotirečí | analýza textu (nerozporuje si claim a reasoning) |
| **Relevantnosť k téme** | argument sa týka diskutovanej témy | embedding podobnosť s topicom projektu |
| **Faktická podloženosť** | argument obsahuje overiteľné tvrdenia | detekcia faktických tvrdení (bez overenia pravdy) |
| **Štrukturálna úplnosť** | argument má všetky časti (claim, reasoning, assumptions) | kontrola štruktúry |
| **Jasnosť** | argument je zrozumiteľný | analýza zložitosti viet, nejednoznačností |

**Dôležité:** Automatické hodnotenie je **lokálne** – každý používateľ má svoju AI, ktorá hodnotí podľa jeho vlastných kritérií. Neexistuje centrálna „pravda“ o kvalite argumentu.

#### B. Užívateľské hodnotenie

Používatelia môžu argumenty hodnotiť manuálne. Tieto hodnotenia sú tiež lokálne a synchronizujú sa len v rámci Trusted Circles (ADR-004).

#### C. Kombinované skóre kvality

Pre každého používateľa existuje **lokálne kombinované skóre** argumentu:

```
quality_score = w1 * AI_logical_consistency +
                w2 * AI_relevance +
                w3 * AI_factual_basis +
                w4 * AI_structural_completeness +
                w5 * AI_clarity +
                w6 * user_rating_aggregate
```

Váhy `w1` až `w6` sú súčasťou **Preference Modelu** používateľa (ADR-034) – každý si môže nastaviť, čo je preňho dôležité.

---

### 👥 3.17 Užívateľské hodnotenie argumentov

Používatelia môžu k argumentu pridať **štruktúrovanú spätnú väzbu**:

| Typ hodnotenia | Význam | Vplyv na kvalitu |
|----------------|--------|------------------|
| `agree` | súhlasím s argumentom | + (pozitívny signál) |
| `disagree` | nesúhlasím | - (negatívny signál, ale neznižuje kvalitu – len alternatívny pohľad) |
| `clarify` | argument je nejasný, potrebuje vysvetlenie | 0 (žiadny vplyv, ale upozornenie) |
| `off-topic` | argument nesúvisí s témou | -- (znižuje relevance skóre) |
| `insightful` | argument je mimoriadne hodnotný, otvára nový pohľad | ++ (výrazne zvyšuje kvalitu) |
| `evidence_request` | žiadam o dôkazy / podklady | 0 (signal, že chýba faktická podloženosť) |

**Dôležité pravidlá:**

1. **Hodnotenie je oddelené od identity autora** – hodnotí sa argument, nie kto ho napísal.
2. **Hodnotenie nie je hlasovanie** – `agree` neznamená „hlasujem za“, ale „považujem to za kvalitný argument“.
3. **Každý používateľ má vlastné hodnotenia** – nie sú globálne.
4. **Hodnotenia sa synchronizujú len v rámci Trusted Circles** – žiadne globálne skóre.

---

### 🎯 3.18 Oddelenie kvality od popularity

**Princíp:**

> **Populárny argument nemusí byť kvalitný. Kvalitný argument nemusí byť populárny. Systém musí rozlišovať tieto dve dimenzie.**

Systém sleduje dve **nezávislé metriky** pre každý argument:

| Metrika | Popis | Z čoho sa počíta |
|---------|-------|------------------|
| **Quality Score** | aká je kvalita argumentu | AI metriky + užívateľské `insightful` / `clarify` / `off-topic` |
| **Popularity Score** | koľko ľudí s argumentom súhlasí | počet `agree` (bez váhy) |

**Prečo je to dôležité:**

* Kvalitný, ale nepopulárny argument môže byť prelomový.
* Populárny, ale nekvalitný argument môže byť manipulácia davom.
* Používateľ musí vidieť obe dimenzie, nie len jednu.

**Použitie v View Layer (ADR-027):**

Používateľ si môže vybrať, ako chce argumenty zoradiť:

* podľa kvality (default pre konceptuálne projekty)
* podľa popularity (voliteľné)
* podľa kombinácie (vlastné váhy)

**Prepojenie na ADR-003 (Non-Manipulation):**

Systém **NESMIE**:

* predvolene zoraďovať len podľa popularity
* skrývať kvalitné, ale nepopulárne argumenty
* prezentovať popularitu ako indikátor kvality

Systém **MÔŽE**:

* zvýrazniť kvalitné argumenty (bez ohľadu na popularitu)
* umožniť používateľovi vidieť „skryté drahokamy“ (vysoká kvalita, nízka popularita)

---

### 📊 3.19 Impact Metrics – meranie vplyvu argumentov na rozhodnutia (NOVÉ v v4)

**Princíp:**

> **Každý argument by mal mať merateľný vplyv na vývoj projektu. Tento vplyv musí byť transparentný a auditovateľný.**

#### A. Definícia Impact Score

Každý argument má `impact_score` – číslo v rozsahu `-1.0` (maximálny negatívny vplyv) až `+1.0` (maximálny pozitívny vplyv), kde `0` znamená žiadny merateľný vplyv.

Impact score sa počíta z:

| Komponent | Popis | Váha |
|-----------|-------|------|
| **Support Shift** | o koľko sa zmenila podpora (ADR-028) pre vetvy po zverejnení argumentu | 0.4 |
| **Branch Creation** | či argument viedol k vytvoreniu novej vetvy | 0.2 |
| **Branch Merger** | či argument viedol k zlúčeniu vetiev | 0.2 |
| **Decision Reference** | koľko rozhodnutí odkazuje na tento argument | 0.1 |
| **User Attribution** | koľko používateľov explicitne uviedlo, že argument ovplyvnil ich rozhodnutie | 0.1 |

#### B. Záznam ovplyvnených rozhodnutí

Každý argument obsahuje `influenced_decisions` – zoznam rozhodnutí, ktoré ovplyvnil:

```json
{
  "influenced_decisions": [
    {
      "decision_id": "dec_001",
      "decision_type": "branch_selection",
      "timestamp": "2026-04-15T14:00:00Z",
      "impact_direction": "positive",
      "impact_magnitude": 0.7,
      "context": "Používateľ si vybral vetvu A na základe tohto argumentu"
    }
  ]
}
```

#### C. Sledovanie zmien v Support & Alignment

Systém automaticky sleduje:

* **Pred argumentom:** Aká bola podpora pre jednotlivé vetvy?
* **Po argumente:** Ako sa podpora zmenila?

Zmenu podpory (support shift) možno priamo priradiť argumentu, ak:

1. Argument bol zverejnený.
2. Podpora sa zmenila v krátkom časovom okne (napr. 48 hodín).
3. Neexistuje iná zjavná príčina zmeny.

> **Upozornenie:** Korelácia nie je kauzalita. Systém poskytuje **návrh vplyvu**, nie definitívnu pravdu. Používateľ môže explicitne potvrdiť alebo odmietnuť, že argument ovplyvnil jeho rozhodnutie.

#### D. Explicitné potvrdenie vplyvu

Používateľ môže k argumentu pridať:

```json
{
  "impact_attestation": {
    "user_id": "user_miro_001",
    "argument_id": "arg_001",
    "influenced_my_decision": true,
    "decision_id": "dec_001",
    "confidence": 0.9,
    "note": "Tento argument mi ukázal, prečo je lokálna cesta lepšia"
  }
}
```

---

### 🌳 3.20 Tree Visualization – stromová vizualizácia argumentov (NOVÉ v v4)

**Princíp:**

> **Argumenty nie sú lineárny zoznam, ale strom. Používateľ musí vidieť hierarchiu a vzťahy medzi argumentmi.**

#### A. Typy vizualizácie

Systém podporuje tri typy stromovej vizualizácie:

| Typ | Popis | Kedy použiť |
|-----|-------|-------------|
| **Argument Tree** | Hierarchická štruktúra argumentov (hlavný argument → podporné → proti) | Prehľad diskusie |
| **Decision Tree** | Ako argumenty viedli ku konkrétnemu rozhodnutiu | Audit rozhodnutia |
| **Impact Tree** | Ktoré argumenty mali najväčší vplyv na vývoj projektu | Analýza vplyvu |

#### B. Argument Tree štruktúra

```
📋 Hlavná téma: "Ako financovať pizzériu?"
│
├── 🌿 Vetva A: "Klasický úver"
│   ├── ✅ Podporný argument: "Úver je rýchly" (Q=0.85, I=0.6)
│   │   └── ❌ Protiargument: "Úver je drahý" (Q=0.78, I=0.5)
│   └── ✅ Podporný argument: "Nezávislosť od investorov" (Q=0.82, I=0.4)
│
├── 🌿 Vetva B: "Crowdfunding"
│   ├── ✅ Podporný argument: "Buduje komunitu" (Q=0.91, I=0.8) ⭐
│   ├── ❌ Protiargument: "Časovo náročné" (Q=0.65, I=0.2)
│   └── ❌ Protiargument: "Neistý výsledok" (Q=0.70, I=0.3)
│
└── 🌿 Vetva C: "Investor"
    ├── ✅ Podporný argument: "Rýchly kapitál" (Q=0.75, I=0.5)
    └── ❌ Protiargument: "Strata kontroly" (Q=0.88, I=0.7)
```

#### C. Vážené zobrazenie

Každý argument v strome má:

* **Q (Quality Score)** – kvalita argumentu (0-1)
* **I (Impact Score)** – vplyv na projekt (-1 až +1)
* **Podpora** – počet používateľov, ktorí súhlasia (voliteľné)

Vetvy môžu mať **agregované skóre**:

```
Vetva A (Úver):     Q=0.72 | I=0.45 | Support=34
Vetva B (Crowdfunding): Q=0.85 | I=0.65 | Support=67  ← Odporúčané
Vetva C (Investor): Q=0.68 | I=0.30 | Support=23
```

#### D. Interaktívna navigácia

Používateľ môže:

* **Rozbaliť/Zbaliť** – skryť detailné argumenty
* **Filtrovať** – zobraziť len argumenty s Q > 0.7
* **Zvýrazniť** – argumenty, ktoré ovplyvnili jeho rozhodnutia
* **Porovnať** – dve vetvy vedľa seba

---

### 🗺️ 3.21 Decision Trail – sledovanie rozhodovacej cesty (NOVÉ v v4)

**Princíp:**

> **Každé rozhodnutie používateľa by malo byť spätne dohľadateľné k argumentom, ktoré ho ovplyvnili.**

#### A. Štruktúra Decision Trail

Pre každé dôležité rozhodnutie (výber vetvy, zmena supportu, commit) systém zaznamenáva:

```json
{
  "decision_id": "dec_001",
  "user_id": "user_miro_001",
  "project_id": "proj_pizzeria_001",
  "decision_type": "branch_selection",
  "selected_branch": "branch_crowdfunding_001",
  "timestamp": "2026-04-15T15:30:00Z",
  "influencing_arguments": [
    {
      "argument_id": "arg_003",
      "influence_weight": 0.8,
      "attestation": "explicit"  // user explicitly said this influenced them
    },
    {
      "argument_id": "arg_007",
      "influence_weight": 0.4,
      "attestation": "inferred"  // system inferred based on behavior
    }
  ],
  "alternative_branches_considered": ["branch_loan_001", "branch_investor_001"],
  "final_confidence": 0.85
}
```

#### B. Vizualizácia Decision Trail

Používateľ vidí svoju rozhodovaciu cestu:

```
Tvoja rozhodovacia cesta pre projekt "Komunitná pizzéria"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

15.4. 10:00  ⚖️  Začiatok: 3 vetvy (Úver, Crowdfunding, Investor)
              📊 Tvoja podpora: Úver 60% | CF 30% | Inv 10%

15.4. 14:30  📖  Čítal si argument "Buduje komunitu" (Q=0.91)
              ✅ Označil si ho ako "insightful"
              📊 Podpora: Úver 50% | CF 50% | Inv 0%

15.4. 16:00  📖  Čítal si argument "Strata kontroly pri investorovi" (Q=0.88)
              ✅ Označil si "ovplyvnilo moje rozhodnutie"
              📊 Podpora: Úver 40% | CF 60% | Inv 0%

15.4. 18:00  ✅  KONEČNÉ ROZHODNUTIE: Vetva "Crowdfunding"
              🔗 Ovplyvnené argumentmi: "Buduje komunitu", "Strata kontroly"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
💡 Spätná väzba: Tvoje rozhodnutie bolo ovplyvnené 2 kvalitnými argumentmi.
   Žiadny z argumentov nebol populárny (priemer P=0.35), ale mali vysokú kvalitu.
```

#### C. Kolektívny Decision Trail

Pre projekt ako celok môže byť zobrazený **kolektívny decision trail**:

* Ako sa menila podpora pre jednotlivé vetvy v čase
* Ktoré argumenty spôsobili najväčšie zmeny
* Kde sa komunita rozdelila (fork points)

---

### 🧠 3.8 Úloha AI

AI:

* extrahuje argumenty z textu
* štrukturuje ich
* odstraňuje emócie
* generuje protiargumenty
* sumarizuje diskusiu
* **hodnotí logickú konzistenciu**
* **hodnotí relevantnosť a faktickú podloženosť**
* **odhaduje impact score na základe zmien v supporte (v4)**
* **generuje stromovú vizualizáciu (v4)**

AI NESMIE:

* rozhodovať
* určovať „pravdu“
* **skrývať kvalitné argumenty kvôli nízkej popularite**
* **manipulovať impact score v prospech niektorých argumentov (v4)**

---

### 🔥 3.9 Argument → Impact Model (Rozšírené v v4)

Argumenty nemajú len existovať.

👉 musia mať **dopad na realitu projektu**

**Princíp:**

> **Argument ovplyvňuje vývoj projektu iba nepriamo – cez štruktúru, nie cez autoritu.**

**To znamená:**

Argument môže:

* posilniť vetvu (branch)
* oslabiť vetvu
* vytvoriť novú vetvu
* zmeniť smer diskusie

❌ **nikdy:**

* nepridáva „váhu hlasu“
* nepridáva autoritu používateľovi
* neovplyvňuje systém cez identitu autora
* **nepridáva váhu na základe popularity**

👉 **Kvalita argumentu ovplyvňuje jeho impact, nie popularita.**

**V4 rozšírenie:** Impact je merateľný a vizualizovateľný cez `impact_score` a `influenced_decisions`.

---

### 🧠 3.10 Vzťah k vetvám (Branching)

Každá vetva projektu:

* je podložená konkrétnymi argumentmi
* obsahuje vlastný argument graph

👉 vetva prežíva, ak:

* má robustné argumenty (vysoká kvalita)
* prechádza protiargumentmi

👉 vetva zaniká (ADR-029), ak:

* nemá podporu
* alebo jej argumenty neobstoja (nízka kvalita)

**Prepojenie na ADR-028 (Support & Alignment):**

Kvalita argumentov ovplyvňuje **alignment** – používateľ, ktorý vidí kvalitné argumenty pre vetvu A a nekvalitné pre vetvu B, prirodzene zmení svoj support.

Nie je to automatické – ale systém ukazuje súvislosť medzi kvalitou argumentov a podporou.

**V4 rozšírenie:** Impact metrics umožňujú kvantifikovať, ako veľmi argumenty zmenili support.

---

### 🧠 3.11 Argument ≠ rozhodnutie

Systém:

❌ nikdy nevyberá „víťazný argument“

👉 poskytuje:

* mapu možností
* dôsledky
* konflikty
* **kvalitu jednotlivých argumentov**
* **impact metrics (v4)**
* **decision trail (v4)**

👉 rozhoduje **človek** (ADR-002)

---

### 🧠 3.12 Ochrana pred mocou (prepojenie na ADR-038)

Argumentation Layer je navrhnutá tak, aby:

* minimalizovala vplyv silných osobností
* zabránila dominancii hlasných používateľov
* oddelila kvalitu argumentu od jeho prezentácie
* **zabránila dominancii populárnych, ale nekvalitných argumentov**
* **zabránila manipulácii impact metrics (v4)**

> Argument môže získať vplyv len tým, že obstojí v konfrontácii s inými argumentmi – nie tým, kto ho vyslovil alebo koľko ľudí s ním súhlasí.

---

### 🔄 3.13 Výstup projektu

Výsledok je:

👉 **structured reasoning output + branch landscape + quality assessment + impact analysis (v4)**

Obsahuje:

* hlavné vetvy
* argumenty pre každú vetvu (s kvalitou)
* protiargumenty (s kvalitou)
* dôsledky
* konflikty medzi vetvami
* **argumenty s najvyššou kvalitou (aj keď nepopulárne)**
* **argumenty s najvyšším impactom (v4)**
* **decision trail pre každého účastníka (v4)**

---

### 🧠 3.14 Prepojenie na View Layer (Rozšírené v v4)

Používateľ vidí:

* zjednodušené argumenty
* hlavné konflikty
* kľúčové rozhodovacie body
* **kvalitu argumentov (zobrazenú oddelene od popularity)**
* **možnosť prepínať medzi „podľa kvality“ a „podľa popularity“**
* **stromovú vizualizáciu argumentov (v4)**
* **svoju rozhodovaciu cestu (decision trail) (v4)**
* **argumenty, ktoré ovplyvnili jeho rozhodnutia (v4)**

👉 nie celý graph (pokiaľ si ho používateľ explicitne nerozbali).

---

### 🧠 3.15 Governance napojenie

Argumentation Layer:

* dopĺňa Support Model (ADR-028)
* dopĺňa Branching (ADR-024)
* je základom pre Power balancing (ADR-038)
* **poskytuje vstup pre alignment cez kvalitu argumentov**
* **poskytuje auditovateľný záznam rozhodnutí (v4)**

> Používa sa hlavne pre conceptual / normative projekty.

---

## 4. 🧠 Dôsledky rozhodnutia

### ✅ Pozitívne

| Oblasť | Dôsledok |
|--------|----------|
| **Kvalitnejšie myslenie** | menej emócií, viac logiky |
| **Ochrana pred manipuláciou** | argument > ego, kvalita > popularita |
| **Škálovateľnosť diskusie** | veľké témy zvládnuteľné |
| **Vyváženie moci** | argument > autor, vplyv nevzniká z aktivity, ale z kvality reasoning |
| **Ochrana pred davom** | populárny ≠ dobrý, systém to nezamlčuje |
| **Transparentnosť hodnotenia** | používateľ vidí, prečo je argument považovaný za kvalitný |
| **Personalizácia kvality** | každý používateľ má vlastné váhy pre metriky kvality |
| **Spätná väzba rozhodnutí (v4)** | používateľ vidí, prečo sa rozhodol tak, ako sa rozhodol |
| **Auditovateľnosť (v4)** | každé rozhodnutie je dohľadateľné k argumentom |
| **Vzdelávací efekt (v4)** | používateľ sa učí z vlastnej rozhodovacej cesty |

### ❗ Negatívne / Trade-offs

| Oblasť | Dôsledok |
|--------|----------|
| **Komplexita** | vyšší vstupný náklad |
| **Kognitívna náročnosť** | vyžaduje dobrý UX (ADR-027) |
| **Výpočtová komplexita** | argument graph môže rásť exponenciálne, plus metriky kvality a impactu |
| **Riziko „falošnej objektivity“** | AI metriky nie sú dokonalé, môžu byť biasované |
| **Lokálna výpočtová náročnosť** | hodnotenie kvality a impactu beží na zariadení používateľa |
| **Kauzalita vs korelácia (v4)** | impact score je odhad, nie absolútna pravda |
| **Možná manipulácia impactu (v4)** | používateľ môže falošne tvrdiť, že argument ovplyvnil jeho rozhodnutie |

---

## 5. 🚫 Alternatívy (zamietnuté)

### ❌ Voľná diskusia (komentáre)

* jednoduchá
* ale: chaotická, manipulovateľná

### ❌ Hlasovanie

* intuitívne
* ale: skreslené, náchylné na dav

### ❌ Čistá AI evaluácia bez užívateľského vstupu

* konzistentná
* ale: AI môže mať vlastný bias, ignoruje kontext

### ❌ Iba popularita bez kvality

* jednoduché
* ale: manipulovateľné davom, porušuje ADR-003

### ❌ Žiadne meranie impactu (v4)

* jednoduchšie
* ale: používateľ nevidí, čo ovplyvnilo jeho rozhodnutia

### ❌ Len lineárny zoznam argumentov (v4)

* jednoduchšie
* ale: stráca sa hierarchia a vzťahy

---

## 6. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-002 | Human-in-the-loop – rozhoduje človek, nie systém |
| ADR-003 | Non-Manipulation – systém nesmie uprednostňovať populárne pred kvalitnými |
| ADR-004 | Privacy-first – hodnotenia sú lokálne, synchronizujú sa len v Trusted Circles |
| ADR-011 | Personalization – váhy kvality sú súčasťou Preference Modelu |
| ADR-015 | Participation Filter – filtruje relevantných účastníkov |
| ADR-024 | Branching – vetvy projektu sú podložené argumentmi |
| ADR-027 | View Layer – zjednodušený pohľad pre používateľa, **stromová vizualizácia (v4)** |
| ADR-028 | Support Model – kvalita argumentov ovplyvňuje alignment, **impact metrics sledujú zmeny supportu (v4)** |
| ADR-029 | Branch Lifecycle – vetvy zanikajú na základe kvality argumentov |
| ADR-030 | Sync & Persistence – argument graph a hodnotenia musia byť perzistentné |
| ADR-034 | Personal AI Internal Model – váhy pre metriky kvality sú v Preference Modeli |
| ADR-038 | Power & Influence Balancing – argumentation layer je nástrojom na elimináciu moci |
| ADR-039 | Truth & Epistemic Model – kvalita argumentov je vstupom pre epistemické hodnotenie |
| ADR-040 | Graceful Degradation – nízka kvalita vstupov vedie k nižšej kvalite hodnotení |
| **ADR-042** | **Unified Entity Model – Argument je typ Node** |
| **ADR-044** | **Impact Visualization (ak vznikne ako samostatné ADR)** |

---

## 7. 📏 Pravidlá implementácie (high-level)

Systém musí:

* umožniť štruktúrované argumenty
* podporovať protiargumenty
* vytvárať argument graph
* oddeliť autora od obsahu
* prepojiť argumenty na vetvy projektu
* zabezpečiť, že argument neovplyvňuje systém cez identitu autora
* prepojiť s AI summarization
* **automaticky hodnotiť logickú konzistenciu, relevantnosť a podloženosť (lokálne)**
* **umožniť užívateľské hodnotenie (agree, disagree, clarify, off-topic, insightful, evidence_request)**
* **oddeliť quality score od popularity score**
* **zobraziť obe dimenzie v View Layer**
* **nikdy neskrývať kvalitné argumenty kvôli nízkej popularite**
* **prepojiť kvalitu argumentov s alignmentom (ADR-028)**
* **sledovať impact argumentov na základe zmien v supporte (v4)**
* **umožniť používateľom explicitne potvrdiť, ktoré argumenty ovplyvnili ich rozhodnutie (v4)**
* **poskytnúť stromovú vizualizáciu argumentov (v4)**
* **poskytnúť decision trail pre každého používateľa (v4)**
* **zaznamenávať každé dôležité rozhodnutie do audit logu (v4)**

---

## 8. 🔥 Príklady

### Príklad 1: Argument s hodnotením kvality

```json
{
  "argument_id": "arg_001",
  "claim": "Pizzéria by mala používať lokálne suroviny",
  "reasoning": "Lokálne suroviny znižujú uhlíkovú stopu a podporujú miestnych farmárov",
  "assumptions": ["Lokálni farmári dokážu pokryť dopyt", "Zákazníci ocenia ekologický prístup"],
  "implications": ["Vyššie náklady na suroviny", "Lepšia marketingová pozícia"],
  "evidence": ["Štúdia X ukazuje, že 70% zákazníkov uprednostňuje lokálne suroviny"],
  
  "quality_metrics": {
    "logical_consistency": 0.92,
    "relevance": 0.95,
    "factual_basis": 0.78,
    "structural_completeness": 1.0,
    "clarity": 0.88
  },
  
  "user_ratings": {
    "agree": 45,
    "disagree": 12,
    "clarify": 3,
    "off_topic": 1,
    "insightful": 8,
    "evidence_request": 2
  },
  
  "quality_score": 0.87,
  "popularity_score": 0.79,
  
  "impact_metrics": {  // NOVÉ v v4
    "impact_score": 0.72,
    "influenced_decisions": ["dec_001", "dec_003"],
    "support_shift": +0.15,
    "branch_created": false,
    "branch_merged": false
  }
}
```

### Príklad 2: View Layer – zobrazenie argumentov

```
--- Argumenty pre vetvu "Lokálne suroviny" ---

Zoradené podľa: [Kvality ▼]  [Popularity]  [Kombinované]  [Impactu ▼]

1. 🧠 Q=0.92 | P=0.45 | I=0.72
   "Pizzéria by mala používať lokálne suroviny"
   ✅ Logicky konzistentné | 📊 Podložené štúdiou | 💥 Vysoký impact
   [8 people found this insightful]
   [Ovplyvnilo 2 rozhodnutia]

2. 🧠 Q=0.88 | P=0.82 | I=0.45
   "Lokálne suroviny sú príliš drahé"
   ⚠️ Vyžaduje evidenciu | 2 ľudia žiadajú podklady

3. 🧠 Q=0.45 | P=0.91 | I=0.12
   "Moja sesternica má pizzériu a nepoužíva lokálne"
   ❌ Nízka relevantnosť | Anecdotálne, nie všeobecné

--- Skryté drahokamy (vysoká kvalita, nízka popularita) ---
👉 "Lokálne suroviny vytvárajú komunitný pocit" (Q=0.89, P=0.12, I=0.68)
```

### Príklad 3: Lokálne nastavenie váh kvality

```json
{
  "user_id": "user_miro_001",
  "preferences": {
    "argument_quality_weights": {
      "logical_consistency": 0.35,
      "relevance": 0.25,
      "factual_basis": 0.30,
      "structural_completeness": 0.05,
      "clarity": 0.05
    },
    "default_sorting": "quality",
    "show_unpopular_quality_arguments": true,
    "impact_weights": {  // NOVÉ v v4
      "support_shift": 0.4,
      "branch_creation": 0.2,
      "branch_merger": 0.2,
      "decision_reference": 0.1,
      "user_attestation": 0.1
    }
  }
}
```

### Príklad 4: Tree Visualization (NOVÉ v v4)

```json
{
  "tree_id": "tree_proj_pizzeria_001",
  "root_topic": "Ako financovať pizzériu?",
  "branches": [
    {
      "branch_id": "branch_loan",
      "name": "Klasický úver",
      "aggregated_quality": 0.72,
      "aggregated_impact": 0.45,
      "support_count": 34,
      "arguments": [
        {
          "argument_id": "arg_loan_001",
          "claim": "Úver je rýchly",
          "quality": 0.85,
          "impact": 0.60,
          "stance": "support"
        },
        {
          "argument_id": "arg_loan_002",
          "claim": "Úver je drahý",
          "quality": 0.78,
          "impact": -0.50,
          "stance": "oppose"
        }
      ]
    },
    {
      "branch_id": "branch_crowdfunding",
      "name": "Crowdfunding",
      "aggregated_quality": 0.85,
      "aggregated_impact": 0.65,
      "support_count": 67,
      "arguments": [
        {
          "argument_id": "arg_cf_001",
          "claim": "Buduje komunitu",
          "quality": 0.91,
          "impact": 0.80,
          "stance": "support",
          "highlight": true
        },
        {
          "argument_id": "arg_cf_002",
          "claim": "Časovo náročné",
          "quality": 0.65,
          "impact": -0.20,
          "stance": "oppose"
        }
      ]
    }
  ]
}
```

### Príklad 5: Decision Trail (NOVÉ v v4)

```json
{
  "user_id": "user_miro_001",
  "project_id": "proj_pizzeria_001",
  "decisions": [
    {
      "decision_id": "dec_001",
      "timestamp": "2026-04-15T15:30:00Z",
      "decision_type": "branch_selection",
      "selected_branch": "branch_crowdfunding",
      "confidence": 0.85,
      "influencing_arguments": [
        {
          "argument_id": "arg_cf_001",
          "claim": "Buduje komunitu",
          "influence_weight": 0.8,
          "attestation": "explicit"
        },
        {
          "argument_id": "arg_investor_002",
          "claim": "Strata kontroly pri investorovi",
          "influence_weight": 0.6,
          "attestation": "explicit"
        }
      ],
      "alternative_branches_considered": ["branch_loan", "branch_investor"]
    }
  ],
  "visualization": {
    "type": "timeline",
    "events": [
      {"time": "2026-04-15T10:00:00", "event": "start", "support": {"loan": 0.6, "cf": 0.3, "investor": 0.1}},
      {"time": "2026-04-15T14:30:00", "event": "read_argument", "argument": "Buduje komunitu", "new_support": {"loan": 0.5, "cf": 0.5, "investor": 0.0}},
      {"time": "2026-04-15T16:00:00", "event": "read_argument", "argument": "Strata kontroly", "new_support": {"loan": 0.4, "cf": 0.6, "investor": 0.0}},
      {"time": "2026-04-15T18:00:00", "event": "decision", "selected": "cf"}
    ]
  }
}
```

---

## 9. 🔥 Zhrnutie

> **Argumenty nie sú len diskusia – sú mechanizmus, ktorým sa mení štruktúra projektu bez toho, aby vznikala mocenská hierarchia.**

Konceptuálne projekty sú riadené **kvalitou argumentov**, nie hlasovaním ani silou osobností.

Argument môže posilniť, oslabiť alebo vytvoriť vetvu – ale nikdy nepridáva váhu hlasu svojmu autorovi.

**V3 priniesla:**

* **Metriky kvality** – logická konzistencia, relevantnosť, podloženosť
* **Užívateľské hodnotenie** – agree, disagree, clarify, off-topic, insightful
* **Oddelenie kvality od popularity** – populárny ≠ kvalitný

**V4 prináša:**

* **Impact Metrics** – meranie vplyvu argumentov na rozhodnutia
* **Tree Visualization** – stromová vizualizácia argumentov a vetiev
* **Decision Trail** – sledovanie rozhodovacej cesty každého používateľa
* **Prepojenie na Support & Alignment** – impact sledovaný cez zmeny v podpore

---

## 10. 🧭 Finálna veta

> **V Synergetiku nemá vplyv ten, kto hovorí – ale to, čo prežije konfrontáciu s inými myšlienkami. A ani popularita nerobí argument pravdou – len kvalita rozhoduje o jeho sile. A každé rozhodnutie je spätne dohľadateľné k argumentom, ktoré ho ovplyvnili.**

V komplexných problémoch nevyhráva ten, kto kričí najhlasnejšie – ale ten, koho argument prežije najviac otázok. A nevyhráva ani ten, koho argument má najviac lajkov. A každý používateľ vidí, prečo sa rozhodol tak, ako sa rozhodol.

---

**Dátum:** 2026-04-15  
**Stav:** Accepted  
**Typ:** Core Mechanism ADR (Conceptual Projects)  
**Verzia:** 4.0 (Hardened with Impact Metrics & Tree Visualization)

---

## 📝 Zoznam zmien oproti Version 3

| Sekcia | Zmena |
|--------|-------|
| 1. Status | Zmenené na v4 |
| 2. Kontext | Doplnený dodatočný problém (netransparentný vplyv, chýbajúca spätná väzba) |
| 3.1 | Doplnené meranie vplyvu a stromová vizualizácia |
| 3.3 | Doplnené `Influenced Decisions` a `Impact Score` do štruktúry argumentu |
| 3.19 | **Nová sekcia:** Impact Metrics – meranie vplyvu argumentov |
| 3.20 | **Nová sekcia:** Tree Visualization – stromová vizualizácia |
| 3.21 | **Nová sekcia:** Decision Trail – sledovanie rozhodovacej cesty |
| 3.8 | Rozšírené úlohy AI o impact odhad a generovanie stromu |
| 3.9 | Rozšírený Impact Model o merateľnosť a vizualizáciu |
| 3.14 | Rozšírený View Layer o strom, decision trail a impact zobrazenie |
| 4. Pozitíva | Doplnené spätná väzba rozhodnutí, auditovateľnosť, vzdelávací efekt |
| 4. Negatíva | Doplnené kauzalita vs korelácia, možná manipulácia impactu |
| 5. Alternatívy | Doplnené "žiadne meranie impactu" a "len lineárny zoznam" |
| 6. Väzby | Doplnené ADR-044 (Impact Visualization) |
| 7. Implementácia | Doplnené pravidlá pre impact metrics, tree visualization, decision trail |
| 8.4 | **Nová sekcia:** Príklad 4 – Tree Visualization |
| 8.5 | **Nová sekcia:** Príklad 5 – Decision Trail |
| 8.1-8.3 | Rozšírené príklady o impact metrics |
| 9. Zhrnutie | Doplnené v4 princípy |
| 10. Finálna veta | Rozšírená o rozhodovaciu cestu |