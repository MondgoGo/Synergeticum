### `INSPIRATION-01-Unified-Entity-Model.md`

# Návrh: Zavedenie univerzálneho modelu entít ("Bunka")

## Východisko z pôvodného systému
Pôvodný systém definoval univerzálnu jednotku nazývanú "Bunka" (Cell), ktorá mohla reprezentovať čokoľvek: Tému, Produkt, Miesto, Udalosť, Skupinu, Osobu. Každá bunka mala jednotnú štruktúru atribútov (ID, názov, popis, tvorca, čas, história, kategórie, tagy, miesto, vzťahy, práva).

## Návrh pre nový systém
Do novej architektúry Personal AI odporúčam prevziať tento koncept ako základný dátový model. Namiesto izolovaných entít (Questions, Projects, Arguments) by sme mali mať **univerzálnu entitu "Node"**.

Tento uzol by mal byť fraktálny – každý uzol (otázka, projekt, argument, vetva) má rovnakú základnú štruktúru a môže obsahovať alebo odkazovať na iné uzly.

## Prepojenie na ADR-034 (Personal AI Internal Model)
- Interest Model by sa mal mapovať na tieto uzly (napr. záujem o "Topic Node").
- Capability Model by sa mal mapovať na typy uzlov, ktoré používateľ vytvára alebo v nich participuje.
- Preference Model by mal ovplyvňovať, ktoré typy uzlov a aké vzťahy medzi nimi Personal AI preferuje.

## Prepojenie na System Architecture
- "Project" a "Branch" by mali byť špecifické typy tohto univerzálneho Node.
- Argumentation Layer by mal byť špecifický pohľad na graf týchto uzlov a ich vzťahov.

## Príklad použitia
```json
{
  "node_id": "proj_pizzeria_001",
  "node_type": "Project", // alebo "Question", "Argument", "Topic", "Person"
  "name": "Komunitná pizzéria v Trnave",
  "attributes": {
    "status": "active",
    "timeline": { "start": "2026-05-01", "end": null }
  },
  "relations": [
    { "target_id": "topic_food_001", "type": "classified_as", "strength": 0.9 },
    { "target_id": "user_miro_001", "type": "created_by" },
    { "target_id": "branch_001", "type": "has_variant" }
  ],
  "rights": { "visibility": "circle", "edit": "founders" }
}
```

---
