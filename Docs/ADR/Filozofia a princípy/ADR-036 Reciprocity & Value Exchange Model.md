# ADR-036 — Reciprocity & Value Exchange Model

## Status

**Accepted**

---

## Kontext

Synergetikum je distribuovaný systém osobných digitálnych inteligencií, ktorý stojí na dobrovoľnej participácii používateľov.

Používatelia:

- odpovedajú na otázky,
- zapájajú sa do diskusií,
- zdieľajú (v rôznej miere) výstupy svojich modelov,
- podieľajú sa na tvorbe projektov.

Bez jasného mechanizmu hodnoty vzniká zásadné riziko:

> Používatelia nebudú mať dôvod participovať ani zdieľať.

Ak systém nedokáže vytvoriť prirodzený cyklus hodnoty, ostane prázdny alebo sa zredukuje na pasívne používanie bez kolektívnej inteligencie.

Preto je potrebné explicitne definovať princíp reciprocity ako základnú vlastnosť systému.

---

## Rozhodnutie

Synergetikum implementuje princíp:

> Participácia generuje hodnotu pre samotného používateľa.

Systém je navrhnutý tak, aby:

- aktívnejší používatelia dostávali kvalitnejšie výstupy,
- zdieľanie v trusted prostredí zlepšovalo lokálnu inteligenciu,
- kolektívna interakcia zvyšovala presnosť a relevanciu odpovedí,
- zapojenie do projektov vytváralo nové príležitosti pre jednotlivca.

### Kľúčové vymedzenie

**Reciprocita NIE JE:**

- priamy vplyv na hlas v systéme,
- priamy vplyv na váhu rozhodovania,
- explicitné bodovanie alebo status.

**Reciprocita JE:**

- kvalita vstupov (personalizácia, presnosť, relevantnosť),
- rýchlosť učenia modelu,
- kvalita agregovaných pohľadov.

---

## Mechanizmus reciprocity

Reciprocita vzniká kombináciou viacerých vrstiev:

### 1. Learning reciprocity

Čím viac používateľ odpovedá a reflektuje:

- tým presnejší je jeho Personal AI model,
- tým lepšie sú personalizované otázky,
- tým relevantnejšie sú odporúčania.

### 2. Network reciprocity

Pri zdieľaní v trusted circle:

- používateľ získava lepšie agregované pohľady,
- dostáva kvalitnejšie signály z okolia,
- jeho model sa učí rýchlejšie (v rámci povolených hraníc).

### 3. Project reciprocity

Pri zapojení do projektov:

- používateľ získava príležitosti na reálnu participáciu,
- jeho schopnosti sú lepšie rozpoznané,
- môže sa zapojiť do realizácie alebo rozhodovania.

### 4. Insight reciprocity

Systém poskytuje kvalitnejšie výstupy tým, ktorí prispievajú:

- hlbšie analýzy,
- presnejšie simulácie,
- lepšie zhrnutia,
- lepšie mapovanie možností.

---

## Cost asymmetry (jadro mechanizmu)

> Systém prirodzene odmeňuje participáciu vyššou kvalitou výstupov.

**Dôležité rozlíšenie:**

| Nesprávne | Správne |
|-----------|---------|
| „Neparticipuješ → penalizujem ťa“ | „Neparticipuješ → nedostaneš benefit“ |

Systém **netrestá**. Len **neodmeňuje** tých, ktorí neprispievajú.

### Princíp:

> Nízka participácia nevedie k penalizácii, ale k absencii benefitov.

---

## Čo systém nerobí

Systém **NEPOUŽÍVA**:

- explicitné bodovanie používateľov (score, karma),
- verejné rebríčky alebo gamifikáciu založenú na statuse,
- nútené zdieľanie ako podmienku používania,
- penalizáciu za neparticipáciu,
- priamu väzbu medzi reciprocitou a hlasom/rozhodovaním.

Dôvod:

- ochrana autonómie,
- minimalizácia sociálneho tlaku,
- prevencia manipulácie a gamifikácie správania,
- zabránenie vzniku „elitného“ systému.

---

## Dôsledky

Implementácia reciprocity znamená:

