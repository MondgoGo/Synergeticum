# 🧾 ADR-042: Unified Entity Model (Node)

---

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

Systém Synergetikum pracuje s rôznymi typmi objektov:

* otázky (Questions)
* projekty (Projects)
* argumenty (Arguments)
* témy (Topics)
* udalosti (Events)
* osoby (Persons)
* vetvy (Branches)
* diskusné vlákna (Threads)

V doterajších ADR sú tieto entity definované oddelene, v rôznych kontextoch:

* `ADR-009` definuje otázky
* `ADR-022` a `ADR-031` definujú projekt a tému
* `ADR-025` definuje argumenty
* `ADR-027` definuje projektový view

---

### Problém

Chýba **jednotný, univerzálny model entity**, ktorý by:

* zjednotil všetky typy objektov pod spoločnú štruktúru
* umožnil fraktálne správanie (entity môžu obsahovať iné entity)
* poskytol konzistentný spôsob pre vzťahy medzi entitami
* zjednodušil implementáciu storage, synchronizácie a práv

Bez jednotného modelu:

* každý typ entity má vlastnú štruktúru
* vzťahy sú definované ad-hoc
* zvyšuje sa komplexita kódu
* ťažšie sa implementuje univerzálne filtrovanie a view vrstva

---

## 3. ⚖️ Rozhodnutie

Systém zavedie **univerzálny model entity nazývaný Node**.

---

### 3.1 Definícia Node

Node je základná stavebná jednotka systému. Všetko je Node:

* otázka je Node
* projekt je Node
* argument je Node
* téma je Node
* udalosť je Node
* osoba (reprezentovaná v systéme) je Node
* vetva je Node
* diskusné vlákno je Node

```json
{
  "node_id": "string (UUID alebo multihash)",
  "node_type": "enum (Question | Project | Argument | Topic | Event | Person | Branch | Thread)",
  "version": "string (semver alebo timestamp+hash)",
  "created_at": "ISO timestamp",
  "updated_at": "ISO timestamp",
  "created_by": "string (node_id používateľa)",
  
  "name": "string",
  "description": "string (optional)",
  "content": "object (typovo špecifický obsah)",
  
  "attributes": {
    "status": "string (active | archived | dormant | proposed)",
    "visibility": "enum (private | circle | public)",
    "custom": {} // voliteľné rozšírenia
  },
  
  "relations": [
    {
      "target_id": "string",
      "relation_type": "enum (parent_of | child_of | classified_as | supported_by | opposes | forked_from | merged_into | references | mentioned_in)",
      "strength": "float (0-1, optional)",
      "metadata": {} // optional
    }
  ],
  
  "rights": {
    "owner": "string (node_id)",
    "edit": ["string (node_id alebo role)"],
    "view": ["string (node_id alebo role)"],
    "comment": ["string (node_id alebo role)"]
  },
  
  "statistics": {
    "participants_count": "int",
    "arguments_count": "int",
    "last_activity": "ISO timestamp",
    "impact_score": "float (0-1, optional)"
  }
}
```

---

### 3.2 Typy Node

| `node_type` | Popis | Špecifický content |
|---|---|---|
| **Question** | Otázka, ktorá spúšťa interakciu | `{ "perspectives": [], "expected_answer_type": "string" }` |
| **Project** | Kolektívny zámer s vetvami | `{ "branch_root_id": "string", "lifecycle_stage": "string" }` |
| **Argument** | Tvrdenie podporujúce alebo proti | `{ "stance": "support | oppose | neutral", "confidence": "float", "evidence": [] }` |
| **Topic** | Téma na organizáciu obsahu | `{ "parent_topic_id": "string", "taxonomy_path": "string[]" }` |
| **Event** | Časovo ohraničená aktivita | `{ "start_time": "ISO", "end_time": "ISO", "location": "string", "recurrence": "string" }` |
| **Person** | Reprezentácia používateľa v systéme | `{ "display_name": "string", "public_key": "string" }` |
| **Branch** | Vetva projektu | `{ "parent_branch_id": "string", "project_id": "string", "merge_status": "string" }` |
| **Thread** | Diskusné vlákno | `{ "posts": [], "closed": "boolean" }` |

