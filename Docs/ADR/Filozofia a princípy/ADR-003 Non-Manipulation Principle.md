# ADR-003 — Non-Manipulation Principle

## Status

Proposed

## Kontext

Synergetikum pracuje s personalizovanými modelmi, ktoré ovplyvňujú, aké otázky, perspektívy a informácie používateľ vidí. To prináša riziko skrytého ovplyvňovania (manipulácie), napríklad:

* potláčanie alternatívnych pohľadov,
* preferovanie jedného smeru bez transparentného dôvodu,
* využívanie personalizácie na nenápadné „tlačenie“ používateľa,
* skryté optimalizácie na metriky systému namiesto záujmu používateľa.

Takéto správanie by podkopalo dôveru a bolo by v rozpore s cieľom systému – podporovať samostatné myslenie a spoluprácu.

## Rozhodnutie

V systéme Synergetikum platí princíp:

> Systém nesmie manipulovať používateľa.

Systém môže:

* personalizovať poradie tém a perspektív,
* vysvetliť, prečo je niečo zobrazené,
* zvýrazniť konflikty, riziká a nejasnosti,
* ponúknuť viacero alternatívnych pohľadov,
* klásť doplňujúce a reflexné otázky.

Systém nesmie:

* skrývať relevantné alternatívy bez vysvetlenia,
* uprednostňovať jeden smer bez transparentného zdôvodnenia,
* optimalizovať obsah na „presvedčenie“ používateľa,
* používať dark patterns (nátlakové UI, framing bez kontextu),
* prezentovať odporúčania ako záväznú pravdu.

## Dôsledky

* Personalizácia musí byť vysvetliteľná („prečo to vidím“),
* používateľ musí mať prístup k alternatívnym perspektívam,
* systém musí umožniť jednoduché prepnutie pohľadu (napr. iné perspektívy, nižšia/vyššia abstrakcia),
* každý významný návrh musí byť doplnený o protiargumenty alebo riziká,
* žiadne skryté optimalizačné ciele, ktoré by menili obsah bez vedomia používateľa.

## Architektonické implikácie

### Question & Perspective System

* každá otázka môže mať viac perspektív (praktická, analytická, systémová, akčná),
* UI musí umožniť zobraziť aj „ďalšie pohľady“.

### View Layer

* personalizácia = poradie a zvýraznenie, nie filtrácia reality,
* poskytovať sekciu „čo ešte stojí za zváženie“.

### Recommendation/Simulation

* každé odporúčanie obsahuje: predpoklady, neistoty, alternatívy,
* simulácie neoznačovať ako pravdu, ale ako scenáre.

### Audit & Explainability

* logovať dôvody výberu otázok/perspektív,
* umožniť debug pohľad (prečo bolo niečo vybrané).

## Alternatívy

### Optimalizácia na engagement/konverziu

Zamietnuté.
Dôvod: vedie k skrytému ovplyvňovaniu a strate dôvery.

### Silná kurácia obsahu (filter-bubble)

Zamietnuté.
Dôvod: znižuje kvalitu rozhodovania a pluralitu názorov.

## Väzby na ďalšie ADR

* ADR-002 — Human-in-the-loop Decision Model
* ADR-009 — Question-based Interaction Model
* ADR-010 — Perspective System
* ADR-013 — Reflection Loop Design
* ADR-027 — Project View & Simplification Model

## Zhrnutie

Systém môže personalizovať cestu používateľa cez informácie, ale nesmie manipulovať výsledok jeho rozhodovania.

## Finálna veta

> Synergetikum ukazuje možnosti a dôsledky, ale nikdy nesmie skryto tlačiť používateľa k jedinému záveru.
