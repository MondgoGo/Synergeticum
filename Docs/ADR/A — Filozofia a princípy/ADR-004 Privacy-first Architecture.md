# ADR-004 — Privacy-first Architecture

## Status

Proposed

## Kontext

Synergetikum pracuje s vysoko citlivými dátami používateľa:

* osobné názory a rozhodnutia,
* behaviorálne vzorce,
* preferencie a hodnoty,
* potenciálne zdravotné alebo kontextové informácie.

Zároveň systém využíva personalizované AI modely, ktoré sa učia priamo z týchto dát.

Bez silného dôrazu na súkromie by vznikli zásadné riziká:

* strata dôvery používateľov,
* zneužitie dát (externé aj interné),
* centralizované profilovanie,
* regulačné problémy (GDPR a pod.),
* bezpečnostné incidenty s vysokým dopadom.

Preto musí byť ochrana súkromia základným architektonickým princípom, nie doplnkom.

## Rozhodnutie

V systéme Synergetikum platí princíp:

> Súkromie je defaultný stav systému (privacy by default & by design).

Základné pravidlá:

* dáta ostávajú primárne lokálne (on-device),
* Personal AI model je súkromný a nezdielaný bez explicitného súhlasu,
* zdieľanie je vždy voliteľné a riadené používateľom,
* minimalizuje sa množstvo prenášaných dát,
* nikdy sa nezdieľajú raw osobné dáta bez jasného dôvodu,
* systém preferuje anonymizované alebo agregované formy znalostí.

## Dôsledky

* používateľ má kontrolu nad tým, čo sa zdieľa a s kým,
* default režim je Solo (žiadne zdieľanie),
* všetky sync mechanizmy musia rešpektovať PermissionLayer,
* systém musí fungovať plnohodnotne aj bez akéhokoľvek zdieľania,
* každé zdieľanie musí byť auditovateľné a vysvetliteľné.

## Architektonické implikácie

### On-device first

* všetky modely bežia primárne lokálne,
* tréning aj inferencia prebieha na zariadení,
* LocalFeatureStore a ModelStore sú základné komponenty.

### Permission Layer

* explicitne riadi, čo sa môže zdieľať,
* obsahuje jednoduché režimy (Solo, Trusted Circle, Community Assist),
* pokročilé nastavenia sú voliteľné.

### Sync & Sharing

* žiadne automatické globálne zdieľanie,
* zdieľajú sa len povolené artefakty (napr. embeddings, agregované štatistiky),
* raw dáta sú defaultne blokované.

### Trust Graph

* zdieľanie je viazané na dôveru medzi peer-mi,
* peer musí byť explicitne schválený,
* rôzne úrovne dôvery ovplyvňujú merge váhu.

### Validation Gate

* každý externý update musí byť validovaný,
* ochrana proti poisoning a nekvalitným updateom.

### Local-first UX

* používateľ nemusí byť online,
* všetky core funkcie fungujú offline,
* sync je bonus, nie nutnosť.

## Alternatívy

### Centralized data storage

Zamietnuté.

Dôvod:

* vysoké riziko zneužitia,
* slabá dôvera,
* regulačné problémy,
* strata hlavnej hodnoty systému.

### Always-on data sharing

Zamietnuté.

Dôvod:

* porušenie súkromia,
* neakceptovateľné pre väčšinu používateľov,
* zbytočné sieťové a energetické náklady.

## Väzby na ďalšie ADR

* ADR-001 — Personal AI Ownership Model
* ADR-003 — Non-Manipulation Principle
* ADR-014 — Network Communication Model (P2P)
* ADR-015 — Participation Filter Model
* ADR-030 — Sync & Persistence Strategy
* ADR-021 — Parent Node Architecture

## Zhrnutie

Súkromie nie je feature, ale základná vlastnosť systému.

Architektúra je navrhnutá tak, aby:

* chránila osobné dáta,
* minimalizovala zdieľanie,
* a zároveň umožnila kontrolovanú spoluprácu.

## Finálna veta

> Synergetikum chráni používateľa tým, že jeho dáta ostávajú jeho vlastníctvom a pod jeho kontrolou.
