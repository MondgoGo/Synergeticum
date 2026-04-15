# 🧾 ADR-025: Argumentation & Reasoning Model (Hardened v5)

> **Zmeny oproti Version 4 (Hardened v4):**
> - **Doplnená časť 3.22:** Threading a typy diskusných príspevkov (prepojenie na INSPIRATION-07)
> - **Rozšírená časť 3.6:** Argument graph o diskusné vlákna
> - **Rozšírená časť 3.14:** View Layer o zobrazenie threadov
> - **Rozšírená časť 6:** Väzby na ADR-027
> - **Nová sekcia 8.6:** Príklad pre Threading a diskusné príspevky

---

## 1. 📌 Status

**Accepted (v5 – Hardened with Threading & Discussion Posts)**

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

### Dodatočný problém (v5)

Aj pri všetkých vyššie uvedených vylepšeniach chýba:

* **štruktúrovaná diskusia** – argumenty sú statické, chýba živá interakcia
* **typy príspevkov** – nevieme rozlíšiť otázku od námietky alebo návrhu
* **threading** – diskusia je plochá, stráca sa kontext

👉 systém potrebuje **diskusné vlákna (threads)** a **typované príspevky**.

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
* **poskytuje diskusné vlákna a typované príspevky (v5)**

---

### 🧩 3.2 Argument ako základná jednotka

Základná entita nie je:

❌ komentár
❌ názor

👉 ale:

👉 **argument**

**V5 rozšírenie:** Každý argument má vlastné diskusné vlákno (thread) pre doplňujúce otázky, návrhy a námietky.

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
| **Thread (v5)** | diskusné vlákno s príspevkami |

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

### 🧠 3.6 Argument graph (Rozšírené v v5)

Argumenty tvoria:

👉 **graph (sieť vzťahov)**

Obsahuje:

* podporujúce argumenty
* protiargumenty
* alternatívne pohľady
* **diskusné vlákna (threads) priradené ku každému argumentu (v5)**

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

### 📊 3.19 Impact Metrics – meranie vplyvu argumentov na rozhodnutia

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

### 🌳 3.20 Tree Visualization – stromová vizualizácia argumentov

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

### 🗺️ 3.21 Decision Trail – sledovanie rozhodovacej cesty

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

### 🧵 3.22 Threading a typy diskusných príspevkov (NOVÉ v v5)

**Princíp:**

> **Argumenty nie sú izolované statické entity. Každý argument má vlastné diskusné vlákno, kde môžu používatelia klásť otázky, navrhovať vylepšenia, pridávať podporu alebo námietky.**

#### A. Diskusné vlákno (Thread)

Každý argument má priradené diskusné vlákno. Vlákno je stromová štruktúra príspevkov.

```json
{
  "argument_id": "arg_001",
  "thread": {
    "thread_id": "thread_arg_001",
    "posts": [
      {
        "post_id": "post_001",
        "parent_post_id": null,  // null = koreňový príspevok (samotný argument)
        "author_id": "user_miro_001",
        "type": "argument",  // špeciálny typ pre samotný argument
        "content": "Pizzéria by mala používať lokálne suroviny...",
        "created_at": "2026-04-15T10:00:00Z"
      },
      {
        "post_id": "post_002",
        "parent_post_id": "post_001",
        "author_id": "user_jana_001",
        "type": "question",
        "content": "Aké konkrétne lokálne suroviny máš na mysli?",
        "created_at": "2026-04-15T11:00:00Z"
      },
      {
        "post_id": "post_003",
        "parent_post_id": "post_002",
        "author_id": "user_miro_001",
        "type": "clarification",
        "content": "Myslím múku od mlyna v Trnave, zeleninu od okolitých farmárov...",
        "created_at": "2026-04-15T11:30:00Z"
      }
    ]
  }
}
```

#### B. Typy príspevkov

| Typ | Význam | Ikona | Vplyv na argument |
|-----|--------|-------|-------------------|
| `argument` | Samotný argument (koreň vlákna) | 📋 | Základná entita |
| `question` | Otázka k argumentu | ❓ | Žiadny priamy vplyv, ale signalizuje nejasnosť |
| `suggestion` | Návrh na vylepšenie argumentu | 💡 | Môže viesť k aktualizácii argumentu |
| `support` | Podpora argumentu (dodatočné dôkazy, príklady) | ✅ | Zvyšuje confidence argumentu |
| `objection` | Námietka (nový protiargument) | ⚠️ | Môže viesť k vytvoreniu nového protiargumentu |
| `clarification` | Žiadosť o vysvetlenie | 📖 | Signalizuje, že argument nie je dostatočne jasný |
| `evidence` | Dodatočný dôkaz | 📎 | Zvyšuje factual_basis skóre |

#### C. Threading pravidlá

