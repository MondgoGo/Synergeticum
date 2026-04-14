### `INSPIRATION-10-Ratings-and-Quality-Metrics.md`

# Návrh: Hodnotenie a metriky kvality

## Východisko z pôvodného systému
Pôvodný systém obsahoval niekoľko typov hodnotenia:
- Hlasovanie (áno/nie, zhoda)
- Hviezdičkové hodnotenie (1-5)
- Like
- Poradie
- Textová recenzia

## Návrh pre nový systém
Tento koncept by mal byť implementovaný ako **Quality Layer** nad argumentmi, vetvami a projektmi:

### 10.1 Typy hodnotení
| Typ | Popis | Kde sa používa |
|-----|-------|----------------|
| **Alignment** | Súhlas/nesúhlas s argumentom | Argumentation Layer |
| **Confidence** | Ako istý je používateľ v svojom rozhodnutí | Decision Flow |
| **Clarity** | Ako zrozumiteľný je argument | Peer review |
| **Relevance** | Ako relevantný je argument k téme | Filtrovanie |
| **Commitment** | Ochota sa reálne zapojiť | Project Flow |

### 10.2 Prevencia manipulácie
- Hodnotenia nie sú anonymné – sú viazané na identitu (ale v rámci Trusted Circle)
- Žiadne "bot" hodnotenia – každý hlas patrí konkrétnemu používateľovi
- História hodnotení je auditovateľná

### 10.3 Prepojenie na ADR-034
- Preference model by mal obsahovať, ako používateľ hodnotí (prísny/zhovievavý)
- Behavioral model sa učí z hodnotiacich vzorcov používateľa


---

