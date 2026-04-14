# ADR-002 — Human-in-the-loop Decision Model

## Status

Proposed

## Kontext

Synergetikum je systém osobných digitálnych inteligencií, ktorý má používateľovi pomáhať myslieť, orientovať sa v komplexných témach, vyhodnocovať perspektívy, sumarizovať diskusie a simulovať možné dôsledky rozhodnutí.

Keďže systém pracuje s osobnými modelmi, otázkami, topicmi, projektmi, argumentmi a odporúčaniami, vzniká riziko, že by AI mohla byť postupne vnímaná ako autorita alebo dokonca ako aktér, ktorý robí rozhodnutia namiesto človeka.

Takýto model je nežiaduci z viacerých dôvodov:

* znižuje autonómiu používateľa,
* oslabuje dôveru v systém,
* zvyšuje riziko manipulácie,
* vytvára nejasnú hranicu zodpovednosti,
* je v rozpore so základnou filozofiou Synergetika.

Systém preto potrebuje explicitne definovať, akú rolu AI má a akú rolu naopak nikdy nesmie prevziať.

## Rozhodnutie

V systéme Synergetikum platí princíp:

> AI nikdy nerozhoduje za človeka.

AI môže:

* pomáhať formulovať otázky,
* sumarizovať vstupy a diskusie,
* simulovať možné scenáre,
* ukazovať alternatívne perspektívy,
* zvýrazňovať konflikty, riziká a príležitosti,
* klásť reflexné otázky späť používateľovi,
* odporúčať ďalšie oblasti na premyslenie.

AI nesmie:

* robiť finálne rozhodnutie,
* konať v mene používateľa bez explicitného poverenia,
* skrývať relevantné alternatívy s cieľom dotlačiť používateľa k určitému smeru,
* vytvárať dojem, že jej výstup je záväzná pravda,
* nahrádzať ľudský úsudok pri hodnotových, osobných alebo kolektívnych rozhodnutiach.

Finálne rozhodnutie je vždy ľudské.

## Dôsledky

Toto rozhodnutie znamená, že:

* Personal AI je poradná a reflexná vrstva, nie riadiaca autorita,
* všetky odporúčania musia byť navrhnuté ako podporujúce myslenie, nie ako príkazy,
* používateľ musí mať možnosť nesúhlasiť, ignorovať alebo zmeniť smer bez penalizácie,
* systém musí zostať vysvetliteľný a transparentný v tom, prečo niečo navrhuje,
* zodpovednosť za rozhodnutie nesie používateľ alebo kolektív ľudí, nie AI.

## Architektonické implikácie

Tento princíp má dopad na viaceré vrstvy systému:

### Personal AI

Personal AI môže vytvárať profil preferencií a návrhy, ale tieto návrhy nesmú byť záväzné.

### Question System

Otázky generované AI majú rozširovať uvažovanie, nie navádzať na jedinú „správnu“ odpoveď.

### Topic Layer

AI môže pomáhať rozpoznať, že z diskusie vzniká zaujímavá téma, ale nesmie sama deklarovať vznik projektu ako autoritatívne rozhodnutie.

### Project Layer

AI môže pomôcť štruktúrovať projekt, vetvy, argumenty a podporu, ale nesmie sama vybrať „víťaznú“ vetvu.

### View Layer

AI môže personalizovať pohľad na realitu projektu, ale nesmie používateľovi zatajiť existenciu relevantných alternatív.

## Alternatívy

### AI ako rozhodovací agent

Zamietnuté.

Dôvod:

* narušenie autonómie človeka,
* vysoké riziko manipulácie,
* nejasná zodpovednosť,
* filozofický rozpor so systémom.

### AI ako čisto pasívny nástroj bez odporúčaní

Tiež zamietnuté.

Dôvod:

* systém by stratil veľkú časť svojej hodnoty,
* používateľ by nedostal podporu pri orientácii v komplexite,
* znížila by sa personalizácia a schopnosť reflexie.

## Väzby na ďalšie ADR

Tento ADR úzko súvisí najmä s:

* ADR-001 — Personal AI Ownership Model
* ADR-003 — Non-Manipulation Principle
* ADR-009 — Question-based Interaction Model
* ADR-013 — Reflection Loop Design
* ADR-025 — Argumentation & Reasoning Model
* ADR-027 — Project View & Simplification Model
* ADR-034 — Personal AI Internal Model

## Zhrnutie

AI v Synergetiku má byť nástrojom rozšíreného myslenia, nie náhradou ľudského rozhodovania.

Jej úlohou je pomáhať človeku lepšie vidieť, lepšie rozumieť a lepšie sa pýtať — nie rozhodnúť namiesto neho.

## Finálna veta

> AI môže sprevádzať rozhodovanie, ale nikdy nesmie prevziať rolu rozhodovateľa.
