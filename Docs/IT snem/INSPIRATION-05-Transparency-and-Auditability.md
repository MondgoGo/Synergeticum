### `INSPIRATION-05-Transparency-and-Auditability.md`

# Návrh: História, klonovanie a auditovateľnosť rozhodnutí

## Východisko z pôvodného systému
Pôvodný systém kládol dôraz na nemennú históriu ("historia sa neda zmazazt, ak ju niekto uz videl") a mechanizmus klonovania (forkovania) celých tém aj s ich históriou, právami a vzťahmi.

## Návrh pre nový systém
Toto sú kľúčové princípy pre dôveryhodnosť a decentralizáciu:

1.  **Nemenný auditný log**:
    - Každá zmena v projekte (nová vetva, zmena stavu, prijatie argumentu) by mala byť zaznamenaná v nemennom logu.
    - Tento log je súčasťou "Project Node" a je replikovaný medzi participantmi.
    - Žiadny používateľ (ani founder) by nemal mať právo zmazať historický záznam.

2.  **Forkovanie (klonovanie) projektov**:
    - Ak dôjde k nezhode, ktokoľvek by mal mať právo "forknúť" celý projekt – vytvoriť jeho kópiu s vlastnou históriou, vlastnými pravidlami a vlastnou komunitou.
    - Toto je základný mechanizmus pre riešenie konfliktov bez centrálnej autority.
    - Forks by mali byť prepojené s pôvodným projektom (vzťah "forked_from").

3.  **Osobné rozhodnutie ako auditný záznam**:
    - Keď používateľ urobí rozhodnutie (napr. "podporujem vetvu A"), toto rozhodnutie je auditným záznamom v jeho lokálnom ProfileEventLog (ADR-034 DTO).
    - Rozhodnutie je kryptograficky podpísané používateľom (súkromný kľúč na zariadení) a môže byť overené kýmkoľvek.

## Prepojenie na Micro-Federation
- Synchronizácia auditných logov medzi participantmi projektu je kritická.
- Pri forkovaní projektu by sa mal synchronizovať len stav a história, nie nutne zoznam participantov (tí si vyberú, ku ktorému forku sa pripoja).

---
