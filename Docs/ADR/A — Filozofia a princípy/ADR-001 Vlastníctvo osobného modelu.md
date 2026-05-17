# ADR-001: Vlastníctvo osobného modelu

**Stav:** Navrhnuté
**Dátum:** 2026-04-11
**Rozhodovatelia:** Architekt systému, produktový vlastník
**Typ:** Základné architektonické rozhodnutie

## Kontext

Navrhovaný systém má fungovať ako personalizovaná AI architektúra pre mobilné a desktopové zariadenia, s možnosťou lokálneho učenia, selektívnej synchronizácie a dôveryhodných kruhov zdieľania.

Pri návrhu existujú dve hlavné filozofie:

### Varianta A: Centrálne alebo kolektívne vlastnený model

Používateľ je primárne zdroj dát. Lokálne zariadenia slúžia ako klienti, prípadne ako edge vykonávatelia časti výpočtu. Hlavná inteligencia systému je v spoločnom modeli.

### Varianta B: Osobne vlastnený model

Každý používateľ má vlastný model, ktorý sa učí primárne z jeho lokálnych dát a je pod jeho kontrolou. Synchronizácia s inými uzlami je len doplnkový mechanizmus, nie hlavný zdroj identity modelu.

Rozhodnutie medzi týmito variantmi zásadne ovplyvní:

* dátový model,
* synchronizačnú logiku,
* UX nastavenia súkromia,
* merge politiku,
* architektúru federácie,
* permission vrstvu,
* dôvernostný model systému.

Systém má zároveň tieto požiadavky:

* vysoká personalizácia,
* súkromie ako jadrová vlastnosť,
* fungovanie offline,
* selektívne zdieľanie,
* kompatibilita s mobilom aj desktopom,
* C# / .NET MAUI runtime,
* možnosť mikro-federácie v trusted circles.

## Rozhodnutie

Systém bude postavený na princípe, že:

**Každý používateľ vlastní svoj osobný model.**

To znamená:

1. **Primárna identita modelu je lokálna.**
   Model používateľa je definovaný predovšetkým jeho lokálnymi dátami, jeho preferenciami, jeho históriou a jeho povoleniami.

2. **Lokálne učenie má prioritu pred externým vplyvom.**
   Externé synchronizačné updatey môžu model ovplyvniť len v obmedzenej a riadenej forme.

3. **Synchronizácia je doplnok, nie autorita.**
   Shared foundation, trusted sync alebo community assist nesmú prepísať osobnú identitu modelu bez explicitne definovaných pravidiel.

4. **Používateľ rozhoduje o zdieľaní.**
   Používateľ určuje:

   * či sa model synchronizuje,
   * s kým sa synchronizuje,
   * ktoré moduly sa môžu zdieľať,
   * aký typ artefaktov sa môže prenášať,
   * aký silný vplyv môžu mať externé updatey.

5. **Osobný model je produktové jadro systému.**
   Sieť, federácia, trust circles aj desktop uzly existujú preto, aby podporili osobný model, nie aby ho nahradili.

## Rozsah rozhodnutia

Toto rozhodnutie sa vzťahuje na:

* vision model,
* text model,
* health model,
* behavior model,
* fusion vrstvu,
* permission vrstvu,
* sync a merge mechanizmy,
* mobilné aj desktopové uzly.

Nevzťahuje sa automaticky na:

* statické base modely distribuované výrobcom systému,
* vývojové experimenty mimo produkčného runtime,
* jednorazové bootstrap modely, ktoré ešte neobsahujú osobné dáta.

## Dôvody rozhodnutia

### 1. Personalizácia nie je vedľajší efekt, ale cieľ

Mnohé problémy, ktoré tento systém rieši, sú osobné, kontextové a časovo závislé. Jeden univerzálny model by príliš často priemeroval odlišných ľudí.

### 2. Súkromie sa lepšie chráni vlastníctvom než anonymizáciou

Ak je osobný model primárny a dáta ostávajú lokálne, netreba sa spoliehať len na to, že centrálne systémy budú „dostatočne anonymné“.

### 3. Offline-first architektúra to prirodzene podporuje

Ak má systém fungovať kvalitne aj bez stáleho spojenia, lokálny model nemôže byť len cache globálneho modelu.

### 4. Trusted sync dáva zmysel len vtedy, ak existuje niečo osobné, čo chrániš

Ak by model nebol osobný, trusted circles by boli len slabšou formou centralizácie.

### 5. Produktová dôvera bude vyššia

Používateľ skôr prijme systém, ktorý komunikuje:
„Toto je tvoj model a ty rozhoduješ, čo sa s ním deje“
než systém, ktorý potichu absorbuje jeho dáta do kolektívnej inteligencie.

## Architektonické dôsledky

### Pozitívne dôsledky

#### 1. Silná personalizácia

Systém sa môže reálne prispôsobovať jednotlivcovi, nie priemernému používateľovi.

#### 2. Lepšie súkromie

Dáta aj veľká časť učenia ostávajú lokálne.

#### 3. Zmysluplné trusted circles

