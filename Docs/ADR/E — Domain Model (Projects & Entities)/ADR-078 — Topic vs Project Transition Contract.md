# 🧾 ADR-078 — Topic vs Project Transition Contract

## 1. 📌 Status

**Accepted (revízia 3.0)**

---

## 2. 🎯 Kontext

Synergetikum má dve samostatné vrstvy:

| Vrstva | ADR | Účel |
|--------|-----|------|
| **Topic Layer** | ADR-031 | Kolektívna diskusia, explorácia, bez záväzku |
| **Project Layer** | ADR-022, ADR-023, ADR-024, ADR-077 | Kolektívna akcia, záväzok, realizácia |

V reálnej implementácii vzniká problém:

> **Kedy presne sa z Topicu stane Project? A čo sa pri tom prenáša a čo nie?**

Bez jasného kontraktu hrozia:

- **Predčasné projekty** – Topic s minimálnou aktivitou sa stane projektom
- **Uviaznuté topicy** – diskusia nikdy neprejde do akcie
- **Strata kontextu** – projekt nevie, z akej diskusie vznikol
- **Duplicita** – rovnaká diskusia vedie k viacerým projektom
- **Pilotná komplexita** – prechod je tak inteligentný, že sa nedá spoľahlivo implementovať

---

## 3. ⚖️ Rozhodnutie

Systém zavádza **explicitný transition contract** medzi Topicom a Projectom.

> **Topic → Project je jednosmerný, explicitný, merateľný proces dozrievania, ktorý vytvára nový Project v stave `forming`, vytvára `initial_branch` a zachováva Topic ako historický a diskusný kontext.**

Pre pilot platí:

> **Prechod sa riadi jednoduchým `Transition Policy Profile`, nie plne dynamickou AI logikou.**

---

## 4. 🧠 Definícia Topic vs Project (ostré hranice)

| Vlastnosť | **Topic** | **Project** |
|-----------|-----------|--------------|
| **Primárny účel** | Explorácia, diskusia, zber perspektív | Akcia, realizácia, rozhodovanie |
| **Záväzok** | Žiadny alebo slabý | Explicitný |
| **Výstupy** | Odpovede, názory, perspektívy | Vetvy, argumenty, úlohy, rozhodnutia |
| **Lifecycle** | Krátky až stredný | Stredný až dlhý |
| **Stabilita** | Nízka | Vyššia |
| **Expectation** | „Budeme sa rozprávať“ | „Budeme niečo robiť“ |

---

## 5. 🧩 Transition Policy Profiles (pilot verzia)

Namiesto plne dynamických thresholdov zavádza systém pre pilot **preddefinované policy profily**.

### 5.1 Cieľ

Policy Profile umožňuje:
- jednoduchšiu implementáciu
- auditovateľnosť
- testovateľnosť
- neskoršiu výmenu za pokročilejší model

### 5.2 Pilotné profily

| Profil | Použitie | Default thresholdy |
|--------|----------|--------------------|
| **small-community** | malé trusted circle, pilot, nízka hustota siete | interest ≥ 0.50, commitment ≥ 2, participants ≥ 2 |
| **default** | bežný pilotný režim | interest ≥ 0.60, commitment ≥ 3, participants ≥ 2 |
| **high-noise** | prostredie s vysokým šumom a množstvom slabých topicov | interest ≥ 0.70, commitment ≥ 4, participants ≥ 3 |

### 5.3 Dôležité pravidlo

> Pre pilot sa **nepoužíva per-topic AI thresholding ako source of truth**.

Personal AI môže:
- lokálne signalizovať kandidáta,
- pomáhať používateľovi pochopiť tému,

ale **transition sa nespúšťa na základe AI konsenzu**.

---

## 6. 📏 Trigger conditions pre pilot

Projekt môže vzniknúť z Topicu iba ak:

### 6.1 Topic je v stave `candidate_for_project`

Tento stav vzniká, keď Topic splní thresholdy z vybraného policy profilu.

### 6.2 Stabilita v čase

Nestačí jednorazový spike. Topic musí spĺňať thresholdy po definované okno:

- napr. **7 dní rolling window**

### 6.3 Potvrdenie ľuďmi

Transition sa dokončí iba ak:
- minimálne 2 participanti potvrdia záujem,
- alebo je splnené pravidlo definované profilom.

### 6.4 Projekt musí byť vytvoriteľný ako existenčne validný

Výsledný Project musí spĺňať minimálne existenčné podmienky podľa ADR-077.

---

## 7. 🔄 Local AI suggestion vs system metrics

### 7.1 Local suggestion

Personal AI môže používateľovi povedať:

> „Táto téma vyzerá ako dobrý kandidát na projekt.“

To je:
- lokálny signál,
- advisory vrstva,
- nie systémový trigger.

### 7.2 System metrics

Pre pilot je prechod založený na:

- `interest_level`
- `commitment_count`
- `participant_count`
- stabilite v čase
- explicitnom ľudskom potvrdení

### 7.3 Rozhodnutie pre pilot

