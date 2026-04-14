# 🧾 ADR-034: Personal AI Internal Model

---

# 1. 📌 Status

**Proposed**

---

# 2. 🎯 Kontext

Personal AI je základný stavebný prvok systému Synergetikum.

Doteraz je definovaná ako:

* lokálny model používateľa
* súkromná
* rozhoduje o participácii
* filtruje obsah
* interpretuje projekty

---

### Problém

Nie je definované:

👉 čo presne Personal AI obsahuje
👉 čo sa učí
👉 ako sa mení

---

Bez toho:

* systém je neoveriteľný
* správanie je nepredvídateľné
* architektúra je neúplná

---

# 3. ⚖️ Rozhodnutie

Personal AI je definovaná ako:

👉 **lokálny adaptívny model preferencií, schopností a rozhodovacích vzorcov používateľa**

---

# 4. 🧠 Core Model

Personal AI obsahuje 5 hlavných vrstiev:

---

## 4.1 Interest Model (záujmy)

Reprezentuje:

* témy, ktoré používateľa zaujímajú
* intenzitu záujmu

---

Príklad:

```json
{
  "food": 0.8,
  "business": 0.6,
  "politics": 0.2
}
```

---

---

## 4.2 Capability Model (schopnosti)

Reprezentuje:

* čo vie používateľ robiť
* v čom vie prispieť

---

Príklad:

```json
{
  "construction": 0.7,
  "finance": 0.4,
  "design": 0.2
}
```

---

---

## 4.3 Preference Model (rozhodovacie preferencie)

Reprezentuje:

* ako používateľ rozmýšľa

---

Príklady dimenzií:

* risk_tolerance
* long_term_vs_short_term
* individual_vs_collective

---

---

## 4.4 Behavioral Model (správanie)

Reprezentuje:

* ako sa používateľ reálne správa

---

Obsahuje:

* odpovede na otázky
* participáciu
* commitment
* ignorované témy

---

👉 rozdiel medzi tým čo hovorí vs čo robí

---

---

## 4.5 Context Model (situácia)

Reprezentuje:

* kde je
* v akej situácii
* aké má obmedzenia

---

Príklady:

* lokalita
* čas
* dostupnosť

---

---

# 5. 🔄 Learning Mechanism

---

Personal AI sa učí z:

---

## 5.1 Explicit input

* odpovede na otázky

---

## 5.2 Implicit input

* kliky
* ignorovanie
* participácia

---

## 5.3 Behavioral feedback

* commitment vs realita

---

---

# 6. 🧠 Update Model

Každý input:

* mierne upravuje model
* neprepíše ho

---

👉 model je:

* stabilný
* ale adaptívny

---

---

# 7. ⚖️ Identity vs Noise

---

Model musí:

* odolávať náhodným vstupom
* reflektovať dlhodobé správanie

---

👉 krátkodobý signál ≠ zmena identity

---

---

# 8. 🧠 Output použitia

Personal AI používa model na:

---

## 8.1 Question selection

čo ukázať

---

## 8.2 Participation filter

kde sa zapojiť

---

## 8.3 Project interpretation

čo je relevantné

---

## 8.4 Role suggestion

ako sa zapojiť

---

---

# 9. 🔒 Privacy

Model:

* je lokálny
* neopúšťa zariadenie
* nie je zdieľaný

---

Zdieľajú sa len:

* agregované signály

---

---

# 10. ⚠️ Bias control

---

Model nesmie:

* uzatvárať používateľa
* vytvárať echo chamber

---

Musí obsahovať:

* exploration factor
* náhodné nové podnety

---

---

# 11. 📊 Dôsledky

---

## ✅ Pozitíva

* personalizácia
* vyššia relevancia
* adaptivita

---

## ❗ Negatíva

* komplexita
* potreba ladenia

---

---

# 12. 🔗 Väzby

* ADR-033 Participation
* ADR-032 Questions
* ADR-031 Topics

---

---

# 🔥 13. Zhrnutie

👉
**Personal AI je dynamický model identity používateľa, ktorý sa učí z jeho správania a rozhodnutí a riadi jeho interakciu so systémom.**

---

# 🧭 Finálna veta

👉
**Systém nepozná používateľa.
Jeho Personal AI si ho postupne vytvára.**

---