---

### 3.3 Vzťahy medzi Node-mi

Vzťahy sú **orientované** a definujú grafovú štruktúru.

| `relation_type` | Význam | Príklad |
|---|---|---|
| `parent_of` | A obsahuje B | Projekt obsahuje argument |
| `child_of` | B je súčasťou A | Argument patrí do projektu |
| `classified_as` | A je kategorizovaný ako B | Projekt je téma "jedlo" |
| `supported_by` | A je podporený B | Argument podporuje projekt |
| `opposes` | A je v rozpore s B | Argument je proti vetve |
| `forked_from` | A je kópia B | Fork projektu |
| `merged_into` | A bol zlúčený do B | Vetva bola zlúčená |
| `references` | A odkazuje na B | Otázka odkazuje na tému |
| `mentioned_in` | A je spomenutý v B | Osoba je spomenutá v argumente |

---

### 3.4 Fraktálna vlastnosť

Node je **fraktálny** – každý Node môže obsahovať iné Node-y.

* Projekt obsahuje argumenty a vetvy
* Téma obsahuje podtémy a otázky
* Diskusné vlákno obsahuje príspevky

To neznamená fyzické vnorenie (JSON v JSON), ale **logické vzťahy** cez `relations`.

Fraktalita umožňuje:

* rekurzívnu navigáciu
* univerzálne zobrazenie stromu
* jednotné spracovanie naprieč typmi

---

### 3.5 Verzionovanie

Každý Node má:

* `version` – identifikátor verzie
* `created_at` / `updated_at` – časové značky

Zmeny v Node:

* sa zaznamenávajú do audit logu (ADR-030)
* vytvárajú novú verziu (immutable history)
* staršie verzie zostávajú dostupné pre forkovanie

---

### 3.6 Práva a viditeľnosť

`rights` definuje, kto môže čo robiť:

* `owner` – vlastník (zvyčajne tvorca)
* `edit` – zoznam používateľov alebo rolí, ktoré môžu meniť
* `view` – zoznam používateľov alebo rolí, ktoré môžu vidieť
* `comment` – zoznam používateľov alebo rolí, ktoré môžu komentovať

Role (zjednodušené oproti pôvodnému systému):

* `owner` – zakladateľ (plné práva)
* `participant` – aktívny člen (môže editovať, komentovať)
* `observer` – len sleduje (len view)

`visibility`:

* `private` – len owner
* `circle` – členovia trusted circle (ADR-004)
* `public` – ktokoľvek

---

## 4. 🧠 Dôsledky rozhodnutia

---

### ✅ Pozitívne

#### 🔧 Jednotná implementácia

* storage, sync, caching – jeden model
* zníženie komplexity

---

#### 🔗 Konzistentné vzťahy

* všetky entity používajú rovnaký relačný model
* grafová štruktúra je prirodzená

---

#### 🧩 Fraktalita

* rovnaký kód pracuje s projektom, argumentom aj témou
* rekurzívne zobrazenie je triviálne

---

#### 📊 Jednotné filtrovanie

* View Layer (ADR-027) môže byť univerzálny
* filter podľa `node_type` a `attributes`

---

### ❗ Negatíva / Trade-offs

#### 📦 Väčší objekt

* Node má polia pre všetky typy
* niektoré polia sú pre daný typ prázdne

---

#### 🔍 Typová bezpečnosť

* v kóde treba kontrolovať `node_type` pred prístupom k `content`
* možnosť runtime chýb

---

#### 🐢 Výkon pri hlbokých grafoch

* rekurzívne načítanie môže byť pomalé
* treba lazy loading a caching

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Samostatné modely pre každý typ entity

* jednoduchšie na pochopenie
* ale: duplicita kódu, ťažšie vzťahy, nekonzistentné práva

→ **Zamietnuté** pre vysokú komplexitu pri vzťahoch.

---

