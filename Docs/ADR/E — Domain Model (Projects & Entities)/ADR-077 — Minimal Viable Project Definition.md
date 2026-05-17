# 🧾 ADR-077 — Minimal Viable Project Definition

## 1. 📌 Status

**Accepted (revízia 3.0)**

---

## 2. 🎯 Kontext

Synergetikum umožňuje:

- vznik Topicov z otázok (ADR-031)
- transformáciu Topic → Project (ADR-078)
- existenciu viacerých vetiev projektu (ADR-024)
- distribúciu projektu v P2P sieti (ADR-023)

V praxi však vzniká zásadný problém:

> **Čo je vôbec projekt? Kedy je niečo už projekt a kedy je to ešte len nápad, téma alebo hlučný signál?**

Bez jasnej definície minima projektu hrozia:

| Riziko | Dôsledok |
|--------|----------|
| **Pseudo-projekty** | Topic s minimálnou aktivitou sa stane „projektom“ |
| **Nekonečný forming stav** | Projekt nikdy neopustí `forming` |
| **Nízka dôvera** | Používatelia nebudú brať projekty vážne |
| **Filtračný chaos** | Personal AI nevie, čo má zobrazovať |
| **Dead projects** | Projekty bez záväzku alebo aktivity zaberajú miesto |
| **Falošná sociálna podpora** | Staré commitment signály predstierajú, že projekt žije |
| **Nestabilný lifecycle** | Projekt „tancuje“ medzi `active`, `dormant`, `forming` |

---

## 3. ⚖️ Rozhodnutie

Systém zavádza **Minimal Viable Project (MVPj)** ako:

> **minimálne existenčné a operačné podmienky, ktoré musí entita typu Project spĺňať, aby mala v systéme zmysel a aby mohla byť považovaná za aktívny kolektívny projekt.**

Zároveň sa explicitne rozlišujú dve úrovne validity:

### 3.1 Existenčná validita

> **Projekt môže existovať ako Project Node / Project Domain Object.**

To znamená:
- môže byť uložený,
- môže mať lifecycle,
- môže byť v stave `draft`, `forming`, `dormant` alebo `archived`,
- ale ešte nemusí byť pripravený na plné surfacovanie.

### 3.2 Operačná pripravenosť

> **Projekt je pripravený byť surfaced ako aktívny kolektívny projekt.**

To znamená:
- môže byť zobrazovaný ako relevantný projekt,
- môže byť odporúčaný,
- môže byť považovaný za `active`.

---

## 4. 🧠 Definícia projektu

> **Projekt nie je to, čo nazveme projektom. Projekt je to, čo spĺňa minimálne existenčné podmienky.**

Projekt môže byť:

- **solo projekt v stave `forming`**
- **kolektívny projekt v stave `active`**

Toto je zámerné:

- systém nesmie blokovať solo začiatky,
- ale zároveň nesmie predstierať, že solo zámer je už kolektívne aktívny projekt.

---

## 5. 📋 Domain invariants — existenčné minimum projektu

Toto sú **invarianty domény**.  
Ak nie sú splnené, projekt **nemá existovať ako validný Project objekt**.

### 5.1 Povinné atribúty

| Atribút | Typ | Popis |
|---------|-----|-------|
| `name` | string, 3–100 znakov | Zrozumiteľný názov projektu |
| `problem_statement` | string, min 20 znakov | Jasné pomenovanie problému |
| `owner_id` | node_id (Person) | Konkrétny človek zodpovedný za projekt |
| `created_at` | ISO timestamp | Čas vzniku |
| `lifecycle_stage` | enum | `draft`, `forming`, `active`, `dormant`, `archived` |
| `origin_topic_id` | node_id (Topic) alebo null | Odkiaľ projekt vznikol |

### 5.2 Minimálny obsah

Projekt musí mať **aspoň jednu** z týchto zložiek:

| Obsah | Popis |
|-------|-------|
| `initial_branch_id` | aspoň jedna počiatočná vetva |
| `goal_statement` | čo chceme dosiahnuť |
| `success_criteria` | ako vieme, že sme uspeli |

> Projekt nesmie byť prázdny kontajner.

### 5.3 Owner musí byť existujúca entita

Aby bol projekt existenčne validný:
- `owner_id` musí odkazovať na existujúci `Person` node
- owner nesmie byť revokovaný / neplatný podľa identity vrstvy

