### `INSPIRATION-11-Calendar-and-Event-Coordination.md`

# Návrh: Kalendár a koordinácia udalostí

## Východisko z pôvodného systému
Pôvodný systém mal kalendár pre udalosti (eventy) s atribútmi:
- Začiatok/koniec
- Pravidelné opakovanie
- Cena
- Miesto (fyzické/online)

## Návrh pre nový systém
Tento koncept by mal byť súčasťou **Project Formation Layer** a **Opportunity Layer**:

### 11.1 Udalosť ako špeciálny typ Node
- Udalosť je Node s typom `Event`
- Môže byť súčasťou projektu (napr. "Stavba komunitnej záhrady – brigáda v sobotu")
- Môže byť samostatná (napr. "Debata o ústave – 15.5. vo Výmenníku")

### 11.2 Personalizovaný kalendár
- Personal AI agreguje udalosti relevantné pre používateľa (podľa Interest a Capability modelu)
- Kalendár je synchronizovaný medzi zariadeniami používateľa
- V rámci Trusted Circle je možné zdieľať kalendáre

### 11.3 Koordinácia bez centrálneho servera
- Udalosť je distribuovaný objekt – každý participant má jej kópiu
- Zmeny (posun termínu, zmena miesta) sa synchronizujú P2P


---
