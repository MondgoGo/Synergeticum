# ADR-008 — Data Sources Strategy

## Status

Proposed

## Kontext

Personal AI v Synergetiku sa učí zo správania používateľa (ADR-006) a je reprezentovaná ako viacvrstvový profil (ADR-007).

Kľúčová otázka je:

> Z akých dát sa systém môže učiť a z akých nie?

Táto otázka je kritická, pretože:

* priamo ovplyvňuje dôveru používateľov,
* určuje kvalitu Personal AI,
* má právne dôsledky (GDPR, súkromie),
* ovplyvňuje adopciu systému.

Bez jasnej stratégie hrozí:

* príliš agresívne zbieranie dát (nedôvera),
* príliš málo dát (slabý model),
* nekonzistentné správanie systému.

---

## Rozhodnutie

V systéme Synergetikum platí:

> Personal AI sa učí primárne z vedome poskytnutých a pozorovaných interakcií v rámci systému, pričom externé zdroje sú voliteľné a striktne kontrolované.

Dátové zdroje sú rozdelené do 3 kategórií:

1. **Primary (core) data sources**
2. **Secondary (implicit) data sources**
3. **Optional external data sources**

---

## 1. Primary Data Sources (core)

Toto je hlavný a povinný zdroj učenia.

Obsahuje:

* odpovede na otázky
* výber možností
* textové odpovede
* explicitné rozhodnutia

Charakteristika:

* vedomé
* kontrolované používateľom
* najvyššia dôvera

Toto je základ Personal AI.

---

## 2. Secondary Data Sources (implicit behavior)

Toto sú odvodené signály zo správania v systéme.

Obsahuje:

* preskočenie otázky
* čas odpovede
* frekvencia používania
* návrat k téme
* dĺžka engagementu

Charakteristika:

* nepriame
* menej presné
* dopĺňajú primary dáta

Použitie:

* jemné doladenie profilu
* detekcia trendov

---

## 3. Optional External Data Sources

Toto sú externé zdroje mimo systému.

Príklady:

* sociálne siete
* správy / obsah, ktorý používateľ konzumuje
* zdravotné dáta (napr. wearables)
* poznámky / dokumenty

Charakteristika:

* vždy voliteľné (opt-in)
* granularita kontroly
* najvyššie riziko z pohľadu súkromia

---

## Pravidlá pre externé dáta

Externé dáta sa môžu použiť len ak:

* používateľ dá explicitný súhlas
* rozsah je jasne definovaný
* spracovanie je transparentné
* dáta sú spracované lokálne (ak je to možné)

Zakázané:

* tiché napojenie na externé zdroje
* automatické scrapingy
* skryté profilovanie

---

## Data Minimization Principle

Platí:

> Systém zbiera len toľko dát, koľko je potrebné na funkciu.

To znamená:

* preferovať jednoduché signály pred komplexnými dátami
* nepoužívať dáta „len pre istotu"
* pravidelne hodnotiť užitočnosť dát

---

## Data Trust Hierarchy

Nie všetky dáta majú rovnakú váhu:

1. Explicitné odpovede → najvyššia váha
2. Implicitné správanie → stredná váha
3. Externé dáta → variabilná váha (nižšia defaultne)

Tento princíp musí byť zohľadnený v learning modeli.

---

## Dôsledky

### Pozitíva

* vysoká dôvera používateľov
* transparentnosť
* kontrola nad dátami

### Negatíva

* pomalší štart modelu
* závislosť na interakcii používateľa

---

## Architektonické implikácie

### Data ingestion

* oddelené pipeline pre primary, secondary a external dáta

### Permission layer

* centralizované riadenie súhlasu

### Storage

* označenie zdroja dát (source tagging)

### Learning

* rôzne váhy signálov podľa typu zdroja

---

## Alternatívy

### Využiť maximum externých dát (social scraping)

Zamietnuté.

Dôvod:

* strata dôvery
* právne riziká

### Čisto explicitný model bez implicitných dát

Zamietnuté.

Dôvod:

* slabšia adaptácia

---

## Väzby na ďalšie ADR

* ADR-004 — Privacy-first Architecture
* ADR-005 — Personal AI Model Definition
* ADR-006 — Learning Model
* ADR-007 — User Profile Representation
* ADR-015 — Participation Filter Model

---

## Zhrnutie

Personal AI sa učí primárne z vedomých interakcií používateľa.

Externé dáta sú len doplnok, nie základ.

---

## Finálna veta

> Synergetikum buduje inteligenciu z toho, čo používateľ vedome robí, nie z toho, čo o ňom potajme zbiera.