### ❌ Document-based s dedičnosťou

* každý typ dedí od základného
* ale: serializácia/deserializácia je komplikovaná

→ **Zamietnuté** pre implementačnú náročnosť v C# / .NET MAUI.

---

## 6. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|---|---|
| ADR-001 (Ownership) | `rights.owner` |
| ADR-004 (Privacy) | `visibility.circle` |
| ADR-009 (Questions) | `node_type = Question` |
| ADR-022 (Project Formation) | `node_type = Project` |
| ADR-025 (Argumentation) | `node_type = Argument`, `relations.supported_by` |
| ADR-027 (Project View) | filtrovanie a zobrazenie Node-ov |
| ADR-030 (Sync) | verzie a audit log |
| ADR-031 (Topic Layer) | `node_type = Topic` |
| ADR-034 (Personal AI) | Interest Model mapovaný na `Topic` Node |
| ADR-037 (Identity) | `node_type = Person` |

---

## 7. 📏 Pravidlá implementácie (high-level)

1. **Storage**
   * Node-y sú uložené v lokálnej databáze (SQLite alebo LMDB)
   * Indexy podľa `node_id`, `node_type`, `relations.target_id`

2. **Sync**
   * Synchronizujú sa celé Node-y (alebo iba zmeny)
   * Verzie sa riešia cez ADR-030

3. **API**
   * `GetNode(node_id)` – načítanie
   * `QueryNodes(filter)` – filtrovanie
   * `CreateNode(node)` – vytvorenie
   * `UpdateNode(node_id, delta)` – aktualizácia
   * `GetRelations(node_id, relation_type)` – vzťahy

4. **Validácia**
   * Povinné polia: `node_id`, `node_type`, `name`, `created_by`
   * `node_type` musí byť z enumu
   * `relations.target_id` musí existovať

---

## 8. 🔥 Príklady použitia

### Projekt ako Node

```json
{
  "node_id": "proj_pizzeria_001",
  "node_type": "Project",
  "name": "Komunitná pizzéria v Trnave",
  "content": {
    "branch_root_id": "branch_main_001",
    "lifecycle_stage": "active"
  },
  "relations": [
    { "target_id": "topic_food_001", "relation_type": "classified_as" },
    { "target_id": "user_miro_001", "relation_type": "parent_of" }
  ],
  "rights": {
    "owner": "user_miro_001",
    "edit": ["participant"],
    "view": ["circle"]
  }
}
```

### Argument ako Node

```json
{
  "node_id": "arg_001",
  "node_type": "Argument",
  "name": "Pizzéria potrebuje minimálne 3 ľudí na rozbeh",
  "content": {
    "stance": "support",
    "confidence": 0.85,
    "evidence": ["skúsenosti z podobných projektov"]
  },
  "relations": [
    { "target_id": "proj_pizzeria_001", "relation_type": "child_of" },
    { "target_id": "proj_pizzeria_branch_finance", "relation_type": "supports" }
  ]
}
```

### Otázka ako Node

```json
{
  "node_id": "q_001",
  "node_type": "Question",
  "name": "Ako zabezpečiť financovanie pizzerie?",
  "content": {
    "perspectives": ["financial", "community", "grant"],
    "expected_answer_type": "text"
  },
  "relations": [
    { "target_id": "topic_food_001", "relation_type": "classified_as" }
  ]
}
```

---

## 9. 🧭 Zhrnutie

> **Všetko je Node. Node je jednotný model pre akýkoľvek objekt v systéme.**

Toto ADR definuje:

* univerzálnu štruktúru `Node`
* typy Node (`Question`, `Project`, `Argument`, `Topic`, `Event`, `Person`, `Branch`, `Thread`)
* vzťahy medzi Node-mi (orientovaný graf)
* fraktálnu vlastnosť (Node obsahuje Node-y)
* základné práva a viditeľnosť

---

## 10. 📝 Finálna veta

> **Bez jednotného modelu entity nie je možné konzistentne spájať otázky, projekty, argumenty a témy. ADR-042 poskytuje tento základ.**

---