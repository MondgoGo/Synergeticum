# 🧾 ADR-023 — Distributed Project Object Model

## 1. 📌 Status

**Accepted (revízia 2.0)**

---

## 2. 🎯 Kontext

Synergetikum je:

- decentralizovaný systém (ADR-014)
- local-first / sync-later architektúra
- systém, v ktorom projekty nevznikajú len ako lokálne záznamy, ale ako kolektívne udržiavané entity

Na projekty sa už viažu ďalšie vrstvy:

- vznik projektu (ADR-022)
- minimálne existenčné podmienky projektu (ADR-077)
- Topic → Project transition (ADR-078)
- vetvenie a varianty (ADR-024)
- view vrstva (ADR-027)
- sync a infra vrstva (ADR-030, ADR-I-* )

Pôvodný problém znie:

> **Kde projekt existuje a ako je udržiavaný v decentralizovanej sieti?**

To vedie k ďalším praktickým otázkam:

- Je projekt viazaný na jedno zariadenie?
- Ako prežije odchod iniciátora?
- Koľko replík je „dosť“?
- Kedy je projekt zdravý, kedy degradovaný a kedy orphaned?
- Ako sa discovery rieši v pilote?
- Čo je presná dátová štruktúra projektu v sieti?

---

## 3. ❗ Hlavný architektonický problém

Pôvodná formulácia:

> „projekt prežije, ak existuje aspoň jeden uzol, ktorý ho drží“

je technicky pravdivá, ale architektonicky nedostatočná.

Prečo:

- 1 replika znamená extrémne krehký stav
- discovery môže zlyhať
- participanti o projekte nemusia vedieť
- sync môže zostať uviaznutý
- UX môže ukazovať „projekt existuje“, aj keď je prakticky mŕtvy

Podobne formulácia:

> „pravda je výsledok synchronizácie medzi uzlami“

je filozoficky správna, ale bez stavov zdravia a degradácie príliš neurčitá.

---

## 4. ⚖️ Rozhodnutie

> **Projekt je distribuovaný doménový objekt, reprezentovaný ako Project Node + súvisiaci graf entít, replikovaný medzi relevantnými uzlami a synchronizovaný bez centrálneho master node.**

To znamená:

- projekt nie je viazaný na jedno zariadenie
- projekt nemá jediný autoritatívny master node
- projekt môže existovať v rôznych replikačných stavoch
- konflikty sa neriešia overwrite modelom, ale cez vetvenie a neskoršie rozhodovanie (ADR-024)
- discovery a replication sa v pilote zjednodušujú, ale architektúra musí byť pripravená na rast

---

## 5. 🧠 Čo je distribuovaný projekt

Distribuovaný projekt nie je len jeden JSON alebo jeden dokument.

Je to:

- `Project` Node
- `Branch` Node-y
- súvisiace `Argument`, `Event`, `Person`, `Topic` väzby
- relačný / grafový kontext
- história a lineage

Teda:

> **Projekt je distribuovaný projektový graf s koreňovým Project Node a rooted branch lineage.**

Nie je to „strom“ v prísnom zmysle celej entity.

### 5.1 Dôležité spresnenie

- **Branch lineage** môže byť stromová
- ale **celý projektový objekt** je skôr:

> **rooted graph with lineage constraints**

Prečo:
- argumenty referencujú vetvy
- eventy referencujú projekty alebo vetvy
- merge vytvára nelineárne väzby
- references a supports/opposes rozbíjajú čistý strom

---

## 6. 🌐 Replikačný model projektu

### 6.1 Základný princíp

Projekt je replikovaný medzi uzlami, ktoré sú naň relevantne naviazané.

Typicky:
- owner node
- participant nodes
- parent node používateľa
- ďalšie uzly, ktoré sú súčasťou autorizovaného kontextu

### 6.2 Neexistuje master node

