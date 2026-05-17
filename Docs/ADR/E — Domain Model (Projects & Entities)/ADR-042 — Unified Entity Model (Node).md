# 🧾 ADR-042 — Unified Entity Model (Node)

## 1. 📌 Status

**Accepted (revízia 2.0)**

---

## 2. 🎯 Kontext

Synergetikum pracuje s rôznymi typmi objektov:

- Questions
- Topics
- Projects
- Arguments
- Branches
- Events
- Persons
- a ďalšími entitami, ktoré budú postupne pribúdať

V doterajších ADR sú tieto entity definované oddelene v rôznych vrstvách systému.

Bez jednotného modelu vznikajú tieto problémy:

- každý typ entity má vlastnú ad-hoc štruktúru
- vzťahy medzi entitami sú nekonzistentné
- storage, sync a rights sa riešia zvlášť pre každý typ
- query layer a view layer musia poznať priveľa špecifických formátov
- rozširovanie systému zvyšuje komplexitu rýchlejšie než lineárne

---

## 3. ❗ Hlavný architektonický problém

Systém potrebuje **univerzálny model entity**, ale zároveň sa musí vyhnúť tomu, aby sa tento model stal:

> **„všetkým pre všetkých“**

Ak bude Node príliš široký:

- všetko skončí v `content`
- typové hranice budú slabé
- invarianty sa rozlejú mimo domény
- sync model bude stabilný, ale doména sa rozpadne

Preto musí byť Node:

> **maximálne stabilný, maximálne malý a maximálne konzervatívny**

a všetka bohatšia doménová logika musí žiť **nad ním**, nie **v ňom**.

---

## 4. ⚖️ Rozhodnutie

Systém zavedie **univerzálny model entity nazývaný Node**.

> **Node je jednotný storage/sync substrate pre všetky entity systému. Nie je to finálny doménový objekt pre aplikáciu.**

To znamená:

- storage a sync pracujú s `Node`
- query layer môže pracovať s `Node`
- typed/domain vrstva pracuje s projekciami alebo doménovými view nad `Node` (ADR-076)
- Node nesmie niesť zbytočne veľa doménovej variability

---

## 5. 🧠 Core princíp Node modelu

### 5.1 Čo je Node

Node je:

- identifikovateľná entita v systéme
- nositeľ minimálneho univerzálneho metadata modelu
- uzol grafu
- jednotka synchronizácie a verzovania

### 5.2 Čo Node NIE JE

Node nie je:

- plnohodnotný doménový objekt pre UI
- typed aggregate
- miesto, kde sa má implementovať business logika
- náhrada za command layer
- view model

---

## 6. 🧱 Core Node Schema (stabilné jadro)

Core Node má byť čo najmenší a čo najstabilnejší.

### 6.1 Povinné core polia

