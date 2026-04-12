# System Architecture Overview (End-to-End)

## Personal AI Network (PAN)

---

# 1. Účel systému

Personal AI Network je distribuovaný systém osobných digitálnych inteligencií, ktorého cieľom je:

* pomáhať jednotlivcovi lepšie sa rozhodovať,
* premieňať individuálne myslenie na kolektívnu inteligenciu,
* premieňať kolektívnu inteligenciu na projekty a spoluprácu,
* a to všetko bez straty súkromia a bez centrálneho riadenia.

Systém nestojí na tom, že jedna centrálna AI vie všetko.
Stojí na tom, že:

👉 každý človek má vlastnú Personal AI
👉 tieto AI medzi sebou komunikujú v P2P sieti
👉 a vďaka tomu vzniká sieť myslenia, otázok, projektov a akcií

---

# 2. Základná filozofia systému

Systém stojí na týchto pilieroch:

## 2.1 Personal AI je lokálna a súkromná

Každý používateľ má vlastný AI model, ktorý žije na jeho zariadení, primárne na mobile a voliteľne aj na stolnom počítači.

## 2.2 Človek je finálny rozhodovateľ

AI pomáha premýšľať, sumarizovať, simulovať a klásť otázky, ale nerozhoduje za človeka.

## 2.3 Sieť je peer-to-peer

Modely medzi sebou komunikujú priamo alebo cez ľahké pomocné vrstvy, ale neexistuje centrálna inteligencia, ktorá by vlastnila projekt, ľudí alebo rozhodnutia.

## 2.4 Projekt je živý objekt

Projekt nevzniká ako statický dokument. Vzniká ako distribuovaný, rozvetvený objekt, ktorý sa vyvíja v čase.

## 2.5 Spolupráca je rovnako dôležitá ako myslenie

Cieľom systému nie je len pomáhať človeku myslieť. Cieľom je pomáhať ľuďom prepájať sa a vytvárať spoločné aktivity, projekty a riešenia.

---

# 3. Hlavné entity systému

Celý systém sa dá pochopiť cez niekoľko hlavných entít.

## 3.1 Používateľ

Reálny človek, ktorý:

* odpovedá na otázky,
* vyberá si, čo ho zaujíma,
* rozhoduje sa,
* a môže sa zapájať do projektov.

## 3.2 Personal AI

Lokálny AI model patriaci konkrétnemu používateľovi.
Reprezentuje:

* jeho spôsob myslenia,
* jeho záujmy,
* jeho preferencie,
* jeho históriu rozhodnutí.

## 3.3 Parent Node

Voliteľné rozšírenie Personal AI na výkonnom zariadení, napríklad na počítači.
Je to stále osobná inteligencia toho istého človeka, len s väčším priestorom na dlhodobé učenie, simulácie a hlbšiu analýzu.

## 3.4 Otázka

Základná vstupná jednotka systému.
Otázka môže byť:

* osobná,
* komunitná,
* projektová,
* systémová,
* realizačná,
* alebo hodnotová.

## 3.5 Perspektíva

Každá otázka môže byť rozložená na viac uhlov pohľadu, napríklad:

* praktický,
* realizačný,
* analytický,
* systémový,
* hodnotový.

## 3.6 Projekt

Distribuovaný kolektívny objekt, ktorý vzniká z otázky alebo iniciatívy človeka a postupne sa rozvíja cez diskusiu, vetvenie, podporu a spoluprácu.

## 3.7 Vetva projektu

Konkrétny variant riešenia v rámci projektu.

## 3.8 Argument

Štruktúrovaná jednotka myslenia pri konceptuálnych projektoch.
Obsahuje:

* tvrdenie,
* zdôvodnenie,
* predpoklady,
* dôsledky,
* protiargumenty.

## 3.9 Support signal

Viacrozmerný signál podpory vetvy:

* alignment,
* commitment,
* confidence,
* prípadne expertízny signál.

---

# 4. Architektonické vrstvy systému

Celý systém je najlepšie chápať ako viacvrstvovú architektúru.