Systém NESMIE:
- mať centrálnu autoritatívnu repliku
- mať jediné miesto, ktoré rozhoduje „pravdu“

Ale zároveň:

> **Nie všetky repliky sú rovnako zdravé, rovnako aktuálne alebo rovnako discoverable.**

---

## 7. 🧠 Project survival states

Aby sa zabránilo vágnej formulácii „existuje aspoň na jednom uzle“, zavádzajú sa explicitné stavy prežitia projektu.

### 7.1 Healthy replicated project

Projekt je **healthy replicated**, ak:
- existuje na viacerých relevantných uzloch
- je discoverable pre participantov
- má funkčný sync medzi aspoň podmnožinou nositeľov
- nie je odkázaný len na jeden holder

Typicky:
- 2+ relevantné repliky
- aspoň jedna dostupná participantom
- lineage a core payload konzistentné

### 7.2 Degraded project

Projekt je **degraded**, ak:
- existuje, ale replikačný alebo discoverability stav je oslabený
- napr. niektoré uzly chýbajú
- alebo sync dlhšie neprebehol
- alebo projekt ostal len na malej časti participantov

Projekt v tomto stave:
- ešte žije
- ale je rizikový

### 7.3 Single-holder project

Projekt je **single-holder**, ak:
- existuje prakticky len na jednom relevantnom uzle
- alebo len na jednom uzle existuje jeho jediná aktuálna použiteľná replika

To znamená:
- technicky existuje
- ale produktovo je v kritickom stave

### 7.4 Orphaned project

Projekt je **orphaned**, ak:
- síce existujú nejaké zvyškové dáta alebo replika
- ale projekt už nie je zmysluplne discoverable,
- nemá funkčný okruh participantov,
- alebo stratil svoju sieťovú väzbu na tých, ktorí ho majú niesť

Orphaned projekt:
- môže byť ešte obnoviteľný,
- ale už sa nesmie tváriť ako zdravý aktívny projekt.

---

## 8. 🔄 Lifecycle distribuovanej existencie

### Fáza 1 — Local creation

Projekt vzniká lokálne:
- typicky po formation / transition procese
- na jednom uzle vznikne prvá platná replika

### Fáza 2 — Initial replication

Projekt sa replikuje:
- na ownerov parent node, ak existuje
- na potvrdených participant nodes
- alebo na iné relevantné nosiče

### Fáza 3 — Collective existence

Projekt už nie je len lokálny záznam.
Je:
- kolektívne držaný
- distribuovaný
- discoverable v príslušnom okruhu participantov

### Fáza 4 — Degradation / Dormancy / Orphaning

Ak:
- participanti miznú
- sync sa rozpadne
- discovery prestane fungovať
- projekt ostane len na minimálnom počte uzlov

prechádza do:
- degraded
- single-holder
- alebo orphaned režimu

---

## 9. 🔍 Discovery model

Discovery je veľmi dôležitý a v pôvodnej verzii bol príliš všeobecný. :contentReference[oaicite:1]{index=1}

### 9.1 Pilotný discovery model

Pre pilot platí zjednodušenie:

> **Discovery sa deje len medzi participant nodes a owner/parent contextom.**

To znamená:

- žiadne open-network všeobecné hľadanie
- žiadne globálne lookupy všetkých projektov
- bootstrap je cez:
  - participant list
  - owner node
  - parent node
  - explicitné odkazy

### 9.2 Budúci discovery model

Do budúcna môže pribudnúť:
- širší P2P lookup
- indexované discovery
- trust-aware discovery

Ale nie je to povinné pre pilot.

### 9.3 Dôležitý princíp

> **Discovery pre pilot má byť úzky, predvídateľný a auditovateľný.**

Nie otvorený a „magický“.

---

## 10. 🔄 Synchronizácia projektu

Projekt sa synchronizuje ako distribuovaný graf entít.