- systém prirodzene zvýhodňuje aktívnych používateľov bez toho, aby ich formálne hodnotil,
- vzniká pozitívna spätná väzba medzi učením a hodnotou,
- zdieľanie má reálny benefit, nie len ideologický,
- pasívni používatelia môžu systém používať, ale dostávajú menej kvalitné výstupy.

---

## Riziká a mitigácie

### 1. Nerovnováha medzi používateľmi

Aktívni používatelia môžu mať výrazne lepší model než pasívni.

**Mitigácia:**

- exploration otázky aj pre pasívnych používateľov,
- základná kvalita aj bez vysokej participácie,
- graceful degradation – systém zostáva použiteľný.

### 2. Echo chambers

Reciprocita v trusted circles môže posilniť uzavreté skupiny.

**Mitigácia:**

- občasné exposure na alternatívne perspektívy,
- diversity v question selection,
- exploration factor v personalizácii.

### 3. Overfitting na komunitu

Model sa môže prispôsobiť úzkemu okruhu.

**Mitigácia:**

- separation personal delta a shared foundation,
- limitovanie váhy externých updateov,
- konzervatívny merge (ADR-007).

### 4. Zneužitie reciprocity na vytvorenie elity

Ak by reciprocity ovplyvňovala hlas v systéme, aktívni by sa stali dominantnými.

**Mitigácia:**

- **reciprocita NIE JE priamo napojená na influence alebo rozhodovaciu váhu**,
- reciprocity ovplyvňuje len kvalitu vstupov a personalizáciu.

---

## Architektonické implikácie

Reciprocita ovplyvňuje:

### Personal AI

- learning rate závisí od interakcie,
- profil sa aktualizuje len na základe vlastných alebo povolených dát.

### Question System

- viac odpovedí → lepší výber otázok,
- systém sa učí, čo používateľa zaujíma.

### Sync Layer

- vyššia participácia → vyššia kvalita peer exchange,
- trusted peers majú väčší význam.

### Project Layer

- participácia → vyššia šanca zapojenia do projektov,
- systém lepšie mapuje schopnosti používateľa.

---

## Alternatívy (zamietnuté)

### Explicitná gamifikácia (points, karma)

**Zamietnuté.**

Dôvod:

- vedie k manipulácii správania,
- vytvára sociálny tlak,
- deformuje motiváciu.

### Žiadna reciprocita

**Zamietnuté.**

Dôvod:

- systém by nemal dostatok dát,
- chýbala by motivácia participovať,
- kolektívna inteligencia by nevznikla.

### Reciprocita priamo napojená na hlas/váhu

**Zamietnuté.**

Dôvod:

- vzniká riziko „elitného systému“,
- aktívni používatelia by dominovali,
- pasívni by stratili zmysel participácie.

---

## Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-000 | Zakladajúci princíp reciprocity |
| ADR-001 | Personal AI Ownership – reciprocity rešpektuje vlastníctvo |
| ADR-002 | Human-in-the-loop – reciprocity neobchádza rozhodovanie človeka |
| ADR-003 | Non-Manipulation – reciprocity nie je manipulácia |
| ADR-004 | Privacy-first – reciprocity funguje cez agregáty, nie raw dáta |
| ADR-015 | Participation Filter – reciprocity je výstupom participácie |
| ADR-028 | Support & Alignment – reciprocity ovplyvňuje kvalitu |

---

## Zhrnutie

Reciprocity Engine zabezpečuje, že systém má prirodzený tok hodnoty:

> Čím viac používateľ prispieva, tým viac hodnoty dostáva späť.

Bez donucovania, bez gamifikácie, bez centrálneho riadenia, bez vzniku elít.

### Kľúčové princípy zhrnuté:

| Princíp | Formulácia |
|---------|-------------|
| Cost asymmetry | Neparticipácia = absencia benefitov, nie penalizácia |
| Reciprocita ≠ power | Ovplyvňuje kvalitu, nie hlas |
| Žiadna gamifikácia | Žiadne body, rebríčky, status |
| Prirodzená motivácia | Hodnota je dôsledkom, nie odmenou |

---

## Finálna veta

> Synergetikum odmeňuje participáciu kvalitou pochopenia, nie bodmi alebo statusom. A nikdy netrestá za neparticipáciu – len neposkytuje benefity, ktoré si nezaslúži.

---

**Dátum:** 2026-04-14  
**Stav:** Accepted  
**Typ:** Core Mechanism ADR
