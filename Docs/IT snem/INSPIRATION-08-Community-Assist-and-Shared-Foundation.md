### `INSPIRATION-08-Community-Assist-and-Shared-Foundation.md`

# Návrh: Shared foundation modely pre rôzne komunity

## Východisko z pôvodného systému
Pôvodný systém mal koncept "Shared Foundation" – spoločný základný model, ktorý sa mení zriedkavo a môže byť výsledkom trusted sync mechanizmu. Toto bolo len naznačené, nie plne rozpracované.

## Návrh pre nový systém
Tento koncept by mal byť výrazne rozšírený:

### 8.1 Typy shared foundation modelov
| Typ | Popis | Príklad |
|-----|-------|---------|
| **Global minimal** | Základný model dodávaný so systémom | Jazykový model, základné kategórie |
| **Community foundation** | Model vytvorený z Trusted Circle | Zdravotný model pre komunitu diabetikov |
| **Domain foundation** | Model pre špecifickú oblasť | Poľnohospodársky model pre farmárov |
| **Project foundation** | Model špecifický pre jeden projekt | Model pre participatívny rozpočet mesta |

### 8.2 Ako vzniká shared foundation
1. Skupina používateľov v Trusted Circle sa dohodne na zdieľaní
2. Ich Personal AI modely vygenerujú agregovaný checkpoint
3. Checkpoint prejde validation gate
4. Nový foundation model je k dispozícii pre členov circle
5. Každý člen si ho môže prijať ako svoj "base" a naň stavať personal delta

### 8.3 Prepojenie na ADR-034
- Shared foundation je oddelený od personal delta
- Personal delta je vždy nadradený – foundation sa mení len výnimočne a konzervatívne


---

