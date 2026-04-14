### `INSPIRATION-06-Tree-and-Structure-Visualization.md`

# Návrh: Stromová štruktúra a vizualizácia vplyvu

## Východisko z pôvodného systému
Pôvodný systém obsahoval koncept "Strom" (Tree), ktorý sledoval:
- Kto koho priviedol do systému (retezová reakcia)
- Aký mal každý človek prínos (počet získaných ľudí, počet slov v prijatých návrhoch, počet lajkov, získané financie)
- Transparentné vs súkromné údaje (veci týkajúce sa viacerých sú verejné, osobné sú súkromné)

## Návrh pre nový systém
Tento koncept by mal byť implementovaný ako **sieť dôvery a vplyvu (Trust & Influence Graph)** s nasledujúcimi vlastnosťami:

### 6.1 Sledovanie prínosu
Pre každého používateľa by Personal AI evidovala (s jeho súhlasom):
- **Influence metrics**: Koľko ľudí začalo používať systém na jeho odporúčanie
- **Contribution metrics**: Koľko jeho návrhov/argumentov bolo prijatých do projektov
- **Engagement metrics**: Ako sa zapája do projektov (response rate, commitment rate)

### 6.2 Transparentnosť v skupinách
- V rámci Trusted Circle sú niektoré metriky viditeľné pre ostatných
- Používateľ môže vidieť, kto je v projekte najaktívnejší, kto najviac prispieva
- Žiadne skryté "skóre" – všetky metriky sú odvodené z reálnych, overiteľných udalostí

### 6.3 Prepojenie na ADR-034
- Tieto metriky by mali byť súčasťou **ParticipationProfile** (ADR-034 DTO)
- Behavioral Model by sa mal učiť z toho, ako sa používateľ zapája a aký má vplyv

### 6.4 Príklad použitia
```json
{
  "user_id": "user_miro_001",
  "influence": {
    "direct_referrals": 5,
    "tree_referrals": 42,
    "referral_quality_score": 0.73
  },
  "contributions": {
    "accepted_proposals": 12,
    "total_words_accepted": 2450,
    "likes_received": 87
  },
  "transparent_only_in_circle": {
    "activity_score": 0.68,
    "reliability_score": 0.82
  }
}
```


---