1. **Stromová štruktúra** – každý príspevok (okrem koreňa) má `parent_post_id`.
2. **Maximálna hĺbka** – odporúčaná maximálna hĺbka vlákna je 5 úrovní (aby diskusia zostala prehľadná). Pri prekročení systém navrhne vytvorenie nového argumentu.
3. **Každý príspevok patrí k jednému argumentu** – nemôže byť priradený k viacerým argumentom.
4. **Príspevky sú synchronizované** medzi participantmi projektu (ADR-030).

#### D. Vplyv príspevkov na kvalitu argumentu

| Typ príspevku | Vplyv na quality_score |
|---------------|----------------------|
| `support` s vysokou kvalitou | + (zvyšuje) |
| `objection` bez protiargumentu | - (znižuje, ak nie je adresovaný) |
| `clarification` požiadavka | 0, ale znižuje clarity skóre dočasne |
| `question` bez odpovede | 0, ale signalizuje potrebu vylepšenia |
| `evidence` s overiteľnými faktami | ++ (výrazne zvyšuje factual_basis) |

#### E. Moderovanie threadov

V rámci Trusted Circle (ADR-004) môžu participanti:

* **Nahlásiť** spam alebo off-topic príspevok
* **Označiť** príspevok ako `resolved` (napr. otázka bola zodpovedaná)
* **Zhrnúť** dlhé vlákno (AI asistované)

Systém **NESMIE** automaticky mazať príspevky – každé odstránenie musí byť auditovateľné (ADR-030).

---

### 🧠 3.8 Úloha AI (Rozšírené v v5)

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
* **sumarizuje dlhé diskusné vlákna (v5)**
* **navrhuje odpovede na nezodpovedané otázky (v5)**
* **deteguje, kedy diskusia presiahla maximálnu hĺbku a navrhuje nový argument (v5)**

AI NESMIE:

* rozhodovať
* určovať „pravdu“
* **skrývať kvalitné argumenty kvôli nízkej popularite**
* **manipulovať impact score v prospech niektorých argumentov (v4)**
* **automaticky mazať príspevky v threadoch (v5)**

---

### 🔥 3.9 Argument → Impact Model

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
* **diskusné vlákna (v5)**

👉 rozhoduje **človek** (ADR-002)

---

### 🧠 3.12 Ochrana pred mocou (prepojenie na ADR-038)

Argumentation Layer je navrhnutá tak, aby:

* minimalizovala vplyv silných osobností
* zabránila dominancii hlasných používateľov
* oddelila kvalitu argumentu od jeho prezentácie
* **zabránila dominancii populárnych, ale nekvalitných argumentov**
* **zabránila manipulácii impact metrics (v4)**
* **zabránila tomu, aby hlasní používatelia dominovali v diskusných vláknach (v5)**

> Argument môže získať vplyv len tým, že obstojí v konfrontácii s inými argumentmi – nie tým, kto ho vyslovil alebo koľko ľudí s ním súhlasí.

---

### 🔄 3.13 Výstup projektu (Rozšírené v v5)

Výsledok je:

👉 **structured reasoning output + branch landscape + quality assessment + impact analysis + discussion threads (v5)**

Obsahuje:

* hlavné vetvy
* argumenty pre každú vetvu (s kvalitou)
* protiargumenty (s kvalitou)
* dôsledky
* konflikty medzi vetvami
* **argumenty s najvyššou kvalitou (aj keď nepopulárne)**
* **argumenty s najvyšším impactom (v4)**
* **decision trail pre každého účastníka (v4)**
* **diskusné vlákna pre každý argument (v5)**

---

### 🧠 3.14 Prepojenie na View Layer (Rozšírené v v5)

Používateľ vidí:

* zjednodušené argumenty
* hlavné konflikty
* kľúčové rozhodovacie body
* **kvalitu argumentov (zobrazenú oddelene od popularity)**
* **možnosť prepínať medzi „podľa kvality“ a „podľa popularity“**
* **stromovú vizualizáciu argumentov (v4)**
* **svoju rozhodovaciu cestu (decision trail) (v4)**
* **argumenty, ktoré ovplyvnili jeho rozhodnutia (v4)**
* **diskusné vlákna priradené ku každému argumentu (v5)**
* **možnosť pridávať otázky, návrhy, podporu a námietky (v5)**
* **zhrnutie dlhých vlákien (AI generované) (v5)**

👉 nie celý graph (pokiaľ si ho používateľ explicitne nerozbali).

---

### 🧠 3.15 Governance napojenie

Argumentation Layer:

