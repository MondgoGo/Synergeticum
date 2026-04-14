# Project Object Concept Model

## Synergetikum

---

# 1. Purpose

Tento dokument definuje:

👉 **aká je vnútorná štruktúra 'Témy' v systéme Synergetikum**

Cieľom je:

* definovať základné komponenty 'Témy'
* vytvoriť spoločný mentálny model pre architektov, developerov a analytikov

---

# 2. Core Definition

'Téma' je:

> **distribuovaný, evolučný objekt, ktorý reprezentuje kolektívne myslenie, rozhodovanie a potenciálnu realizáciu okolo jednej témy alebo zámeru.**

'Téma' NIE JE:

* dokument
* chat
* súbor

'Téma' JE:

* strom variantov
* tok udalostí
* mapa argumentov
* profil podpory
* základ pre spoluprácu

---

# 3. High-Level Structure

'Téma' sa skladá z 5 hlavných častí:

1. Project Identity
2. Branch Structure
3. Event Stream
4. Support & Participation State
5. Argumentation Graph

---

# 4. Project Identity

Stabilná, minimálne sa meniaca časť.

Obsahuje:

* project_id
* názov
* krátky popis
* iniciátora
* typ 'Témy' (execution / consensus / conceptual)
* governance mode
* timestamp vzniku

Úloha:

* jednoznačne identifikovať projekt v sieti
* umožniť discovery a referencovanie

---

# 5. Branch Structure (Core of the Project)

'Téma' je:

👉 **strom vetiev (branches)**

---

## Každá vetva obsahuje:

* branch_id
* parent_branch_id
* názov variantu
* popis riešenia
* stav (proposed / active / stable / dormant / archived)
* timestamp vzniku

---

## Vlastnosti:

* vetvy existujú paralelne
* vetvy sa môžu deliť
* vetvy sa môžu oživiť
* vetvy sa nikdy nemažú (len archivujú)

---

## Úloha 'Témy':

* reprezentovať rôzne riešenia
* eliminovať konflikty cez paralelný vývoj

---

# 6. Event Stream (Evolution Engine)

'Téma' sa neukladá ako statický stav, ale ako:

👉 **tok udalostí (event stream)**

---

## Typy udalostí:

* project_created
* branch_created
* branch_updated
* argument_added
* support_changed
* participant_joined
* participant_left
* branch_state_changed

---

## Vlastnosti:

* každá zmena je event
* eventy sú chronologické
* eventy sa distribuujú medzi uzlami

---

## Úloha:

* umožniť synchronizáciu
* uchovať históriu
* umožniť rekonštrukciu stavu

---

# 7. Derived State (Current Snapshot)

Z eventov sa odvádza:

👉 **aktuálny stav 'Témy'**

---

Obsahuje:

* aktuálne vetvy
* ich stavy
* aktuálnu podporu
* aktívnych participantov

---

👉 toto je „snapshot“ používaný v UI

---

# 8. Support & Participation State

Každá vetva má profil podpory.

---

## Dimenzie:

* alignment
* commitment
* confidence
* (optional) expertise signal

---

## Každý participant má:

* typ zapojenia
* úroveň aktivity
* históriu participácie

---

## Úloha:

* určiť životnosť vetvy
* ovplyvniť lifecycle
* prepojiť projekt s realitou

---

# 9. Argumentation Graph (for Conceptual Projects)

Používa sa hlavne pri:

* pravidlách
* stratégii
* legislatíve
* komplexných rozhodnutiach

---

## Obsahuje:

* argumenty
* protiargumenty
* vzťahy medzi nimi

---

## Každý argument obsahuje:

* claim
* reasoning
* assumptions
* implications

---

## Úloha:

* stabilizovať diskusiu
* oddeliť ego od obsahu
* umožniť deliberáciu

---

# 10. Participation Mapping

'Téma' obsahuje mapu zapojenia:

---

## Participant entity:

* participant_id
* rola (initiator / contributor / executor)
* typ participácie
* aktivita

---

## Úloha:

* vedieť „kto je v projekte“
* prepojiť projekt s ľuďmi
* umožniť Opportunity Layer

---

# 11. Lifecycle Integration

Vetvy aj projekt reagujú na:

* support
* aktivitu
* čas

---

## Prechody:

* active → dormant
* dormant → archived
* archived → revived

---

👉 systém sa čistí prirodzene

---

# 12. Distribution Model

'Téma':

* existuje na viacerých uzloch
* je replikovaný
* synchronizuje sa cez eventy

---

## Dôležité:

* neexistuje master verzia
* neexistuje centrálna autorita
* konflikty → branching

---

# 13. View Projection (User Experience)

Používateľ nikdy nevidí celý projekt.

---

Personal AI vytvára:

👉 **view projection**

---

Obsahuje:

* vybrané vetvy
* zhrnutia
* konflikty
* odporúčané otázky

---

# 14. Minimal Concept Model (Summary)

'Téma' =

* Identity
* Branch Tree
* Event Stream
* Support Signals
* Argument Graph
* Participant Map

---

# 15. Design Constraints

'Téma' musí:

* byť distribuovaný
* byť rekonštruovateľný z eventov
* byť odolný voči konfliktom
* byť škálovateľný
* byť interpretovateľný cez AI

---

# 16. What This Enables

Tento model umožňuje:

* decentralizované projekty
* paralelný vývoj riešení
* kolektívne myslenie bez chaosu
* prepojenie myslenia a akcie

---

# 17. Final Statement

> 'Téma' v Synergetiku nie je dokument ani verzia pravdy.
> Je to živý distribuovaný systém možností, ktorý sa vyvíja v čase a odráža kolektívne myslenie ľudí.

---
