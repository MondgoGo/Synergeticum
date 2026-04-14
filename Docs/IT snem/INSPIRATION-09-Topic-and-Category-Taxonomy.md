### `INSPIRATION-09-Topic-and-Category-Taxonomy.md`

# Návrh: Stromová kategória a sémantické vzťahy

## Východisko z pôvodného systému
Pôvodný systém obsahoval:
- Stromovú štruktúru kategórií (hierarchia)
- Tagy ako priame prepojenia (neuronová sieť)
- Vzťahy medzi bunkami (parent, related, solves, contradicts)

## Návrh pre nový systém
Tento koncept by mal byť jadrom pre **Interest Model** a **Question Layer**:

### 9.1 Dvojúrovňová klasifikácia
| Úroveň | Popis | Príklad |
|--------|-------|---------|
| **Kategória (strom)** | Hierarchická, dedičná | "Jedlo → Pečenie → Chlieb" |
| **Tagy (graf)** | Voľné, nehierarchické | "#bezlepkové", "#kváskové", "#komunitné" |

### 9.2 Sémantické vzťahy medzi témami
- `parent_of` / `child_of` (hierarchia)
- `related_to` (príbuzné, nie hierarchické)
- `contradicts` (protichodné)
- `supports` (podporuje)
- `evolves_from` (vývojová nadväznosť)

### 9.3 Prepojenie na ADR-034
- Interest model by mal byť štruktúrovaný podľa tejto taxonómie
- Personal AI sa učí nielen "čo používateľa zaujíma", ale aj "aké vzťahy medzi témami preferuje"

### 9.4 Príklad
```json
{
  "topic_key": "food_business",
  "taxonomy": {
    "category_path": ["business", "small_business", "food_business"],
    "tags": ["#startup", "#local", "#sustainable"],
    "relations": [
      { "target": "community_projects", "type": "related_to", "strength": 0.7 },
      { "target": "constitutional_topics", "type": "contradicts", "strength": 0.1 }
    ]
  }
}
```


---