* dopĺňa Support Model (ADR-028)
* dopĺňa Branching (ADR-024)
* je základom pre Power balancing (ADR-038)
* **poskytuje vstup pre alignment cez kvalitu argumentov**
* **poskytuje auditovateľný záznam rozhodnutí (v4)**
* **poskytuje auditovateľný záznam diskusných príspevkov (v5)**

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
| **Štruktúrovaná diskusia (v5)** | otázky, návrhy, námietky majú svoje miesto |
| **Prehľadnosť (v5)** | diskusné vlákna udržiavajú kontext |
| **Živá argumentácia (v5)** | argumenty nie sú statické, môžu sa vyvíjať |

### ❗ Negatívne / Trade-offs

| Oblasť | Dôsledok |
|--------|----------|
| **Komplexita** | vyšší vstupný náklad |
| **Kognitívna náročnosť** | vyžaduje dobrý UX (ADR-027) |
| **Výpočtová komplexita** | argument graph môže rásť exponenciálne, plus metriky kvality, impactu a threadov |
| **Riziko „falošnej objektivity“** | AI metriky nie sú dokonalé, môžu byť biasované |
| **Lokálna výpočtová náročnosť** | hodnotenie kvality a impactu beží na zariadení používateľa |
| **Kauzalita vs korelácia (v4)** | impact score je odhad, nie absolútna pravda |
| **Možná manipulácia impactu (v4)** | používateľ môže falošne tvrdiť, že argument ovplyvnil jeho rozhodnutie |
| **Správa threadov (v5)** | potreba moderovania a sumarizácie |
| **Úložné nároky (v5)** | diskusné príspevky zväčšujú objem dát |

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

### ❌ Iba argumenty bez diskusných vlákien (v5)

