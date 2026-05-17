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

## Definície (merateľné)

### Čo je participácia?

Participácia je **akákoľvek interakcia, ktorá poskytuje systému novú informáciu o používateľovi**.

| Typ interakcie | Počíta sa ako participácia | Váha (relatívna) |
|----------------|---------------------------|------------------|
| Odpoveď na otázku | ✅ áno | vysoká |
| Explicitná reflexia / komentár | ✅ áno | vysoká |
| Zapojenie do diskusie (vlákno) | ✅ áno | stredná |
| Zdieľanie modelu (sync v trusted circle) | ✅ áno | stredná |
| Účasť na projekte (akýkoľvek príspevok) | ✅ áno | vysoká |
| Prečítanie otázky (bez odpovede) | ❌ nie | - |
| Preskočenie otázky | ❌ nie | - |
| Len pasívne prezeranie | ❌ nie | - |
| Náhodná odpoveď (detekovaná) | ❌ nie (ignoruje sa) | - |

### Čo je „kvalitnejší výstup“?

Kvalita výstupu je merateľná v týchto dimenziách:

| Dimenzia | Definícia | Meranie |
|----------|-----------|---------|
| **Presnosť** | Zhoda predikcie/odporúčania s neskoršou realitou | % zhody pri spätnom hodnotení |
| **Relevantnosť** | Miera, do akej výstup zodpovedá záujmom používateľa | Používateľ odpovie na otázku vs. preskočí |
| **Hĺbka** | Množstvo perspektív a alternatív zobrazených používateľovi | Počet perspektív pri rovnakej otázke |
| **Rýchlosť učenia** | Počet interakcií potrebných na stabilizáciu personalizácie | Počet odpovedí do dosiahnutia konzistentnej personalizácie |

### Úrovne kvality výstupov

| Úroveň | Presnosť | Relevantnosť | Hĺbka | Poznámka |
|--------|----------|--------------|-------|-----------|
| **Základná** | štandardná (baseline) | všeobecná | 1–2 perspektívy | dostupná vždy |
| **Rozšírená** | +20 % oproti baseline | personalizovaná | 3–4 perspektívy | vyžaduje strednú participáciu |
| **Hlboká** | +40 % oproti baseline | vysoko personalizovaná | 5+ perspektív | vyžaduje vysokú participáciu |

---

## Mechanizmus reciprocity

Reciprocita vzniká kombináciou viacerých vrstiev:

### 1. Learning reciprocity

Čím viac používateľ odpovedá a reflektuje:

- tým presnejší je jeho Personal AI model,
- tým lepšie sú personalizované otázky,
- tým relevantnejšie sú odporúčania.

**Merateľné:** Počet odpovedí → zlepšenie presnosti o +X % (kalibrované počas pilotu).

### 2. Network reciprocity

Pri zdieľaní v trusted circle:

- používateľ získava lepšie agregované pohľady,
- dostáva kvalitnejšie signály z okolia,
- jeho model sa učí rýchlejšie (v rámci povolených hraníc).

**Merateľné:** Počet aktívnych peerov → rýchlosť učenia sa zvyšuje o +Y %.

### 3. Project reciprocity

Pri zapojení do projektov:

- používateľ získava príležitosti na reálnu participáciu,
- jeho schopnosti sú lepšie rozpoznané,
- môže sa zapojiť do realizácie alebo rozhodovania.

**Merateľné:** Počet projektov → počet pozvánok do nových projektov.

### 4. Insight reciprocity

Systém poskytuje kvalitnejšie výstupy tým, ktorí prispievajú:

- hlbšie analýzy,
- presnejšie simulácie,
- lepšie zhrnutia,
- lepšie mapovanie možností.

**Merateľné:** Porovnanie výstupov pre používateľov s rôznou úrovňou participácie.

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

### Decay participácie (polčas rozpadu)

Participácia nie je permanentný stav. Aby systém odrážal súčasné správanie používateľa:

> Každý participačný signál má polčas rozpadu **30 dní**.

**Dôsledok:**

- aktívny používateľ si udržiava vysokú úroveň,
- používateľ, ktorý prestal participovať, sa postupne vráti na základnú úroveň,
- nejde o penalizáciu – ide o to, že staré zásluhy prestávajú byť relevantné.

**Implementácia:** Exponenciálny váhový faktor s half-life = 30 dní.

---

## Ochrana proti falošnej participácii

Systém musí rozlišovať medzi **užitočnou** a **náhodnou/spamovou** participáciou.

### Detekované vzorce (automaticky ignorované)

| Vzorec | Detekcia | Akcia |
|--------|----------|-------|
| Náhodné odpovede | Nízka konzistencia odpovedí (variance > prah) | Odpoveď sa nezapočítava do participácie |
| Rovnaká odpoveď opakovane | Identická odpoveď na rôzne otázky | Ignoruje sa ako spam |
| Príliš rýchle odpovede | Čas odpovede < 1 sekunda (bez možnosti prečítania) | Považuje sa za náhodnú |
| Pattern „všetko áno“ alebo „všetko nie“ | Extrémna jednostrannosť | Znížená váha participácie |