Synchronizácia má charakter spolupráce medzi osobnými modelmi, nie odovzdania kontroly.

#### 4. Lepšia modularita

Každý modelový modul môže mať odlišnú politiku zdieľania a merge.

#### 5. Vyššia produktová dôveryhodnosť

Vlastníctvo modelu je jasne komunikovateľná hodnota.

### Negatívne dôsledky

#### 1. Vyššia architektonická zložitosť

Systém musí riešiť:

* personal delta,
* validation gate,
* rollback,
* permission engine,
* trust graph,
* lokálne verzie modelov.

#### 2. Slabšia globálna uniformita

Nebude existovať jeden „pravdivý“ model pre všetkých.

#### 3. Ťažšia evaluácia

Výkon systému bude potrebné hodnotiť aj na úrovni osobných trajektórií, nie len globálnych metrík.

#### 4. Komplikovanejší sync

Synchronizácia nemôže byť agresívna. Musí rešpektovať individuálnu identitu modelu.

#### 5. Vyšší nárok na UX a transparentnosť

Používateľ musí chápať, čo vlastní, čo zdieľa a aký to má dopad.

## Implementačné dôsledky

Z tohto rozhodnutia priamo vyplýva potreba týchto mechanizmov:

* `UserModelProfile`
* `PermissionLayer`
* `TrustGraph`
* `SharingPolicy`
* `ValidationGate`
* `RollbackManager`
* `SharedFoundation`
* `PersonalDelta`
* `ModelVersionRegistry`

Každý modelový modul musí vedieť rozlíšiť:

* lokálny checkpoint,
* shared foundation checkpoint,
* pending external candidate,
* aktívny osobný stav po merge.

## Čo tento princíp znamená prakticky

### Mobil

Mobil je primárny nositeľ osobného modelu:

* zbiera osobné dáta,
* vykonáva inferenciu,
* robí ľahké lokálne učenie,
* rozhoduje o syncu.

### Desktop

Desktop môže:

* zrýchliť učenie,
* slúžiť ako teacher,
* slúžiť ako trusted coordinator,
* ale nesmie automaticky prevziať vlastníctvo osobného modelu používateľa.

### Trusted circle

Trusted circle môže:

* zdieľať znalosti,
* zlepšovať shared foundation,
* pomáhať s cold start alebo jemným posunom kvality,
* ale nesmie byť nadradený lokálnemu modelu.

## Alternatívy, ktoré boli zvažované

### A. Centrálne vlastnený model

**Zamietnuté**, pretože:

* oslabuje personalizáciu,
* zhoršuje privacy framing,
* vedie k produktovej aj filozofickej centralizácii.

### B. Kolektívny model s lokálnou cache

**Zamietnuté**, pretože:

* lokálna inteligencia je sekundárna,
* používateľ reálne nevlastní svoj model,
* trusted circles strácajú zmysel.

### C. Úplne izolované modely bez synchronizácie

**Neprijaté ako default**, pretože:

* stráca sa možnosť spolupráce,
* horší cold start,
* menší benefit trusted circles a desktop uzlov.

## Riziká

### 1. Romantizácia individuality

Je ľahké podceniť, ako veľmi systém skomplikuje každý pokus chrániť osobnú identitu modelu.

### 2. Príliš slabý shared layer

Ak bude shared foundation príliš slabý, synchronizácia nebude mať reálny efekt.

### 3. Produktový chaos

Ak používateľ dostane príliš veľa granular settings, architektúra bude síce čistá, ale UX zlyhá.

### 4. Falošný pocit kontroly

Ak systém deklaruje „vlastníš model“, ale v praxi ho externé updatey silno menia, dôvera sa rozpadne.

## Mitigácie

* zaviesť pevné režimy Solo / Trusted Circle / Community Assist,
* držať merge konzervatívny,
* zaviesť explicitné oddelenie `foundation` a `personal delta`,
* mať jasné acceptance testy,
* mať rollback mechanizmus,
* používateľovi ukazovať zrozumiteľné stavy modelu a syncu.

## Súvisiace ADR

* ADR-002: Modulárna architektúra viacerých modelov
* ADR-003: Selektívna federovaná synchronizácia
* ADR-004: Model dôveryhodných kruhov
* ADR-007: Konzervatívny merge
* ADR-008: Foundation + Personal Delta
* ADR-009: Heterogénne uzly
* ADR-014: Rollback mechanizmus

## Poznámka pre implementáciu

Toto ADR je zakladajúce.
Ak sa neskôr zmení, bude mať dopad prakticky na celý systém:

* sync protokol,
* versioning,
* UX režimy,
* trust model,
* model registry,
* merge engine,
* audit a observability.

Preto sa má považovať za jedno z najvyššie nadradených architektonických rozhodnutí.

---

Ak chceš, spravím ti hneď aj:

* **ADR-003 Selektívna federovaná synchronizácia**
  alebo
* **ADR-007 Validation Gate**
  alebo
* **ADR-009 Heterogénne uzly mobil vs desktop**

Tieto tri na ADR-001 priamo nadväzujú.