* jednoduchšie
* ale: chýba priestor na otázky, návrhy a vysvetlenia

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
| **ADR-027** | **View Layer – zjednodušený pohľad pre používateľa, stromová vizualizácia, zobrazenie threadov (v5)** |
| ADR-028 | Support Model – kvalita argumentov ovplyvňuje alignment, impact metrics sledujú zmeny supportu |
| ADR-029 | Branch Lifecycle – vetvy zanikajú na základe kvality argumentov |
| ADR-030 | Sync & Persistence – argument graph, hodnotenia a **diskusné vlákna** musia byť perzistentné |
| ADR-034 | Personal AI Internal Model – váhy pre metriky kvality sú v Preference Modeli |
| ADR-038 | Power & Influence Balancing – argumentation layer je nástrojom na elimináciu moci |
| ADR-039 | Truth & Epistemic Model – kvalita argumentov je vstupom pre epistemické hodnotenie |
| ADR-040 | Graceful Degradation – nízka kvalita vstupov vedie k nižšej kvalite hodnotení |
| **ADR-042** | **Unified Entity Model – Argument je typ Node, Thread môže byť samostatný Node** |

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
* **poskytnúť ku každému argumentu diskusné vlákno (v5)**
* **umožniť typované príspevky (question, suggestion, support, objection, clarification, evidence) (v5)**
* **udržiavať stromovú štruktúru diskusie (parent_post_id) (v5)**
* **obmedziť maximálnu hĺbku vlákna (napr. 5 úrovní) (v5)**
* **poskytnúť AI sumarizáciu dlhých vlákien (v5)**

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
  
  "impact_metrics": {
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
   💬 12 príspevkov v diskusii (3 otázky, 2 námietky)

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
    "impact_weights": {
      "support_shift": 0.4,
      "branch_creation": 0.2,
      "branch_merger": 0.2,
      "decision_reference": 0.1,
      "user_attestation": 0.1
    }
  }
}
```

### Príklad 4: Tree Visualization

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

### Príklad 5: Decision Trail

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

### Príklad 6: Threading a diskusné príspevky (NOVÉ v v5)

```json
{
  "argument_id": "arg_cf_001",
  "claim": "Buduje komunitu",
  "thread": {
    "thread_id": "thread_arg_cf_001",
    "summary": "Diskusia o tom, ako crowdfunding buduje komunitu. Hlavné otázky: aké konkrétne aktivity, aký je odhadovaný dosah.",
    "posts": [
      {
        "post_id": "post_001",
        "parent_post_id": null,
        "author_id": "user_miro_001",
        "type": "argument",
        "content": "Crowdfunding buduje komunitu – ľudia sa cítia byť súčasťou niečoho väčšieho.",
        "created_at": "2026-04-15T10:00:00Z",
        "quality_score": 0.91
      },
      {
        "post_id": "post_002",
        "parent_post_id": "post_001",
        "author_id": "user_jana_001",
        "type": "question",
        "content": "Aké konkrétne aktivity myslíš, že najviac budujú komunitu?",
        "created_at": "2026-04-15T11:00:00Z",
        "resolved": true,
        "resolved_at": "2026-04-15T12:00:00Z"
      },
      {
        "post_id": "post_003",
        "parent_post_id": "post_002",
        "author_id": "user_miro_001",
        "type": "clarification",
        "content": "Pravidelné updatey, Q&A session, oslava pri dosiahnutí cieľa, možnosť zapojiť sa do rozhodovania o bonusoch.",
        "created_at": "2026-04-15T12:00:00Z"
      },
      {
        "post_id": "post_004",
        "parent_post_id": "post_001",
        "author_id": "user_peter_001",
        "type": "support",
        "content": "Podľa štúdie Y projekty s komunitným crowdfundingom majú o 40% vyššiu mieru udržania zákazníkov.",
        "created_at": "2026-04-15T13:00:00Z",
        "evidence": ["Štúdia Y, 2024"]
      },
      {
        "post_id": "post_005",
        "parent_post_id": "post_001",
        "author_id": "user_zuzana_001",
        "type": "objection",
        "content": "Ale crowdfunding môže vytvoriť len ilúziu komunity, ak sa ľudia nezapájajú aj po skončení kampane.",
        "created_at": "2026-04-15T14:00:00Z"
      },
      {
        "post_id": "post_006",
        "parent_post_id": "post_005",
        "author_id": "user_miro_001",
        "type": "clarification",
        "content": "Súhlasím, preto navrhujeme pravidelný komunitný newsletter a štvrťročné stretnutia.",
        "created_at": "2026-04-15T15:00:00Z"
      }
    ],
    "statistics": {
      "total_posts": 6,
      "unresolved_questions": 0,
      "unanswered_objections": 0,
      "last_activity": "2026-04-15T15:00:00Z"
    }
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

**V4 priniesla:**

* **Impact Metrics** – meranie vplyvu argumentov na rozhodnutia
* **Tree Visualization** – stromová vizualizácia argumentov a vetiev
* **Decision Trail** – sledovanie rozhodovacej cesty každého používateľa

**V5 prináša:**

* **Threading** – každý argument má vlastné diskusné vlákno
* **Typy príspevkov** – question, suggestion, support, objection, clarification, evidence
* **Stromová štruktúra diskusie** – zachovanie kontextu
* **AI sumarizácia vlákien** – prehľadnosť dlhých diskusií

---

## 10. 🧭 Finálna veta

> **V Synergetiku nemá vplyv ten, kto hovorí – ale to, čo prežije konfrontáciu s inými myšlienkami. A ani popularita nerobí argument pravdou – len kvalita rozhoduje o jeho sile. A každé rozhodnutie je spätne dohľadateľné k argumentom, ktoré ho ovplyvnili. A každý argument je živou diskusiou – s otázkami, návrhmi, podporou a námietkami.**

V komplexných problémoch nevyhráva ten, kto kričí najhlasnejšie – ale ten, koho argument prežije najviac otázok. A nevyhráva ani ten, koho argument má najviac lajkov. A každý používateľ vidí, prečo sa rozhodol tak, ako sa rozhodol. A každý môže prispieť do diskusie štruktúrovaným spôsobom.

---

**Dátum:** 2026-04-15  
**Stav:** Accepted  
**Typ:** Core Mechanism ADR (Conceptual Projects)  
**Verzia:** 5.0 (Hardened with Threading & Discussion Posts)

---

## 📝 Zoznam zmien oproti Version 4

| Sekcia | Zmena |
|--------|-------|
| 1. Status | Zmenené na v5 |
| 2. Kontext | Doplnený dodatočný problém (chýbajúca štruktúrovaná diskusia) |
| 3.1 | Doplnené diskusné vlákna a typované príspevky |
| 3.2 | Rozšírené o thread pre každý argument |
| 3.3 | Doplnené `Thread` do štruktúry argumentu |
| 3.6 | Rozšírený argument graph o diskusné vlákna |
| 3.22 | **Nová sekcia:** Threading a typy diskusných príspevkov |
| 3.8 | Rozšírené úlohy AI o sumarizáciu threadov a detekciu hĺbky |
| 3.12 | Rozšírené o ochranu pred dominanciou v threadoch |
| 3.13 | Rozšírený výstup projektu o discussion threads |
| 3.14 | Rozšírený View Layer o zobrazenie threadov a pridávanie príspevkov |
| 3.15 | Rozšírené o auditovateľný záznam diskusných príspevkov |
| 4. Pozitíva | Doplnené štruktúrovaná diskusia, prehľadnosť, živá argumentácia |
| 4. Negatíva | Doplnené správa threadov, úložné nároky |
| 5. Alternatívy | Doplnené "iba argumenty bez diskusných vlákien" |
| 6. Väzby | Doplnené ADR-027 (View Layer) |
| 7. Implementácia | Doplnené pravidlá pre threading, typy príspevkov, maximálnu hĺbku, sumarizáciu |
| 8.6 | **Nová sekcia:** Príklad 6 – Threading a diskusné príspevky |
| 9. Zhrnutie | Doplnené v5 princípy |
| 10. Finálna veta | Rozšírená o diskusné vlákna a typy príspevkov |