> **System metrics + human confirmation sú jadro transitionu. AI suggestion je pomocná vrstva, nie rozhodovacia vrstva.**

---

## 8. 🔄 Transition Process (pilot verzia)

### Krok 1 — Candidate detection

Systém periodicky alebo event-driven vyhodnocuje Topic podľa vybraného `Transition Policy Profile`.

Ak Topic spĺňa profil po požadované časové okno:

```text
topic.lifecycle.stage = candidate_for_project
````

### Krok 2 — Notification

Systém oznámi relevantným participantom:

```text
📢 Téma "{{topic.name}}" dozrela na kandidáta projektu.

Chceš sa zapojiť do projektu?
[✓] Áno, chcem byť participant
[ ] Nie, zatiaľ len diskutujem
```

### Krok 3 — Human confirmation

Transition pokračuje len ak sa nazbiera požadovaný počet potvrdení.

### Krok 4 — Project creation

Systém vytvorí:

* nový `Project`
* `lifecycle_stage = forming`
* `origin_topic_id = topic.id`
* `initial_branch`
* participant set z potvrdených participantov

### Krok 5 — Cooling-off period

Po vytvorení projektu nasleduje:

* **48 hodín odvolacej lehoty**

Počas nej môže:

* owner,
* alebo participant, ktorý potvrdil záujem,

odvolať transition.

### Krok 6 — Finalization

Ak nikto transition neodvolá:

* Project zostáva v stave `forming`
* Topic prejde do `converted_to_project`
* Topic ostáva aktívny ako diskusný kontext

---

## 9. ❄️ Cooling-off a osud aborted projectu

Toto musí byť explicitné.

### 9.1 Ak je transition odvolaný počas cooling-off:

* Topic sa vráti do stavu `active`
* vytvorený Project sa **nemaže fyzicky**
* Project prejde do stavu:

```text
aborted
```

alebo technicky:

* `forming` + `aborted_at`
* podľa toho, ako presne modeluješ lifecycle

### 9.2 Pre pilot odporúčanie

Najpraktickejšie je:

> **Project Node ostáva uložený ako auditovateľný „aborted formation attempt“, ale nesurfacuje sa ako živý projekt.**

To znamená:

* zachová sa audit trail
* nevzniká neviditeľné mazanie
* dá sa analyzovať, prečo transition padol

### 9.3 Topic po aborted transition

* vracia sa do `active`
* môže sa neskôr znova stať `candidate_for_project`

---

## 10. 📦 Čo sa prenáša (Topic → Project)

| Položka                | Prenáša sa? | Ako                                            |
| ---------------------- | ----------- | ---------------------------------------------- |
| `name`                 | ✅           | pracovný názov alebo názov odvodený z Topicu   |
| `problem_statement`    | ✅           | extrahovaný z diskusie                         |
| `origin_topic_id`      | ✅           | povinný odkaz                                  |
| potvrdení participanti | ✅           | stanú sa `participant_ids`                     |
| `directions`           | ⚠️          | použijú sa na vytvorenie `initial_branch`      |
| odpovede               | ❌           | ostávajú v Topicu                              |
| raw diskusia           | ❌           | ostáva v Topicu ako kontext                    |
| agregované signály     | ⚠️          | môžu byť prepočítané do inicializačných metrík |

---

## 11. 🌱 Initial Branch

Každý Project vytvorený transitionom musí mať:

```text
initial_branch
```

Táto vetva obsahuje minimálne:

* počiatočný smer projektu,
* extrahovaný problém,
* hlavné considerations / directions.

To je dôležité pre zladenie s:

* ADR-022
* ADR-024
* ADR-077

---

## 12. 🔄 Život Topicu po transition

Topic po transition:

* **neumiera**
* **nie je read-only**
* **už ale priamo neprepísuje projekt**

To znamená:

| Situácia              | Správanie                                       |
| --------------------- | ----------------------------------------------- |
| nová odpoveď v Topicu | uloží sa do Topicu                              |
| ďalší nový smer       | môže vytvoriť nový Topic                        |
| projekt existuje      | Topic zostáva historickým a diskusným kontextom |

Po úspešnom transitione:

* `topic.project_reference = project.id`
* `project.origin_topic_id = topic.id`

---

## 13. 🧠 Manuálny override

Používateľ môže skúsiť vytvoriť Project z Topicu aj bez splnenia thresholdov.

Ale:

* systém musí zobraziť varovanie
* výsledný Project sa stále musí dať vytvoriť aspoň ako existenčne validný `forming`

To znamená:

* manuálny override neobchádza ADR-077
* len obchádza automatickú eligibility detekciu

---

## 14. 🚫 Čo sa NESMIE stať

| Zakázaná situácia                        | Prečo                            |
| ---------------------------------------- | -------------------------------- |
| Topic → Project bez `origin_topic_id`    | strata pôvodu                    |
| Project bez `initial_branch`             | chýba minimálny smer             |
| transition bez human confirmation        | strata kontroly                  |
| Topic read-only po transition            | diskusia často pokračuje         |
| aborted transition bez audit trailu      | strata vysvetliteľnosti          |
| AI consensus ako jediný trigger v pilote | prílišná komplexita príliš skoro |

---

## 15. 📊 Príklad pre pilot

### Before

```json
{
  "node_id": "topic_community_garden_001",
  "node_type": "Topic",
  "name": "Komunitná záhrada v Petržalke",
  "lifecycle": {
    "stage": "candidate_for_project"
  },
  "aggregates": {
    "interest_level": 0.78,
    "participant_count": 5,
    "response_count": 23
  },
  "signals": {
    "commitment_count": 4,
    "directions": [
      "hľadáme pozemok",
      "potrebujeme grant"
    ]
  },
  "transition_policy_profile": "default"
}
```

### After successful transition

```json
{
  "node_id": "proj_community_garden_001",
  "node_type": "Project",
  "name": "Komunitná záhrada v Petržalke",
  "content": {
    "origin_topic_id": "topic_community_garden_001",
    "lifecycle_stage": "forming",
    "created_at": "2026-04-16T10:00:00Z"
  },
  "participant_ids": ["user_miro_001", "user_jana_001"],
  "initial_branch_id": "branch_main_001"
}
```

### Topic po successful transition

```json
{
  "node_id": "topic_community_garden_001",
  "node_type": "Topic",
  "lifecycle": {
    "stage": "converted_to_project"
  },
  "project_reference": "proj_community_garden_001",
  "is_active": true
}
```

### Project po aborted transition

```json
{
  "node_id": "proj_community_garden_001",
  "node_type": "Project",
  "content": {
    "origin_topic_id": "topic_community_garden_001",
    "lifecycle_stage": "forming",
    "aborted_at": "2026-04-17T09:00:00Z",
    "aborted_reason": "cooling_off_withdrawal"
  },
  "visibility": "internal_audit_only"
}
```

---

## 16. 🧠 Dôsledky rozhodnutia

### ✅ Pozitíva

| Dôsledok          | Popis                             |
| ----------------- | --------------------------------- |
| jednoduchší pilot | menej inteligencie, viac kontroly |
| auditovateľnosť   | prechod je jasný a vysvetliteľný  |
| menšia komplexita | profiles sú implementovateľné     |
| zladený slovník   | rovnaké pojmy naprieč E vrstvou   |
| zachovaný kontext | Topic ostáva živý                 |

### ❗ Negatíva / Trade-offs

| Dôsledok                          | Mitigácia                  |
| --------------------------------- | -------------------------- |
| menej adaptívne než plne AI model | pre pilot je to výhoda     |
| potreba policy správy             | len 3 profily v pilotovi   |
| aborted projekty v storage        | riešené audit-only režimom |

---

## 17. 🔗 Väzby na ďalšie ADR

| ADR     | Vzťah                                                             |
| ------- | ----------------------------------------------------------------- |
| ADR-022 | formation vytvára `Project` + `initial_branch`                    |
| ADR-023 | vzniknutý Project je distribuovaný objekt                         |
| ADR-024 | initial branch je vstup do branching modelu                       |
| ADR-031 | Topic je vstupná entita                                           |
| ADR-075 | transition je proces, nie stav                                    |
| ADR-076 | Topic a Project sa mapujú do typed projections                    |
| ADR-077 | výsledný Project musí byť existenčne validný a vzniká v `forming` |
| ADR-082 | validácia transition výsledku                                     |

---

## 18. 📏 Pravidlá implementácie (pilot)

Systém musí:

1. používať `Transition Policy Profile`
2. pre pilot nepoužívať AI consensus ako source of truth
3. pracovať so system metrics + human confirmation
4. vytvoriť Project vždy v stave `forming`
5. vytvoriť `initial_branch`
6. nastaviť `origin_topic_id`
7. zachovať Topic aktívny po transition
8. pri odvolaní označiť Project ako aborted, nie ho ticho zmazať
9. používať jednotný slovník:

   * `candidate_for_project`
   * `forming`
   * `active`
   * `origin_topic_id`
   * `initial_branch`

---

## 19. 🔥 Zhrnutie

> **Topic je diskusia. Project je akcia. Pre pilot sa prechod z Topicu do Projectu neriadi plne dynamickou inteligenciou, ale jednoduchým policy profilom, systémovými metrikami a ľudským potvrdením.**

---

## 20. 🧭 Finálna veta

> **Prechod do projektu nemá byť magický. Má byť zrozumiteľný, auditovateľný a dostatočne jednoduchý na to, aby sa dal spoľahlivo postaviť v pilote — bez toho, aby si si zabetónoval budúcnosť.**

```

---

Toto je už verzia, ktorú by som pre pilot odporučil výrazne viac než tú predošlú.

Najväčšia zmena je, že sme:
- stiahli AI z centra rozhodovania,
- nahradili ju **policy profilmi + systémovými metrikami + ľudským potvrdením**,
- a jasne sme povedali, čo sa stane s aborted projektom.

Ďalší logický krok je podľa mňa:
- buď prepracovať **ADR-023**,
- alebo spraviť **pilot profile pre celú kapitolu E**.
```