---

## 4.1 Personal Intelligence Layer

Táto vrstva žije na zariadení používateľa.

Obsahuje:

* Personal AI model,
* lokálnu históriu odpovedí,
* lokálne preferencie,
* interný profil spôsobu myslenia,
* lokálnu interpretáciu projektov a otázok.

Úloha tejto vrstvy:

* učiť sa z interakcií používateľa,
* vyberať relevantné otázky,
* filtrovať komplexitu,
* interpretovať projekty podľa používateľa,
* rozhodovať o zapojení do siete.

Táto vrstva je základom súkromia aj identity.

---

## 4.2 Interaction Layer

Táto vrstva reprezentuje spôsob, akým človek vstupuje do systému.

Obsahuje:

* feed otázok,
* možnosť označiť, čo ma zaujíma,
* odpovedanie na otázky,
* zapájanie sa do projektov,
* reflexné otázky od AI.

Úloha tejto vrstvy:

* vytvárať kvalitný vstup do učenia Personal AI,
* umožniť používateľovi vedomé zapájanie,
* premeniť pasívne scrollovanie na aktívne myslenie.

---

## 4.3 Question & Perspective Layer

Táto vrstva spracúva otázky a mení ich na použiteľné kognitívne objekty.

Obsahuje:

* pôvodnú otázku,
* kontext otázky,
* rozpad na perspektívy,
* poradie perspektív podľa používateľa.

Úloha tejto vrstvy:

* zachovať pôvodný význam otázky,
* umožniť viaceré spôsoby pochopenia,
* personalizovať poradie pohľadov bez manipulácie.

Dôležitý princíp:

* nemení sa pravda otázky,
* mení sa len poradie a spôsob vstupu.

---

## 4.4 Participation Layer

Táto vrstva rozhoduje, či sa konkrétna Personal AI zapojí do konkrétnej otázky alebo projektu.

Obsahuje:

* participation filter,
* relevanciu,
* schopnosť prispieť,
* lokálny threshold zapojenia,
* typ participácie.

Úloha:

* zabrániť preťaženiu siete,
* zabrániť šumu,
* zabezpečiť, že odpovedajú len relevantné uzly.

Toto je kľúčové pre škálovanie.

---

## 4.5 P2P Network Layer

Táto vrstva prepája jednotlivé Personal AI do siete.

Obsahuje:

* peer-to-peer komunikáciu,
* collective query mechanizmus,
* distribúciu otázok,
* výmenu agregovaných odpovedí,
* discovery a routing mechanizmy.

Úloha:

* vytvoriť decentralizovanú sieť inteligencií,
* umožniť kolektívne myslenie bez centrálneho servera,
* prenášať perspektívy, nie dáta.

Táto vrstva neslúži na prenos surových osobných dát.
Slúži na prenos:

* odpovedí,
* agregovaných signálov,
* podpory,
* argumentov,
* projektových zmien.

---

## 4.6 Project Formation Layer

Táto vrstva premieňa kolektívne otázky a odpovede na projektové objekty.

Obsahuje:

* detekciu vznikajúceho zámeru,
* vytváranie projektov,
* Living Project Document,
* identifikáciu ľudí, ktorí sa môžu zapojiť.

Úloha:

* premeniť diskusiu na konkrétny smer,
* identifikovať, kedy už nejde len o názor, ale o zárodok projektu,
* umožniť, aby z myslenia vznikala realita.

Projekt môže vzniknúť:

* emergentne z diskusie,
* alebo manuálne iniciáciou jedného človeka.

Po vzniku však žije kolektívne.

---

## 4.7 Distributed Project Object Layer

Táto vrstva definuje, kde a ako projekt existuje.

Obsahuje:

* distribuovaný projektový objekt,
* replikáciu medzi uzlami,
* identitu projektu,
* historickú kontinuitu,
* offline prežívanie.

Úloha:

* zabezpečiť, že projekt nie je naviazaný na jedno zariadenie,
* umožniť, aby projekt prežil aj odchod iniciátora,
* udržať projekt ako živú entitu siete.