---

## 6. 👤 Solo projekt vs kolektívny projekt

### 6.1 Solo projekt (`forming`)

Projekt môže byť v stave `forming`, ak:
- je existenčne validný,
- má ownera,
- má minimálny smer,
- ale ešte nemá dostatočnú sociálnu podporu.

To znamená:

> **Jeden človek môže začať projekt.**

### 6.2 Kolektívny projekt (`active`)

Projekt môže byť v stave `active`, iba ak:
- je existenčne validný,
- a zároveň spĺňa podmienky operačnej pripravenosti.

Teda:

> **`forming` môže byť solo. `active` musí byť kolektívny.**

---

## 7. 🤝 Operačná pripravenosť projektu

Operačná pripravenosť nie je čistá pravda domény.  
Je to kombinácia:

- minimálnych invariantov,
- aktivity,
- sociálnej podpory,
- a policy thresholdov.

### 7.1 Požadované podmienky

Projekt je operačne pripravený, ak:

| Podmienka | Typ |
|-----------|-----|
| `active_participant_count >= 2` | operational |
| `active_commitment_count >= 1` | operational |
| `recent_activity == true` | operational |
| `has_direction == true` | domain + operational |
| `owner_is_operationally_valid == true` | operational |

---

## 8. 🧠 Active participant

Toto je kľúčové — nestačí len `participant_count`.

### 8.1 Definícia

> **Active participant je participant, ktorý je evidovaný ako člen projektu a zároveň má nedávnu relevantnú aktivitu alebo aktívny commitment.**

Participant sa počíta ako **active participant**, ak platí aspoň jedno z:

- má aktívny explicitný commitment
- má aktívny implicitný commitment
- vykonal relevantnú aktivitu v projektovom okne
- je owner a nie je revokovaný ani dlhodobo neaktívny

### 8.2 Relevantná aktivita

Relevantná aktivita môže byť napríklad:

- komentár k projektu
- pridanie / úprava argumentu
- návrh alebo úprava vetvy
- editácia cieľa alebo success criteria
- potvrdenie účasti
- reakcia na project prompt

### 8.3 Dôležitý princíp

> `participant_count` je len hrubý signál.  
> `active_participant_count` je signál použiteľný pre lifecycle a surfacovanie.

---

## 9. 🔄 Commitment model

ADR zavádza dva typy commitmentu:

### 9.1 Explicitný commitment

| Typ | Príklad | Váha |
|-----|---------|------|
| explicitný | klik „zapojím sa“ | 1 |

### 9.2 Behaviorálny (implicitný) commitment

| Typ | Príklad | Váha |
|-----|---------|------|
| behaviorálny | 3+ relevantné akcie v definovanom okne | 1 |

### 9.3 Pravidlo výpočtu

