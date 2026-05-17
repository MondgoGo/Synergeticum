Áno. Toto ADR treba pritvrdiť a premeniť z vízie na **architektonický kontrakt**.
Tvoja diagnostika je presná: najväčší problém je, že dnes nie je dosť jasné:

* čo je input,
* čo je output,
* čo je presný formation flow,
* a čo je ešte len prezentačný jazyk.

Nižšie je **prepracovaná verzia ADR-022**, už zladená s:

* ADR-075 (state vs process) 
* ADR-076 (typed projections + command layer) 
* ADR-077 (MVPj) 
* ADR-078 (Topic → Project transition) 
* a opravuje slabiny pôvodného ADR-022 

---

````md
# 🧾 ADR-022 — Collective Project Formation

## 1. 📌 Status

**Proposed (v2)**

---

## 2. 🎯 Kontext

Synergetikum umožňuje:

- vznik otázok a Topicov (ADR-009, ADR-031)
- kolektívne odpovede a signály záujmu (ADR-016)
- identifikáciu príležitostí (ADR-019)
- vznik projektov z topicov (ADR-078)

Doteraz systém pokrýva najmä:

- myslenie
- diskusiu
- perspektívy
- signály záujmu

V reálnej implementácii však vzniká zásadný problém:

> **Ako sa z kolektívnej diskusie stane konkrétny projektový objekt, ktorý už má minimálnu štruktúru, zodpovednosť a smer?**

Bez jasného formation kontraktu hrozí:

| Riziko | Dôsledok |
|--------|----------|
| Pseudo-projekty | Topic s minimálnou aktivitou vytvorí „projekt“ |
| Nekonzistentný vznik | Raz vzniká projekt automaticky, raz ručne, raz vôbec |
| Nejasný storage model | Architekt nevie, aké entity sa reálne vytvárajú |
| Strata kontextu | Projekt nevie, z akej diskusie vznikol |
| Chaos vo vrstvách | Mieša sa doménový objekt, workflow a view |

---

## 3. ⚖️ Rozhodnutie

Systém zavádza **Collective Project Formation** ako **process layer**, nie ako samostatný „dokumentový typ“.

> **Project Formation nie je entita. Je to proces, ktorý transformuje Topic na Project v stave `forming`, ak sú splnené definované podmienky.**

To znamená:

- nevzniká „Living Project Document“ ako nová core doménová entita
- vzniká **Project Node / Project Domain Object**
- vzniká **Initial Branch**
- zachováva sa väzba na **Origin Topic**

---

## 4. 🧠 Čo Project Formation JE a čo NIE JE

### 4.1 Project Formation JE

Project Formation je:

- procesná vrstva
- sada pravidiel a commandov
- kontrolovaný prechod z Topicu do Projectu
- miesto, kde sa extrahuje minimálna počiatočná štruktúra projektu

### 4.2 Project Formation NIE JE

Project Formation nie je:

- nový typ Node
- nový dokumentový formát
- view layer
- prezentačný profil

---

## 5. 📥 Formation Input Contract

Project Formation pracuje s nasledujúcimi vstupmi:

### 5.1 Primárny vstup: Topic

Topic musí byť existujúci a validný podľa ADR-031 a ADR-078.

### 5.2 Vstupné signály z Topicu

Formation process môže pracovať s:

| Vstup | Popis |
|-------|-------|
| `interest_level` | agregovaný záujem o tému |
| `participant_count` | počet relevantných participantov |
| `commitment_signals` | explicitné alebo implicitné signály záväzku |
| `directions` | identifikované smery riešenia |
| `problem candidates` | formulácie problému |
| `resource hints` | kto vie prispieť, aké capability sú dostupné |
| `risk hints` | identifikované riziká a námietky |

### 5.3 Human confirmation

Formation nikdy nekončí bez explicitného ľudského potvrdenia, ak sa nejedná o ručné založenie projektu.

---

## 6. 📤 Formation Output Contract

Ak formation uspeje, systém vytvorí presne tieto doménové objekty:

### 6.1 Project

Nový `Project` v stave:

