### `INSPIRATION-07-Forum-and-Discussion-Structures.md`

# Návrh: Štruktúrovaná diskusia a filtrácia príspevkov

## Východisko z pôvodného systému
Pôvodný systém obsahoval podrobný návrh pre diskusie:
- Filter príspevkov podľa času, priateľov, likeov, počtu reakcií
- Komentáre viazané ku konkrétnym častiam dokumentu
- Oddelenie diskusie o návrhu od diskusie o dokumente

## Návrh pre nový systém
Tento koncept by mal byť súčasťou **Argumentation Layer** a **View Layer**:

### 7.1 Kontextová diskusia
- Každý argument, každá vetva, každý projekt má vlastný diskusný priestor
- Diskusia je viazaná na konkrétny "Node" (univerzálny model)

### 7.2 Personalizované filtrovanie diskusie
- Personal AI používa Interest Model na zobrazenie relevantných príspevkov
- Používateľ môže filtrovať podľa:
  - Dátumu (najnovšie/najstaršie)
  - Dôveryhodnosti autora (v rámci Trusted Circle)
  - Počtu podporných reakcií
  - Relevantnosti k jeho záujmom

### 7.3 Threading a štruktúra
- Diskusie sú stromové (reakcie na reakcie)
- Každý príspevok môže mať typ: `question`, `suggestion`, `support`, `objection`, `clarification`

### 7.4 Prepojenie na Micro-Federation
- Diskusné príspevky sú synchronizované medzi participantmi projektu
- Nie je potrebný centrálny server – každý peer má kópiu diskusie


---