```text
active_commitment_count =
  explicit_commitments_within_ttl
  + implicit_commitments_within_activity_window
````

---

## 10. ⏳ Operational policy windows

Toto NIE SÚ invarianty domény.
Toto sú **operational policy thresholds**, ktoré môžu byť časom upravené bez rozbitia core domény.

### 10.1 Typické defaulty pre pilot

| Policy parameter                  | Default |
| --------------------------------- | ------- |
| `explicit_commitment_ttl_days`    | 30      |
| `implicit_commitment_window_days` | 14      |
| `recent_activity_window_days`     | 7       |
| `dormant_after_days`              | 7       |
| `archive_after_days`              | 30      |
| `reactivation_window_hours`       | 48      |

### 10.2 Dôležitý princíp

> Hodnoty typu 30 / 14 / 7 / 48 sú **policy defaulty**, nie večné pravdy domény.

To znamená:

* môžu sa meniť,
* majú byť konfigurovateľné,
* ale ich význam zostáva stabilný.

---

## 11. 👤 Owner validity vs owner liveness

Pôvodná formulácia „owner must be alive and valid“ je príliš neurčitá.

Pre pilot sa zavádza presnejší model:

### 11.1 Owner je existenčne validný, ak:

* owner node existuje
* owner nie je revokovaný / zrušený
* owner je identitne platný

### 11.2 Owner je operačne validný, ak:

* spĺňa existenčnú validitu
* a zároveň:

  * nie je dlhodobo neaktívny nad threshold
  * neodišiel explicitne z projektu
  * nie je v stave, ktorý znemožňuje niesť rolu ownera

### 11.3 Dôsledok

> Projekt môže existovať aj s problematickým ownerom,
> ale nemôže zostať dlhodobo `active`, ak owner operačne zlyhal a nebol nahradený.

---

## 12. 🔄 Lifecycle a validita

Projekt prechádza týmito stavmi:

| Stage      | Existenčná validita | Operačná pripravenosť | UI správanie         |
| ---------- | ------------------- | --------------------- | -------------------- |
| `draft`    | áno                 | nie                   | len owner            |
| `forming`  | áno                 | nie                   | obmedzene surfaced   |
| `active`   | áno                 | áno                   | plnohodnotný projekt |
| `dormant`  | áno                 | nie                   | znížená priorita     |
| `archived` | áno (historicky)    | nie                   | história             |

### Princíp

* **existenčná validita** hovorí, že projekt smie existovať
* **operačná pripravenosť** hovorí, že smie byť surfaced ako `active`

---

## 13. 📡 Revalidácia: event-driven + fallback

### 13.1 Event-driven revalidácia

Po každej relevantnej udalosti sa prehodnotí stav projektu.

Relevantné udalosti:

* nový participant
* strata participantu
* nový argument
* nová vetva
* editácia projektu
* explicitný commitment
* expiry commitmentu
* strata owner validity

### 13.2 Fallback timer

Ak projekt nemá relevantnú udalosť po policy okno nečinnosti:

* spustí sa revalidácia

### Princíp

> Nie „každých X dní bez ohľadu na realitu“, ale „po X dňoch bez relevantného pohybu“.

---

## 14. 🧠 Hysteréza medzi stavmi

Aby projekt neskákal medzi stavmi pri malých výkyvoch, zavádza sa hysteréza.

### Typické pravidlá

| Prechod                           | Podmienka                                                   |
| --------------------------------- | ----------------------------------------------------------- |
| `forming → active`                | projekt je operačne pripravený                              |
| `active → dormant`                | strata operačnej pripravenosti po policy okne               |
| `dormant → forming`               | znovu získal základnú aktivitu, ale ešte nie plnú readiness |
| `forming → active` po reaktivácii | až po znovusplnení readiness                                |
| `dormant → archived`              | dlhodobá nečinnosť                                          |

### Dôležitý princíp

> **Reaktivácia z `dormant` nejde priamo do `active`, ale späť do `forming`.**

Toto je dôležité, lebo:

* dormant projekt treba najprv znovu rozbehnúť,
* až potom môže získať plnú kolektívnu readiness.

To zabraňuje:

* nervóznemu lifecycle,
* falošným návratom,
* a UX chaosu.

---

## 15. 🚫 Čo NIE JE projekt

Systém nesmie považovať za plnohodnotný projekt:

| Entita                                       | Prečo                            |
| -------------------------------------------- | -------------------------------- |
| `Topic`                                      | je to diskusia, nie akčný objekt |
| nápad bez ownera                             | nikto nenesie zodpovednosť       |
| otázka                                       | je to vstup, nie projekt         |
| prázdny Project node                         | nemá minimálny obsah             |
| `active` projekt bez aktívnych participantov | predstiera kolektív              |
| projekt bez smeru                            | nemá vykonateľný základ          |

---

## 16. 📋 Validácia projektu

### 16.1 Existenčná validácia

```python
def is_existentially_valid(project):
    if not project.name or len(project.name) < 3:
        return False, "Chýba názov"
    if not project.problem_statement or len(project.problem_statement) < 20:
        return False, "Chýba problem_statement"
    if not project.owner_id:
        return False, "Chýba owner"
    if not project.created_at:
        return False, "Chýba created_at"
    if not project.lifecycle_stage:
        return False, "Chýba lifecycle_stage"
    if not (
        project.initial_branch_id
        or project.goal_statement
        or project.success_criteria
    ):
        return False, "Chýba minimálny obsah"
    return True, "Valid"
```

### 16.2 Operačná pripravenosť

```python
def is_operationally_ready(project, now, policy):
    if not is_existentially_valid(project)[0]:
        return False, "Nie je existenčne validný"
    if project.active_participant_count(now, policy) < 2:
        return False, "Chýbajú 2 aktívni participanti"
    if project.active_commitment_count(now, policy) < 1:
        return False, "Chýba aktívny commitment"
    if not project.has_direction():
        return False, "Chýba smer projektu"
    if not project.owner_is_operationally_valid(now, policy):
        return False, "Owner nie je operačne validný"
    return True, "Ready"