Projekt:

* môže vzniknúť lokálne,
* ale akonáhle sa stane kolektívnym, žije distribuovane.

---

## 4.8 Branching & Governance Layer

Táto vrstva rieši konflikty, varianty a evolúciu projektu.

Obsahuje:

* vetvy projektu,
* branching,
* governance mode,
* lifecycle vetiev,
* support a alignment model.

Úloha:

* umožniť, aby pri konflikte nevznikol boj o jednu pravdu,
* ale aby vznikli paralelné varianty,
* ktoré môžu ďalej žiť, slabnúť alebo sa aktivovať.

Táto vrstva je zásadná najmä pri otázkach, kde neexistuje jedna samozrejmá odpoveď.

---

## 4.9 Argumentation Layer

Táto vrstva sa používa pri konceptuálnych, hodnotových a normatívnych projektoch.

Obsahuje:

* argumenty,
* protiargumenty,
* štruktúru tvrdenie–zdôvodnenie–dôsledky,
* argument graph.

Úloha:

* oddeliť argument od osoby,
* stabilizovať diskusiu,
* zvýhodniť kvalitu uvažovania pred hlasitosťou,
* umožniť deliberáciu bez chaosu.

Táto vrstva je kritická napríklad pri:

* pravidlách,
* ústavných návrhoch,
* stratégiách,
* dlhodobých spoločenských projektoch.

---

## 4.10 View & Simplification Layer

Táto vrstva rieši najväčší praktický problém systému:

* komplexitu.

Obsahuje:

* personalizovaný pohľad na projekt,
* výber relevantných vetiev,
* sumarizáciu,
* zvýraznenie konfliktov,
* zjednodušený vstup do komplexnej reality.

Úloha:

* zabezpečiť, aby človek nemusel vidieť celý projektový strom,
* ale len tú časť, ktorá je pre neho relevantná a pochopiteľná.

Dôležitý princíp:

* Reality Layer obsahuje všetko,
* View Layer ukazuje len to, čo používateľ potrebuje.

---

## 4.11 Opportunity Layer

Táto vrstva prepája myslenie s reálnym zapojením.

Obsahuje:

* návrhy rolí,
* návrhy zapojenia,
* identifikáciu toho, kde vie človek pomôcť.

Úloha:

* premeniť otázku na konkrétnu príležitosť,
* ukázať používateľovi, že nemusí projekt viesť, aby bol jeho súčasťou.

Príklad:

* niekto nemusí založiť pizzériu,
* ale môže ju pomôcť postaviť, investovať, organizovať alebo propagovať.

---

## 4.12 Simulation & Reflection Layer

Táto vrstva robí z AI partnera pre myslenie.

Obsahuje:

* scenárový simulátor,
* realistické a kritické projekcie,
* reflexné otázky,
* sokratovskú spätnú väzbu.

Úloha:

* ukazovať možné budúcnosti,
* vracať otázky späť človeku,
* učiť sa z jeho hodnôt a rozhodnutí.

Kľúčový princíp:

* AI neradí autoritatívne,
* AI rozširuje pole rozhodovania.

---

# 5. End-to-end toky systému

Nižšie sú najdôležitejšie procesné toky.

---

## 5.1 Individuálny tok

```text
Otázka → výber otázky → odpoveď používateľa → update Personal AI → lepšia personalizácia
```

Toto je základný mechanizmus rastu digitálnej inteligencie.

---

## 5.2 Kolektívny tok

```text
Otázka → distribúcia cez P2P → participation filter → odpovede modelov → agregácia → pohľad pre používateľa
```

Toto vytvára kolektívnu inteligenciu bez centrálnej autority.

---

## 5.3 Projektový tok

```text
Otázka / iniciatíva → detekcia zámeru → vznik projektu → vznik vetiev → podpora a argumenty → možná aktivácia
```

Toto mení otázky na realitu.

---

## 5.4 Projektový view tok