```text
lifecycle_stage = forming
````

Projekt musí byť aspoň **existenčne validný** podľa ADR-077.

### 6.2 Initial Branch

Systém vytvorí počiatočnú vetvu projektu:

* `initial_branch`
* parent = null
* project_id = nový projekt

Táto vetva predstavuje prvý realizovateľný smer projektu.

### 6.3 Linkage

Musia vzniknúť väzby:

* `project.origin_topic_id = topic.id`
* `topic.project_reference = project.id`

### 6.4 Initial participant set

Do projektu sa prenášajú iba tí participanti, ktorí:

* potvrdili záujem,
* alebo sú explicitne súčasťou formation procesu.

---

## 7. 🔄 Formation Lifecycle (procesné fázy)

### Fáza 1 — Topic maturity detection

Systém alebo participanti identifikujú, že Topic dozrel na kandidáta projektu.

Toto je riešené cez ADR-078.

Výstup:

* Topic je `candidate_for_project`

### Fáza 2 — Formation preparation

Systém pripraví návrh projektového minima z Topicu:

* pracovný názov projektu
* problem statement
* možné smery
* potenciálnych participantov
* základné riziká

### Fáza 3 — Human confirmation

Používatelia potvrdia, že chcú Topic transformovať na Project.

Bez potvrdenia sa Project nevytvorí.

### Fáza 4 — Project creation

Systém vytvorí:

* nový Project (`forming`)
* initial branch
* linkage na Topic

### Fáza 5 — Post-formation stabilization

Po vzniku projektu:

* Topic ostáva historickým a diskusným kontextom
* Project sa ďalej vyvíja už vo vlastnej vrstve
* ďalšie zmeny v Topicu už priamo neprepísujú projekt

---

## 8. 🧠 Minimálny obsah vytvorený pri formation

Formation process sa snaží vytvoriť minimálne:

| Pole                | Zdroj                                       |
| ------------------- | ------------------------------------------- |
| `name`              | Topic name alebo extrahovaný pracovný názov |
| `problem_statement` | extrahované z diskusie                      |
| `origin_topic_id`   | Topic                                       |
| `owner_id`          | iniciátor alebo potvrdený zakladateľ        |
| `participant_ids`   | potvrdení účastníci                         |
| `initial_branch_id` | novo vytvorená vetva                        |
| `goal_statement`    | ak sa dá spoľahlivo extrahovať              |
| `success_criteria`  | voliteľné, ak existujú                      |

Ak sa tieto dáta nepodarí pripraviť aspoň na existenčné minimum, formation musí zlyhať alebo ostať v prípravnom stave.

---

## 9. 🚫 „Living Project Document“ ako vysvetľujúci pojem

Pojem **Living Project Document** môže zostať ako:

* vysvetľujúca metafora,
* user-facing jazyk,
* opis dynamickej povahy projektu.

Ale:

> **nie je to core doménová entita.**

Core doménové entity sú:

* `Project`
* `Branch`
* `Topic`
* `Argument`
* `Person`

To je dôležité, aby architekt nemodeloval „dokument“, ale stabilný domain objekt.

---

## 10. 🖼️ Insight / Proposal / Project ako presentation profiles

Pôvodné typy:

* Insight Document
* Proposal Document
* Project Document

sa v tejto ADR nepovažujú za core doménové typy.

Namiesto toho sa berú ako:

> **presentation / workflow profiles nad rovnakým doménovým základom**

Teda:

| Profil     | Význam                             |
| ---------- | ---------------------------------- |
| `Insight`  | sumarizačný pohľad na diskusiu     |
| `Proposal` | návrhový pohľad na možný smer      |
| `Project`  | akčný pohľad na existujúci projekt |

Tieto profily patria skôr do:

* view layer,
* workflow layer,
* alebo UI.

Nie do core domain modelu.

---

## 11. 🤝 Úloha človeka

Project Formation nesmie byť plne autonómny.

Systém môže:

* detegovať zámery,
* navrhovať projektové minimum,
* extrahovať smery,
* sumarizovať problém.

Ale:

> **človek má finálne slovo pri vzniku projektu.**

To je dôležité kvôli:

* dôvere,
* zodpovednosti,
* non-manipulation,
* human-in-the-loop.

---

## 12. 🔐 Ochrana súkromia

Formation process musí rešpektovať privacy-first princípy:

* nevyžaduje surové osobné dáta
* pracuje so signálmi, nie s plnými profilmi
* participanti sa prenášajú len, ak to explicitne potvrdili
* Topic môže byť zdrojom agregovaných poznatkov, nie raw exportov

---

## 13. 🧠 Dôsledky rozhodnutia

### ✅ Pozitíva

| Dôsledok                        | Popis                                      |
| ------------------------------- | ------------------------------------------ |
| Jasný formation contract        | Architekt vie, čo vzniká a kedy            |
| Menší chaos vo vrstvách         | entita ≠ proces ≠ view                     |
| Žiadny pseudo-dokumentový model | core doména ostáva čistá                   |
| Lepšia implementovateľnosť      | formation je command/process, nie metafora |
| Priame prepojenie na MVPj       | vznikajú len existenčne validné projekty   |

### ❗ Negatíva / Trade-offs

| Dôsledok                      | Mitigácia                           |
| ----------------------------- | ----------------------------------- |
| Vyššia disciplína pri návrhu  | ale nižší chaos neskôr              |
| Menej poetický model          | ale výrazne lepší pre implementáciu |
| Potreba ďalších ADR na detail | už riešené v 075 / 077 / 078        |

---

## 14. 🚫 Alternatívy (zamietnuté)

### ❌ Living Project Document ako core entita

Zamietnuté, lebo:

* je to metafora, nie presný domain model
* môže viesť k nejednotnej implementácii

### ❌ Manuálne zakladanie projektov bez väzby na Topic

Zamietnuté ako default, lebo:

* stráca sa kolektívny kontext
* vzniká chaos a duplicita

### ❌ Plne automatický vznik projektu

Zamietnuté, lebo:

* porušuje human-in-the-loop
* znižuje dôveru
* vedie k predčasným projektom

---

## 15. 🔗 Väzby na ďalšie ADR

| ADR     | Vzťah                                                 |
| ------- | ----------------------------------------------------- |
| ADR-016 | query a odpovede generujú vstupy                      |
| ADR-019 | opportunity signals pomáhajú identifikovať kandidatov |
| ADR-023 | výsledný projekt je distribuovaný objekt              |
| ADR-024 | formation vytvára initial branch                      |
| ADR-031 | Topic je vstupná entita                               |
| ADR-075 | formation je process, nie state                       |
| ADR-076 | výstup sa mapuje do doménových projekcií              |
| ADR-077 | project po formation musí byť existenčne validný      |
| ADR-078 | tento ADR určuje, kedy Topic dozrel na Project        |

---

## 16. 📏 Pravidlá implementácie (high-level)

Systém musí:

1. pracovať s Topicom ako primárnym inputom
2. extrahovať projektové minimum zo signálov a diskusie
3. vytvárať výstup ako:

   * `Project`
   * `Initial Branch`
   * `Topic ↔ Project linkage`
4. nebrať „Living Project Document“ ako core storage entitu
5. presunúť Insight / Proposal / Project document typy do view/workflow vrstvy
6. vyžadovať human confirmation pred final creation
7. nevytvárať projekt, ktorý nespĺňa existenčné minimum podľa ADR-077

---

## 17. 🔥 Zhrnutie

> **Collective Project Formation nie je dokument. Je to proces, ktorý transformuje dozretý Topic na existenčne validný Project v stave `forming`, vytvorí jeho počiatočnú vetvu a zachová väzbu na pôvodnú diskusiu.**

---

## 18. 🧭 Finálna veta

> **Otázky generujú myslenie. Topicy organizujú diskusiu. Project Formation je okamih, keď sa z kolektívneho záujmu stane prvý skutočný projektový objekt.**

```

---

Toto je už verzia, ktorú by som architektovi dal.

Ešte by som spravil jeden malý follow-up v ďalšom kole:
- zosúladiť slovník medzi ADR-022, ADR-077 a ADR-078 tak, aby sa všade rovnako používali pojmy:
  - `candidate_for_project`
  - `forming`
  - `active`
  - `origin_topic_id`
  - `initial_branch`

To je malá vec, ale výrazne znižuje budúci chaos.

Pošli ďalší a upravím ho rovnako.
```