```

---

## 17. 🧪 Revalidácia projektu

```python
def revalidate_project(project, now, policy):
    if project.lifecycle_stage == "active":
        if not is_operationally_ready(project, now, policy)[0]:
            project.lifecycle_stage = "dormant"
            notify_owner("Projekt prešiel do dormant stavu")

    elif project.lifecycle_stage == "forming":
        if is_operationally_ready(project, now, policy)[0]:
            project.lifecycle_stage = "active"
            notify_participants("Projekt je teraz aktívny")

    elif project.lifecycle_stage == "dormant":
        if has_reactivation_signal(project, now, policy):
            project.lifecycle_stage = "forming"
            notify_participants("Projekt bol znovu otvorený vo forming stave")
        elif exceeds_archive_window(project, now, policy):
            project.lifecycle_stage = "archived"
            notify_owner("Projekt bol archivovaný")
```

---

## 18. 📊 Dôsledky straty podmienok

| Situácia                                    | Reakcia systému                          |
| ------------------------------------------- | ---------------------------------------- |
| owner zmizne / je revokovaný                | projekt stráca operačnú pripravenosť     |
| explicitný commitment expiroval             | zníži sa aktívny commitment count        |
| participant ostal v zozname, ale je pasívny | nemusí sa počítať ako active participant |
| žiadna vetva / cieľ / criteria              | projekt stráca smer                      |
| dlhodobé ticho                              | `active → dormant`, neskôr `archived`    |

---

## 19. 🧠 MVPj vs UX

| Stage      | Čo používateľ vidí     | Ako sa surfacuje  |
| ---------- | ---------------------- | ----------------- |
| `draft`    | len owner              | nesurfacovať      |
| `forming`  | „formujúci sa projekt“ | obmedzene         |
| `active`   | normálny projekt       | plne              |
| `dormant`  | „neaktívny projekt“    | znížená priorita  |
| `archived` | história               | len na požiadanie |

### Dôležitý princíp

> Nie všetko, čo existuje, sa má zobrazovať ako plnohodnotný projekt.

---

## 20. 🚫 Čo sa NESMIE stať

| Zakázaná situácia                                | Prečo                        |
| ------------------------------------------------ | ---------------------------- |
| `active` projekt bez 2 aktívnych participantov   | predstiera kolektív          |
| commitment bez expiry / decay                    | vytvára falošnú podporu      |
| revalidácia len podľa kalendára                  | nerešpektuje realitu         |
| dormant → active bez medzikroku                  | lifecycle je príliš nervózny |
| policy windows prezentované ako invariant domény | zabetónuješ budúce zmeny     |

---

## 21. 📊 Príklad validného solo projektu (`forming`)

```json
{
  "node_id": "proj_garden_001",
  "node_type": "Project",
  "name": "Komunitná záhrada v Petržalke",
  "content": {
    "problem_statement": "V Petržalke chýba verejný priestor na komunitné pestovanie.",
    "goal_statement": "Overiť záujem a pripraviť prvý návrh záhrady.",
    "origin_topic_id": "topic_garden_001",
    "lifecycle_stage": "forming",
    "created_at": "2026-04-16T10:00:00Z"
  },
  "owner_id": "user_miro_001",
  "participant_ids": ["user_miro_001"],
  "initial_branch_id": "branch_main_001"
}
```

---

## 22. 📊 Príklad validného kolektívneho projektu (`active`)

```json
{
  "node_id": "proj_garden_001",
  "node_type": "Project",
  "name": "Komunitná záhrada v Petržalke",
  "content": {
    "problem_statement": "V Petržalke chýba verejný priestor na komunitné pestovanie.",
    "goal_statement": "Vytvoriť komunitnú záhradu s 20 záhonmi.",
    "success_criteria": "20 aktívnych členov a 2 podujatia ročne",
    "origin_topic_id": "topic_garden_001",
    "lifecycle_stage": "active",
    "created_at": "2026-04-16T10:00:00Z"
  },
  "owner_id": "user_miro_001",
  "participant_ids": ["user_miro_001", "user_jana_001"],
  "initial_branch_id": "branch_main_001",
  "signals": {
    "explicit_commitments": [
      {
        "user_id": "user_jana_001",
        "created_at": "2026-04-16T11:00:00Z",
        "expires_at": "2026-05-16T11:00:00Z"
      }
    ]
  }
}
```

---

## 23. 🧠 Dôsledky rozhodnutia

### ✅ Pozitíva

| Dôsledok                    | Popis                                                  |
| --------------------------- | ------------------------------------------------------ |
| podpora reality             | 1 človek môže začať                                    |
| žiadne falošné kolektívy    | active vyžaduje reálnu sociálnu oporu                  |
| lepšie dáta                 | active participant je kvalitnejší signál než raw count |
| stabilnejší lifecycle       | forming ↔ dormant ↔ active sú jasnejšie                |
| menší refaktor v budúcnosti | invarianty a policy sú oddelené                        |

### ❗ Negatíva / Trade-offs

| Dôsledok                    | Mitigácia                           |
| --------------------------- | ----------------------------------- |
| vyššia zložitosť definície  | ale výrazne lepšia budúca stabilita |
| potreba policy konfigurácie | defaulty pre pilot                  |
| viac stavovej logiky        | jasná process vrstva podľa ADR-075  |

---

## 24. 🚫 Alternatívy (zamietnuté)

| Alternatíva                                         | Dôvod zamietnutia        |
| --------------------------------------------------- | ------------------------ |
| 2 ľudia ako podmienka už pre `forming`              | zabíja solo začiatky     |
| raw `participant_count` ako jediný readiness signál | príliš hrubé             |
| owner „alive“ bez definície                         | architektonicky neurčité |
| dormant → active priamo                             | nestabilný lifecycle     |
| policy windows ako invarianty domény                | ťažké budúce zmeny       |

---

## 25. 🔗 Väzby na ďalšie ADR

| ADR     | Vzťah                                                |
| ------- | ---------------------------------------------------- |
| ADR-022 | formation musí vytvoriť existenčne validný project   |
| ADR-023 | distribuovaný project model                          |
| ADR-024 | branching poskytuje minimálny smer                   |
| ADR-026 | owner a participant roly                             |
| ADR-027 | surfacovanie rešpektuje readiness                    |
| ADR-031 | `origin_topic_id`                                    |
| ADR-075 | lifecycle transitiony sú process logika              |
| ADR-076 | typed projections nad projectom                      |
| ADR-078 | transition Topic → Project vytvára `forming` project |
| ADR-082 | validation implementuje invarianty a policy          |

---

## 26. 📏 Pravidlá implementácie

Systém musí:

1. rozlišovať existenčnú validitu a operačnú pripravenosť
2. umožniť solo `forming`
3. vyžadovať kolektívnu readiness pre `active`
4. používať pojem `active_participant`, nie len raw participant count
5. commitment signalom priradiť TTL / decay
6. oddeliť domain invarianty od operational policy windows
7. používať event-driven revalidáciu
8. mať fallback timer po období nečinnosti
9. používať hysterézu
10. vracať `dormant` projekty najprv do `forming`, nie rovno do `active`

---

## 27. 🔥 Zhrnutie

> **Projekt môže vzniknúť ako solo iniciatíva, ale aktívnym kolektívnym projektom sa stáva až vtedy, keď získa reálnu a aktuálnu sociálnu podporu.**

Tento ADR definuje:

* existenčné minimum projektu,
* operačnú pripravenosť,
* active participant,
* owner operational validity,
* commitment TTL/decay,
* oddelenie invariantov a policy,
* event-driven revalidáciu,
* hysterézu,
* a jasný návrat z dormant do forming.

---

## 28. 🧭 Finálna veta

> **Uvoľni vstup, sprísni výstup.**
>
> Synergetikum nesmie blokovať tiché začiatky, ale zároveň nesmie predstierať, že každý solo nápad alebo pasívna dvojica už tvoria skutočný aktívny projekt.

```

---

Toto je už verzia, ktorú by som architektovi dal bez pocitu, že tam ostali dôležité nejasnosti.

Najväčší upgrade oproti predchádzajúcej verzii je, že už nie je nejasné:
- čo je invariant,
- čo je policy,
- čo je active participant,
- čo znamená validný owner,
- a kam sa dormant projekt vracia.

Ak chceš, ďalší najlogickejší krok je teraz:
- **ADR-023** alebo
- **ADR-024**,

lebo práve tie dva budú po tomto ešte viac cítiť potrebu dotiahnutia do rovnakej tvrdosti.
```