```text
Komplexný projekt → View Layer → zhrnutie relevantných vetiev → reflexná otázka → rozhodnutie používateľa
```

Toto robí systém použiteľným.

---

## 5.5 Opportunity tok

```text
Projekt → identifikácia potrieb → mapping schopností používateľa → návrh role → zapojenie
```

Toto mení pozorovateľov na účastníkov.

---

## 5.6 Simulačný tok

```text
Zámer / rozhodnutie → simulácia scenárov → dôsledky → otázka späť → update preferencií
```

Toto pomáha premýšľať nad budúcnosťou.

---

# 6. Typy projektov a governance režimy

Systém musí rozlišovať medzi rôznymi typmi projektov.

## 6.1 Realizačné projekty

Príklady:

* pizzéria
* stavba
* lokálna služba

Riadenie:

* commitment,
* zdroje,
* realizácia.

## 6.2 Konsenzuálne projekty

Príklady:

* komunitné pravidlá
* lokálne rozhodnutia

Riadenie:

* širšia zhoda,
* alignment,
* diskusia.

## 6.3 Odborné / konceptuálne projekty

Príklady:

* ústava
* pravidlá systému
* právne rámce

Riadenie:

* kvalita argumentu,
* robustnosť reasoning modelu,
* odborný signál,
* nie len popularita.

---

# 7. Súkromie, dôvera a hranice systému

Systém je použiteľný len vtedy, ak je dôveryhodný.

## 7.1 Čo ostáva lokálne

* dáta používateľa
* interný stav Personal AI
* model preferencií
* jemné osobné odpovede

## 7.2 Čo sa môže dostať do siete

* agregované odpovede
* argumenty
* podpora
* anonymizované projektové signály

## 7.3 Čo systém nesmie robiť

* nesmie centrálne profilovať ľudí,
* nesmie rozhodovať za nich,
* nesmie ich tlačiť jedným smerom,
* nesmie vytvoriť skrytý mocenský uzol.

---

# 8. Čo chceme systémom dosiahnuť

Toto je dôležité povedať explicitne.

Nechceme vytvoriť len:

* nástroj na sebapoznanie,
* ani len AI poradcu.

Chceme vytvoriť systém, ktorý umožní:

## 8.1 Individuálny prínos

* lepšie rozhodovanie,
* väčšie sebauvedomenie,
* lepšie chápanie dôsledkov.

## 8.2 Kolektívny prínos

* viac spolupráce medzi ľuďmi,
* lepšie prepájanie schopností,
* vznik iniciatív, ktoré by inak nevznikli.

## 8.3 Systémový prínos

* novú vrstvu spoločenskej koordinácie,
* decentralizovanú sieť myslenia,
* priestor, kde sa názory, projekty a ľudia môžu stretávať bez toho, aby ich riadila jedna autorita.

---

# 9. Čo systém nie je

Je dôležité explicitne povedať aj to, čo systém nie je.

Systém nie je:

* klasická sociálna sieť,
* klasický chatbot,
* centralizovaná AI služba,
* hlasovací systém,
* nástroj na manipuláciu ľudí,
* ani nástroj, ktorý má určovať „jedinú správnu pravdu“.

---

# 10. Finálna architektonická veta

Personal AI Network je:

👉 **distribuovaný systém osobných digitálnych inteligencií, ktoré sa učia lokálne, komunikujú peer-to-peer, premieňajú otázky na kolektívne myslenie a kolektívne myslenie na projekty, spoluprácu a realitu.**

---

# 11. Odporúčané ďalšie architektonické dokumenty

Na tento overview prirodzene nadväzujú:

* ADR-001 – Personal AI Ownership Model
* ADR-014 – P2P Network Communication Model
* ADR-015 – Participation Filter Model
* ADR-022 – Collective Project Formation
* ADR-023 – Distributed Project Object Model
* ADR-024 – Multi-Variant Governance
* ADR-025 – Argumentation & Reasoning Model
* ADR-027 – Project View & Simplification Model
* ADR-028 – Support & Alignment Model
* ADR-029 – Branch Lifecycle Model

---