### Čo systém NEROBÍ

- Neoznačuje používateľa ako „spamera“ (žiadna nálepka)
- Neznižuje existujúcu úroveň participácie (len ignoruje nové vstupy)
- Neblokuje používateľa z participácie

---

## Percepcia reciprocity (Ako používateľ vie, že je odmenený?)

Toto je kritická otázka. Mechanika reciprocity je zbytočná, ak používateľ nevie, že funguje.

Používateľ nevidí do vnútra modelu. Nevidí „rýchlosť učenia“. Nevidí váhy.

> Ak používateľ nevie, že je odmenený, tak pre neho odmena neexistuje.

### Formy komunikácie reciprocity

Systém používa **LEN** nasledujúce formy:

#### 1. Prirodzený pocit zlepšenia (najdôležitejší)

Používateľ postupne cíti, že:

- systém mu čoraz lepšie rozumie,
- odpovede sú čoraz presnejšie a relevantnejšie,
- nemusí opakovať to isté znova a znova,
- otázky sú čoraz lepšie zacielené na jeho záujmy.

Toto je najprirodzenejšia a najnenásilnejšia forma percepcie.

**Limitácia:** vyžaduje čas, kým si to používateľ uvedomí.

---

#### 2. Spätná väzba v momente benefitu (jediná povolená explicitná forma)

Keď systém poskytne výstup, ktorý je kvalitnejší vďaka predchádzajúcej participácii:

> „Toto odporúčanie vzniklo vďaka tvojim odpovediam v oblasti zdravia.“

Alebo:

> „Vidíš túto tému? Spoznali sme ju, lebo si odpovedal na podobné otázky.“

**Pravidlá:**

- zobrazená **v kontexte** (nie ako notifikácia)
- nevyžaduje akciu používateľa
- nie je opakovaná (max 1x za reláciu na rovnaký typ benefitu)

---

#### 3. Voliteľný stavový indikátor (nie dashboard)

Používateľ môže vidieť **jeden binárny indikátor**:

> „Tvoja participácia zlepšuje kvalitu systému.“ (áno/nie)

**Žiadne:**

- úrovne (základná/stredná/vysoká)
- číselné skóre
- percentá
- porovnanie s ostatnými

**Dôvod:** Akýkoľvek viacstavový indikátor vytvára level system a gamifikáciu.

---

### Čo systém NIKDY nerobí

| Zakázané | Dôvod |
|----------|-------|
| Číselné skóre (body, karma) | Gamifikácia = manipulácia |
| Verejné rebríčky | Sociálny tlak, toxicita |
| Porovnávanie s ostatnými | Narúša autonómiu |
| Odomykanie funkcií | Trestanie neaktívnych |
| „Si v top X %“ | Statusová súťaž |
| Pravidelné notifikácie o participácii | Spam, otravovanie |
| Levely / úrovne (aj v dashboarde) | Vytvára gamifikáciu |
| Dashboard s viac ako binárnym stavom | Level system |

---

## Čo systém nerobí (celkový prehľad)

Systém **NEPOUŽÍVA**:

- explicitné bodovanie používateľov (score, karma),
- verejné rebríčky alebo gamifikáciu založenú na statuse,
- nútené zdieľanie ako podmienku používania,
- penalizáciu za neparticipáciu,
- priamu väzbu medzi reciprocitou a hlasom/rozhodovaním,
- odomykanie funkcií na základe aktivity,
- porovnávanie používateľov medzi sebou,
- pravidelné notifikácie o participácii,
- level system alebo úrovne participácie.

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
- pasívni používatelia môžu systém používať, ale dostávajú menej kvalitné výstupy,
- používateľ **vie**, že jeho participácia má zmysel – vidí to na kvalite výstupov a na explicitnej spätnej väzbe v momente benefitu,
- stará participácia postupne stráca váhu (decay), aby systém odrážal súčasné správanie,
- falošná participácia (spam, náhodné odpovede) sa nezapočítava.

---

## Riziká a mitigácie

### 1. Nerovnováha medzi používateľmi

Aktívni používatelia môžu mať výrazne lepší model než pasívni.

**Mitigácia:**

- exploration otázky aj pre pasívnych používateľov,
- základná kvalita (baseline) aj bez vysokej participácie,
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

### 5. Falošná participácia (spam, náhodné odpovede)

Používatelia by mohli umelo zvyšovať svoju participáciu bez reálnej hodnoty.

**Mitigácia:**

- detekcia náhodných odpovedí (variance, čas, patterny),
- takéto odpovede sa nezapočítavajú,
- žiadna penalizácia, len ignorovanie.

### 6. Používateľ nechápe súvislosť