To znamená:
- sync sa netýka len root Project Node
- ale aj:
  - branch lineage
  - argumentov
  - eventov
  - participant väzieb
  - relevantných relations

### 10.1 Pravda v systéme

Pravda nie je:
- centrálny master
- ani „prvá kópia, ktorá prišla“

Pre túto vrstvu platí:

> **Pravda je výsledok verziovaného, auditovateľného a eventual-consistent sync procesu nad viacerými replikami.**

### 10.2 Dôležité spresnenie

To neznamená:
- že všetky uzly vidia to isté v rovnakom čase
- ani že každá replika je rovnako kvalitná

Preto sa zavádzajú survival / health stavy.

---

## 11. 🔀 Konflikty a vetvenie

Konflikty sa v projekte neriešia:
- posledným zápisom,
- overwrite,
- ani „autoritou jedného uzla“

Konflikty sa riešia:
- vetvením
- a neskoršou evaluáciou / merge / alignment procesom

### Princíp

> **Conflict does not destroy the project object. It creates structure inside it.**

Na to nadväzuje ADR-024.

---

## 12. 👤 Vlastníctvo vs nositeľstvo

Projekt:
- má ownera / iniciátora / participantov
- ale nie je ich „lokálnym súborom“

Treba rozlišovať:

| Pojem | Význam |
|-------|--------|
| **owner** | rola v projekte |
| **holder** | uzol, ktorý nesie repliku |
| **participant** | človek zapojený do projektu |
| **replica carrier** | konkrétny technický nosič projektu |

To je dôležité, lebo:
- owner nemusí byť jediný holder
- participant nemusí byť držiteľ kompletnej repliky
- parent node môže byť holder bez toho, aby bol participantom

---

## 13. 🧱 Minimálna distribuovaná identita projektu

Každý distribuovaný projekt musí mať aspoň:

- unikátne `project_id`
- lineage / pôvod
- replication state
- discovery path
- core project payload
- minimal participant context

### Pre architektúru to znamená

Project object musí byť schopný odpovedať na otázky:

- kto som?
- odkiaľ som vznikol?
- kde približne existujem?
- v akom replikačnom stave som?
- kto ma môže nájsť?

---

## 14. 🔐 Perzistencia a strata

Pôvodné tvrdenie:
> projekt prežije, ak existuje aspoň jeden uzol, ktorý ho drží

sa nahrádza presnejšou formuláciou:

> **Projekt technicky existuje, ak existuje aspoň jedna použiteľná replika.  
> Projekt je však považovaný za zdravý iba vtedy, ak spĺňa minimálne replikačné a discovery podmienky.**

### 14.1 Dôsledky

| Stav | Význam |
|------|--------|
| 1 použiteľná replika | technická existencia |
| 2+ relevantné repliky | základná odolnosť |
| chýba discovery | vysoké riziko orphaningu |
| žiadna replika | zánik projektu |

---

## 15. 🚫 Čo sa NESMIE stať

| Zakázaná situácia | Prečo |
|-------------------|-------|
| projekt sa tvári ako zdravý, ale existuje len na jednom zariadení | falošný pocit odolnosti |
| discovery je v pilote open-ended a neauditovateľné | príliš vysoká komplexita |
| celý projekt sa modeluje ako čistý strom | nezvládne merges a references |
| replikačný stav nie je explicitne modelovaný | nemožno rozlíšiť healthy vs degraded |
| owner = jediný holder | centralizačný failure mode |

---

## 16. 🧠 Dôsledky rozhodnutia

### ✅ Pozitíva

| Dôsledok | Popis |
|----------|-------|
| realistickejší model prežitia | projekt nie je binárne „je / nie je“ |
| lepšia pripravenosť na budúcnosť | zdravie, degradácia, orphaning sú modelované |
| pilot je jednoduchší | participant-based discovery |
| menší architektonický dlh | nepredstierame tree tam, kde je graph |
| lepšia observabilita | dá sa sledovať, v akom stave projekt existuje |