```json
{
  "node_id": "string (UUID, multihash alebo iný stabilný identifikátor)",
  "node_type": "enum",
  "schema_version": "string",
  "created_at": "ISO timestamp",
  "updated_at": "ISO timestamp",
  "created_by": "node_id (Person)",
  "payload": "object",
  "relations": [],
  "access": {},
  "lifecycle": {}
}
````

### 6.2 Význam core polí

| Pole                       | Účel                    |
| -------------------------- | ----------------------- |
| `node_id`                  | unikátna identita       |
| `node_type`                | určuje typ uzla         |
| `schema_version`           | verzia payload schémy   |
| `created_at`, `updated_at` | audit a sync            |
| `created_by`               | pôvodca                 |
| `payload`                  | typovo špecifické dáta  |
| `relations`                | väzby na iné Node-y     |
| `access`                   | minimálny access model  |
| `lifecycle`                | minimálny stavový rámec |

---

## 7. 📦 Zúženie Node: payload namiesto „všetko v jednom“

Pôvodný model mal veľa univerzálnych polí ako:

* `name`
* `description`
* `statistics`
* `attributes.custom`
* rôzne rights zoznamy

To je riskantné, lebo časom vznikne „univerzálny blob“.

### Rozhodnutie

> **Typovo špecifické dáta sa presúvajú do `payload`, nie do core Node.**

Teda:

* `Project` si definuje svoj payload
* `Topic` si definuje svoj payload
* `Argument` si definuje svoj payload
* atď.

Core Node zostáva malý.

### 7.1 Príklad

```json
{
  "node_id": "proj_001",
  "node_type": "Project",
  "schema_version": "2.0",
  "created_at": "2026-04-16T10:00:00Z",
  "updated_at": "2026-04-16T12:00:00Z",
  "created_by": "person_miro_001",
  "payload": {
    "name": "Komunitná záhrada v Petržalke",
    "problem_statement": "V Petržalke chýba verejný priestor na komunitné pestovanie.",
    "goal_statement": "Overiť záujem a pripraviť prvý návrh záhrady.",
    "origin_topic_id": "topic_garden_001",
    "lifecycle_stage": "forming",
    "initial_branch_id": "branch_main_001"
  },
  "relations": [
    { "target_id": "topic_garden_001", "relation_type": "originated_from" }
  ],
  "access": {
    "visibility": "circle",
    "policy_mode": "simple"
  },
  "lifecycle": {
    "status": "active"
  }
}
```

---

## 8. 🧩 Node types — stabilné jadro vs rozšírenia

Jedna z najväčších pascí Node modelu je pridávať nové `node_type` príliš ľahko.

### 8.1 Core Node Types

Tieto typy sa považujú za **stabilné core typy**:

| `node_type` | Popis                               |
| ----------- | ----------------------------------- |
| `Question`  | vstupná otázka                      |
| `Topic`     | diskusná / tematická entita         |
| `Project`   | akčný objekt                        |
| `Branch`    | vetva projektu                      |
| `Argument`  | argument v diskusii alebo projekte  |
| `Person`    | reprezentácia identity používateľa  |
| `Event`     | plánovaná alebo zaznamenaná udalosť |

### 8.2 Non-core / optional typy

Nie všetko musí byť first-class Node hneď.

#### `Thread`

`Thread` sa zatiaľ **nepovažuje za povinný core typ**.

Môže byť:

* samostatný Node typ v neskoršej fáze,
* alebo odvodený view/materialization nad:

  * Argumentmi
  * odpoveďami
  * komentármi

### Rozhodnutie pre architektúru

> **Core Node Types sa menia veľmi zriedka. Nový node_type musí byť výnimočný architektonický zásah, nie pohodlné rozšírenie.**

---

## 9. 🔗 Relations — konzervatívny a stabilný model

Relations sú jeden z najcitlivejších aspektov celého systému.

Ak sa relation typy budú meniť často, rozbije sa:

* sync
* query layer
* validation
* analytics
* view layer
* migrácie

### 9.1 Relations sú orientované

Každá relation je orientovaná:

```json
{
  "target_id": "string",
  "relation_type": "enum",
  "metadata": {}
}
```

### 9.2 Core relation types

Pre jadro systému sa zavádza konzervatívny zoznam:

| `relation_type`   | Význam                      |
| ----------------- | --------------------------- |
| `originated_from` | Project vznikol z Topicu    |
| `belongs_to`      | entita patrí pod inú entitu |
| `references`      | voľný odkaz                 |
| `supports`        | podpora                     |
| `opposes`         | opozícia                    |
| `forked_from`     | vetva vznikla z inej vetvy  |
| `merged_into`     | vetva bola zlúčená          |
| `classified_as`   | kategorizácia / taxonómia   |
| `authored_by`     | autorstvo                   |
| `participates_in` | participácia osoby          |

### 9.3 Princíp stability

> Relation typy sa majú **zmrazovať skoro** a meniť veľmi opatrne.

Ak vznikne potreba nového relation typu, treba sa najprv pýtať:

* nedá sa modelovať metadata nad existujúcim relation typom?
* je to naozaj nový semantický typ?
* nerozbije to query a validation?

---

## 10. 🧠 Fraktálna vlastnosť

Node model je fraktálny v tom zmysle, že:

* Project môže obsahovať Branch
* Branch môže obsahovať Argument
* Topic môže obsahovať ďalšie Topics alebo Questions
* Event sa môže viazať na Project, Branch alebo Topic

To však neznamená:

* JSON vnáranie do nekonečna
* fyzické embedovanie všetkých children

### Princíp

> Fraktalita je **logická**, nie nutne **fyzická**.

To znamená:

* reprezentuje sa cez `relations`
* nie cez hlboké vnorenie payloadov

---

## 11. 🔐 Access model — pilot vs budúcnosť

Pôvodný `rights` model bol funkčný, ale príliš mäkký a potenciálne ťažko synchronizovateľný. 

### 11.1 Pilotný režim — simple access

Pre pilot sa povoľuje jednoduchý access model:

```json
{
  "visibility": "private | circle | public",
  "policy_mode": "simple",
  "owner_id": "person_001"
}
```

Voliteľne možno doplniť:

* `editor_roles`
* `comment_roles`

ale len v obmedzenom rozsahu.

### 11.2 Budúci režim — ACL / inheritance

Do budúcna musí architektúra počítať s tým, že access model môže prerásť do:

* ACL policy modelu
* inheritance rules
* role-based access propagation

### Dôležité pravidlo

> Pilot môže mať zjednodušený rights model, ale Node schema musí byť pripravená na budúci ACL model bez rozbitia identity a sync vrstvy.

---

## 12. 🔄 Lifecycle v Node

Core Node má len minimálny lifecycle wrapper:

```json
"lifecycle": {
  "status": "active | dormant | archived | aborted"
}
```

Ale detailný lifecycle:

* Project lifecycle
* Topic lifecycle
* Branch lifecycle

sa modeluje v `payload` daného typu alebo v typed/domain vrstve nad Node.

### Princíp

> Core lifecycle je len univerzálny rámec.
> Typovo špecifické lifecycle stavy nepatria do univerzálneho jadra.

---

## 13. 🧠 Versioning a evolúcia schémy

Každý Node musí byť pripravený na evolúciu v čase.

### 13.1 Dve úrovne verzie

| Úroveň              | Význam                         |
| ------------------- | ------------------------------ |
| `schema_version`    | verzia payload schémy          |
| audit/sync revision | verzia konkrétnej zmeny entity |

### 13.2 Princíp

> Node model musí umožniť evolúciu bez toho, aby každá zmena payloadu rozbila storage a sync substrát.

To znamená:

* stabilné core
* flexibilný payload
* version-aware mapping (ADR-076)

---

## 14. 🚫 Čo sa NESMIE stať

| Zakázaná situácia                                       | Prečo                           |
| ------------------------------------------------------- | ------------------------------- |
| všetka doménová variabilita ide do core Node            | Node sa stane blobom            |
| nový node_type sa pridáva bez disciplíny                | rozpadne sa typová hranica      |
| relation typy sa menia ad-hoc                           | rozpadne sa sync a query vrstva |
| Node nesie business logiku                              | miešanie storage a domény       |
| access model je naveky len „zoznam userov“              | neškálovateľné                  |
| Thread je zavedený ako core len preto, že sa hodí do UI | nesprávna vrstva rozhodnutia    |

---

## 15. 🧠 Dôsledky rozhodnutia

### ✅ Pozitíva

| Dôsledok                             | Popis                                  |
| ------------------------------------ | -------------------------------------- |
| stabilnejšie jadro                   | menej refaktorov v storage/sync vrstve |
| menšie riziko generic blobu          | variability sú nad Node                |
| lepšia pripraviteľnosť na budúcnosť  | ACL, nové projections, nové payloady   |
| zrozumiteľnejší model pre architekta | core vs domain vrstva sú oddelené      |
| bezpečnejší rast komplexity          | systém môže rásť postupne              |

### ❗ Negatíva / Trade-offs

| Dôsledok                       | Mitigácia                                          |
| ------------------------------ | -------------------------------------------------- |
| viac vrstiev                   | je to zámerné                                      |
| potreba typed/domain mapovania | rieši ADR-076                                      |
| pilot nepoužije všetko         | to je v poriadku — model má byť pripravený dopredu |

---

## 16. 🚫 Alternatívy (zamietnuté)

### ❌ Samostatné storage modely pre každý typ

Zamietnuté pre:

* duplicitu
* nekonzistentné relations
* drahý sync model

### ❌ Bohatý core Node s množstvom univerzálnych polí

Zamietnuté pre:

* generic blob risk
* slabé typové hranice
* budúci chaos

### ❌ Full document-centric model

Zamietnuté pre:

* miešanie view/document pojmov s core doménou

---

## 17. 🔗 Väzby na ďalšie ADR

| ADR     | Vzťah                                           |
| ------- | ----------------------------------------------- |
| ADR-022 | Project Formation vytvára Project Node          |
| ADR-023 | Distributed Project Model je postavený nad Node |
| ADR-024 | Branching používa Branch Node a relations       |
| ADR-025 | Argumentation používa Argument Node             |
| ADR-031 | Topic Layer používa Topic Node                  |
| ADR-075 | state/process boundary žije nad Node            |
| ADR-076 | typed projections mapujú Node do domény         |
| ADR-077 | MVPj pravidlá platia nad Project payloadom      |
| ADR-078 | Topic → Project transition vytvára Node väzby   |
| ADR-082 | validácia beží nad Node + typed/domain vrstvou  |

---

## 18. 📏 Pravidlá implementácie (high-level)

Systém musí:

1. držať `Node` jadro malé a stabilné
2. ukladať doménovú variabilitu do `payload`
3. zavádzať nové `node_type` veľmi konzervatívne
4. relation typy meniť len výnimočne
5. neimplementovať business logiku priamo v Node
6. podporovať pilotný jednoduchý access model
7. byť pripravený na budúci ACL / inheritance model
8. podporovať version-aware mapping nad Node
9. rátať s tým, že nie všetky budúce entity musia byť core Node types od prvého dňa

---

## 19. 🧪 Príklady

### Topic Node

```json
{
  "node_id": "topic_food_001",
  "node_type": "Topic",
  "schema_version": "1.0",
  "created_at": "2026-04-16T10:00:00Z",
  "updated_at": "2026-04-16T11:00:00Z",
  "created_by": "person_miro_001",
  "payload": {
    "name": "Komunitná pizzéria v Trnave",
    "origin_question_id": "question_001",
    "lifecycle_stage": "candidate_for_project"
  },
  "relations": [
    { "target_id": "question_001", "relation_type": "originated_from" }
  ],
  "access": {
    "visibility": "circle",
    "policy_mode": "simple",
    "owner_id": "person_miro_001"
  },
  "lifecycle": {
    "status": "active"
  }
}
```

### Project Node

```json
{
  "node_id": "proj_pizzeria_001",
  "node_type": "Project",
  "schema_version": "1.0",
  "created_at": "2026-04-16T12:00:00Z",
  "updated_at": "2026-04-16T12:00:00Z",
  "created_by": "person_miro_001",
  "payload": {
    "name": "Komunitná pizzéria v Trnave",
    "problem_statement": "V Trnave chýba komunitný priestor s lokálnou pizzériou.",
    "origin_topic_id": "topic_food_001",
    "lifecycle_stage": "forming",
    "initial_branch_id": "branch_main_001"
  },
  "relations": [
    { "target_id": "topic_food_001", "relation_type": "originated_from" },
    { "target_id": "branch_main_001", "relation_type": "belongs_to" }
  ],
  "access": {
    "visibility": "circle",
    "policy_mode": "simple",
    "owner_id": "person_miro_001"
  },
  "lifecycle": {
    "status": "active"
  }
}
```

### Argument Node

```json
{
  "node_id": "arg_001",
  "node_type": "Argument",
  "schema_version": "1.0",
  "created_at": "2026-04-16T12:30:00Z",
  "updated_at": "2026-04-16T12:30:00Z",
  "created_by": "person_jana_001",
  "payload": {
    "title": "Pizzéria potrebuje aspoň 3 ľudí na rozbeh",
    "stance": "support",
    "confidence": 0.85,
    "evidence": ["skúsenosti z podobných projektov"]
  },
  "relations": [
    { "target_id": "proj_pizzeria_001", "relation_type": "belongs_to" },
    { "target_id": "branch_main_001", "relation_type": "supports" }
  ],
  "access": {
    "visibility": "circle",
    "policy_mode": "simple",
    "owner_id": "person_jana_001"
  },
  "lifecycle": {
    "status": "active"
  }
}
```

---

## 20. 🔥 Zhrnutie

> **Všetko dôležité v systéme môže byť reprezentované ako Node, ale Node nesmie byť miestom, kde sa stráca doménová presnosť.**

Tento ADR definuje:

* Node ako jednotný storage/sync substrate
* malé a stabilné core jadro
* payload pre typovo špecifické dáta
* konzervatívny prístup k node_type
* stabilný relation model
* jednoduchý pilotný access model s výhľadom na ACL
* pripravenosť na budúcu komplexitu bez nutnosti veľkých refaktorov

---

## 21. 🧭 Finálna veta

> **Node má byť základ, nie smetisko.**
>
> Ak bude príliš veľký, rozpadne sa doména. Ak bude dostatočne malý a stabilný, unesie rastúcu komplexitu systému bez toho, aby si každé dva mesiace prepisoval architektúru.

```

---

Toto je verzia, ktorú by som už architektovi poslal.

Najdôležitejšia zmena je, že už nie je nejasné:
- čo je core,
- čo je payload,
- čo je budúca vrstva nad Node,
- ktoré typy sú naozaj core,
- a že pilot nemusí implementovať všetko, ale architektúra na to musí byť pripravená.

Ak chceš, ďalší dobrý krok je teraz:
- skontrolovať, či je **ADR-076 plne zladené s touto novou verziou 042**, aby tam nevznikol jemný rozpor medzi `payload`, lifecycle wrapperom a typed projections.
```
