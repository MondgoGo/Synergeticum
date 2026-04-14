### `INSPIRATION-02-Advanced-Filtering-and-View-Layer.md`

# Návrh: Pokročilé filtrovanie a personalizované pohľady

## Východisko z pôvodného systému
Pôvodný systém obsahoval mimoriadne podrobný návrh filtrovania pre "Bunky" – filtre podľa kľúčových slov, dátumu, lokality (rádius), kategórie (stromová štruktúra), tagov, osôb (s rôznymi rolami – founder, editor, suggester, commenter). Boli definované špecifické filtre pre rôzne typy buniek (udalosti podľa ceny, opakovania; miesta podľa otváracích hodín).

## Návrh pre nový systém
Tento koncept by mal byť jadrom **View Layer** (System Architecture Overview, bod 6.10). Personal AI by nemala len radiť otázky, ale mala by byť zodpovedná za **konštrukciu personalizovaného pohľadu na celý systém uzlov**.

- Personal AI používa svoj Interest Model na určenie, ktoré kategórie/tagy uzlov sú relevantné.
- Používa Preference Model na určenie priority rôznych typov filtrov (napr. niekto preferuje filtre podľa dátumu, iný podľa lokality).
- View Layer by nemal byť len pasívny filter, ale aktívny agent, ktorý *konštruuje* pohľad na základe profilu používateľa a kontextu (Context Model z ADR-034).

## Rozšírenie pre Personal AI
Personal AI by sa nemala učiť len z odpovedí na otázky, ale aj z *interakcií s filtrami* – čo používateľ filtruje, čo ignoruje, ako si nastavuje pohľady. To je implicitný vstup do Behavioral Modelu.

## Prepojenie na Micro-Federation
Filtre by mali byť synchronizovateľné v rámci Trusted Circles – napr. "použi filtre mojich priateľov pre tému X" ako forma sociálneho objavovania.

---