### ❗ Negatíva / Trade-offs

| Dôsledok | Mitigácia |
|----------|-----------|
| vyššia pojmová komplexita | ale presnejšia architektúra |
| viac stavov na modelovanie | oplatí sa kvôli robustnosti |
| pilot nebude implementovať plnú distribuovanú inteligenciu | to je v poriadku |

---

## 17. 🚫 Alternatívy (zamietnuté)

### ❌ Centrálne uložený projekt
Zamietnuté, lebo:
- jednoduchý, ale centralizovaný
- vytvára single point of failure

### ❌ Projekt viazaný na iniciátora
Zamietnuté, lebo:
- intuitívne, ale krehké
- odchod iniciátora ohrozuje existenciu

### ❌ Čisto stromová reprezentácia
Zamietnuté, lebo:
- branch lineage môže byť stromová,
- ale celý projekt je grafová štruktúra

---

## 18. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-014 | sieťový substrate |
| ADR-022 | formation vytvára projekt, ktorý sa má začať distribuovať |
| ADR-024 | konflikty sa materializujú ako vetvy |
| ADR-027 | view vrstva musí vedieť zohľadniť health/discovery stav |
| ADR-030 | sync a perzistencia riešia mechaniku |
| ADR-042 | projekt je Project Node + graf nad Node modelom |
| ADR-077 | projekt musí byť existenčne validný |
| ADR-078 | transition Topic → Project vytvára počiatočný distribuovaný objekt |

---

## 19. 📏 Pravidlá implementácie

Systém musí:

1. modelovať projekt ako distribuovaný objekt, nie lokálny dokument
2. odlíšiť technickú existenciu od zdravého replikačného stavu
3. zaviesť stavy:
   - `healthy_replicated`
   - `degraded`
   - `single_holder`
   - `orphaned`
4. pre pilot používať participant-based discovery
5. nepredpokladať open-network discovery v prvej verzii
6. modelovať projekt ako rooted graph with branch lineage constraints
7. neviazať prežitie projektu len na ownera
8. byť pripravený na to, že pilot neimplementuje všetku budúcu distribuovanú komplexitu, ale model ju musí vedieť uniesť

---

## 20. 🧪 Príklady

### Healthy replicated project
- replika na owner node
- replika na parent node
- replika aspoň na jednom participant node
- project je discoverable participantom

### Degraded project
- replika len na owner node a parent node
- participant node ju už nemajú
- discovery funguje obmedzene

### Single-holder project
- jediná použiteľná replika ostala na jednom uzle
- projekt technicky existuje, ale je kriticky ohrozený

### Orphaned project
- existuje fragment alebo stará replika
- discovery participantom už nefunguje
- projekt sa nesmie tváriť ako aktívne zdravý

---

## 21. 🔥 Zhrnutie

> **Projekt nie je súbor na jednom zariadení. Je to distribuovaný projektový graf, ktorý môže byť zdravý, oslabený alebo takmer stratený — a architektúra to musí vedieť rozlíšiť.**

---

## 22. 🧭 Finálna veta

> **Projekt nežije len preto, že niekde existuje jedna kópia.  
> Žije vtedy, keď má nositeľov, discovery a replikačný stav dostatočný na to, aby ho komunita dokázala ďalej niesť.**
```

---

Toto je verzia, ktorú by som architektovi poslal.

Najdôležitejšie zlepšenia oproti pôvodnej verzii sú:

* explicitné **survival thresholds / states**
* pilotne zjednodušený **discovery model**
* opustenie nepresného „tree“ framingu v prospech:

  * **rooted graph with branch lineage constraints**

Ďalší prirodzený krok po tomto je podľa mňa:

* prepracovať **ADR-024** na rovnakú úroveň tvrdosti,
  lebo 023 a 024 spolu priamo súvisia a po spevnení 023 bude 024 pôsobiť ešte mäkšie.
