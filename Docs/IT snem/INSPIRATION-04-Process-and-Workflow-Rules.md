### `INSPIRATION-04-Process-and-Workflow-Rules.md`

# Návrh: Procesné pravidlá pre vývoj projektu a rozhodovanie

## Východisko z pôvodného systému
Pôvodný systém mal prepracovaný procesný rámec pre vývoj témy/dokumentu, ktorý zahŕňal:
- Hierarchiu: Návrh (laik) → Spracovanie (spolutvorca) → Schválenie (snem) → Finálna verzia.
- Pravidlá, kto môže čo (vidieť, navrhovať, schvaľovať, editovať).
- Mechanizmy: Klonovanie (fork) a spájanie (merge) nápadov.

## Návrh pre nový systém
Nový systém definuje "Project", "Branch" a "Decision Flow", ale chýba mu **formálny procesný model**. Odporúčam implementovať:

1.  **Stavy projektu**:
    - `Ideation` (otázka/diskusia)
    - `Proposal` (konkrétny návrh)
    - `Review` (posudzovanie)
    - `Active` (realizácia)
    - `Completed` / `Archived`
    (Toto by boli atribúty v univerzálnom modeli uzla)

2.  **Role v rámci uzla/projektu** (prevzaté z pôvodného systému):
    - `founder` (zakladateľ)
    - `editor` (môže upravovať)
    - `reviewer` (môže schvaľovať/odmietať zmeny)
    - `contributor` (prispieva, ale nemení)
    - `observer` (len sleduje)

3.  **Pravidlá pre zmenu stavu**:
    - Kto môže presunúť projekt z `Proposal` do `Review`? (napr. traja editori)
    - Kedy sa vetva (`Branch`) stáva hlavnou? (napr. 2/3 reviewerov súhlasí)
    - Tieto pravidlá by mali byť súčasťou uzla ako "governance policy".

## Prepojenie na Personal AI
- Personal AI by mala používať Preference Model (napr. `risk_tolerance`, `individual_vs_collective`) na odporúčanie, aké procesné pravidlá používateľovi navrhnúť pre jeho projekty.
- Behavioral Model by sa mal učiť z toho, ako používateľ dodržiava (alebo nedodržiava) tieto pravidlá.

## Prepojenie na ADR-034 (Internal Model)
Tieto roly a pravidlá by mali byť explicitne zohľadnené v Capability Modeli (schopnosť byť editorom, reviewerom) a Participation Profile (ako sa používateľ zapája do rôznych fáz projektu).

---