Aj keď sú výstupy kvalitnejšie, používateľ nemusí vedieť, že je to vďaka jeho participácii.

**Mitigácia:**

- spätná väzba v momente benefitu (explicitná kauzalita),
- prirodzený pocit zlepšenia (postupne sa dostaví),
- binárny indikátor pre záujemcov.

### 7. Decay participácie môže pôsobiť ako penalizácia

Ak používateľ prestane participovať, jeho úroveň klesá.

**Mitigácia:**

- decay je pomalý (30 dní polčas),
- komunikuje sa ako „staré dáta strácajú relevanciu“, nie ako trest,
- základná úroveň je vždy dostupná.

---

## Architektonické implikácie

Reciprocita ovplyvňuje:

### Personal AI

- learning rate závisí od interakcie,
- profil sa aktualizuje len na základe vlastných alebo povolených dát,
- participačné signály majú váhu s decay.

### Question System

- viac kvalitných odpovedí → lepší výber otázok,
- systém sa učí, čo používateľa zaujíma,
- náhodné odpovede sa ignorujú.

### Sync Layer

- vyššia participácia → vyššia kvalita peer exchange,
- trusted peers majú väčší význam.

### Project Layer

- participácia → vyššia šanca zapojenia do projektov,
- systém lepšie mapuje schopnosti používateľa.

### Feedback Layer

- spätná väzba v momente benefitu (jediná explicitná forma),
- binárny stavový indikátor (voliteľný),
- žiadne notifikácie, žiadny dashboard s úrovňami.

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

### Len prirodzený pocit zlepšenia (bez explicitnej komunikácie)

**Zamietnuté ako jediná forma.**

Dôvod:

- používateľ nemusí spozorovať súvislosť,
- trvá príliš dlho, kým si to uvedomí,
- systém by pôsobil „magicky“.

### Dashboard s úrovňami (základná/stredná/vysoká)

**Zamietnuté.**

Dôvod:

- vytvára level system,
- level system = gamifikácia,
- vedie k optimalizácii na úroveň, nie na kvalitu.

### Pravidelné notifikácie o participácii

**Zamietnuté.**

Dôvod:

- aj 1x týždenne je spam,
- používatelia ich vypnú,
- neprinášajú hodnotu v momente.

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

## Otvorené otázky (pre pilot)

Nasledujúce otázky nie sú v tomto ADR definitívne vyriešené. Budú kalibrované počas pilotu:

| Otázka | Spôsob riešenia |
|--------|------------------|
| Aký je presný vzťah medzi počtom odpovedí a zlepšením presnosti? | Pilotné meranie, kalibrácia parametrov |
| Aký je optimálny polčas rozpadu participácie? | Pilotné meranie (začiatok: 30 dní) |
| Ako presne detekovať náhodné odpovede? | Pilotné meranie, úprava prahov |
| Aká je minimálna základná kvalita (baseline) pre pasívnych používateľov? | Pilotné meranie, spätná väzba |

---

## Zhrnutie

Reciprocity Engine zabezpečuje, že systém má prirodzený tok hodnoty:

> Čím viac používateľ prispieva, tým viac hodnoty dostáva späť.

Bez donucovania, bez gamifikácie, bez centrálneho riadenia, bez vzniku elít, bez level systemov, bez notifikačného spamu.

**A zároveň:** používateľ **vie**, že je odmenený – cíti to na kvalite výstupov, vidí to v spätnej väzbe v momente benefitu a môže to kedykoľvek skontrolovať (binárny indikátor).

### Kľúčové princípy zhrnuté:

| Princíp | Formulácia |
|---------|-------------|
| Cost asymmetry | Neparticipácia = absencia benefitov, nie penalizácia |
| Reciprocita ≠ power | Ovplyvňuje kvalitu, nie hlas |
| Žiadna gamifikácia | Žiadne body, rebríčky, status, levely |
| Prirodzená motivácia | Hodnota je dôsledkom, nie odmenou |
| Percepcia reciprocity | Používateľ musí vedieť, že je odmenený |
| Explicitná kauzalita | Spájame benefit s predchádzajúcou akciou (v momente) |
| Decay participácie | Staré zásluhy strácajú váhu (30d half-life) |
| Ochrana proti spamu | Náhodné odpovede sa nezapočítavajú |
| Žiadne notifikácie | Iba spätná väzba v momente benefitu |
| Žiadne levely | Iba binárny indikátor (áno/nie) |

---

## Finálna veta

> Synergetikum odmeňuje participáciu kvalitou pochopenia, nie bodmi alebo statusom. A nikdy netrestá za neparticipáciu – len neposkytuje benefity, ktoré si nezaslúži. Používateľ o tom vie – cíti to, vidí to v momente benefitu a môže to kedykoľvek overiť (áno/nie). Žiadne levely, žiadne notifikácie, žiadna gamifikácia.

---

**Dátum:** 2026-04-14  
**Stav:** Accepted  
**Typ:** Core Mechanism ADR
