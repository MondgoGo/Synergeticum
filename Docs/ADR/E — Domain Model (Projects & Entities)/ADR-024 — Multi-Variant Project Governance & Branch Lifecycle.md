# 🧾 ADR-024: Multi-Variant Project Governance & Branch Lifecycle

---

## 1. 📌 Status

**Proposed (updated)**

---

## 2. 🎯 Kontext

Systém Personal AI Network:

* umožňuje vznik projektov (ADR-022)
* funguje decentralizovane (ADR-014)
* reprezentuje projekty ako distribuované objekty (ADR-023)

---

Pri vývoji projektu:

* vznikajú rôzne názory
* dochádza ku konfliktom
* neexistuje jednoznačná „pravda“

---

### Problém

Je potrebné definovať:

👉 **ako sa projekt vyvíja pri existencii viacerých konkurenčných návrhov**

---

Bez riešenia:

* vznikajú konflikty
* dominujú silné osobnosti
* projekt stagnuje alebo zlyhá

---

### Kľúčová otázka

👉
**Ako umožniť paralelný vývoj viacerých riešení bez nutnosti centrálneho rozhodovania?**

---

## 3. ⚖️ Rozhodnutie

---

## 🌳 3.1 Projekt ako množina variantov

👉
**Projekt nie je jedna verzia – je množina variantov (branchov)**

---

Každý variant:

* reprezentuje konkrétny smer riešenia
* existuje paralelne s ostatnými

---

---

## 🔀 3.2 Branching model

---

Nový variant vzniká keď:

* existuje zásadná odlišnosť
* existuje konflikt
* nie je dosiahnutá zhoda

---

👉 namiesto konfliktu:

👉 vzniká nová vetva

---

---

## 🧠 3.3 Neexistencia jedného víťaza

---

Systém NESMIE:

* automaticky vybrať víťazný variant
* potlačiť menšinové názory

---

👉 varianty koexistujú

---

---

## ⚖️ 3.4 Realita ako rozhodovací mechanizmus

---

Variant „vyhráva“ iba ak:

* má podporu (ADR-028)
* má zdroje
* je realizovaný

---

👉 nie hlasovaním

---

---

# 🧠 3.5 Branch Lifecycle (NOVÉ)

---

Každý variant má životný cyklus:

---

## 🟢 Active

* má podporu
* má aktivitu
* má rast

---

---

## 🟡 Dormant

* slabá aktivita
* slabý commitment
* stagnácia

---

---

## 🔴 Archived

* dlhodobo bez podpory
* bez aktivity

---

---

## ❗ Dôležité pravidlá

---

👉 archivácia ≠ zmazanie

* vetva zostáva v histórii
* môže byť obnovená

---

👉 prechod medzi stavmi je:

* automatický
* založený na aktivite a podpore

---

---

# 🧠 3.6 Faktory lifecycle

---

Stav vetvy je ovplyvnený:

* alignment (záujem)
* commitment (zapojenie)
* aktivita (zmeny, diskusia)
* čas (bez pohybu)

---

---

# 🧠 3.7 Ochrana proti zahlteniu

---

Systém musí:

* obmedziť počet aktívnych vetiev
* zvýrazniť relevantné vetvy (ADR-027)

---

👉 ostatné sú:

* dormant
* alebo archived

---

---

# 🧠 3.8 Evolúcia projektu

---

Projekt:

* rastie do viacerých smerov
* môže sa deliť
* môže zlučovať (voliteľne)

---

👉 nie je lineárny

---

---

# 🧠 3.9 Branch lineage

---

Každý variant obsahuje:

* parent vetvu
* históriu vzniku

---

👉 umožňuje sledovať evolúciu

---

---

# 🧠 3.10 Vzťah k iniciátorovi

---

Iniciátor:

* vytvára prvú vetvu
* má vplyv na začiatku

---

ALE:

👉 nemá kontrolu nad ďalším vývojom

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitíva

---

### ⚖️ Eliminácia konfliktov

* namiesto boja vznikajú vetvy

---

### 🌐 Sloboda vývoja

* každý smer môže existovať

---

### 🔄 Adaptabilita

* projekt sa prispôsobuje

---

---

## ❗ Negatíva / Trade-offs

---

### ⚠️ Fragmentácia

* veľa vetiev

---

### ⚙️ Komplexita

* ťažšie riadenie

---

👉 riešené cez:

* lifecycle
* view layer

---

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Jeden finálny dokument

* jednoduchý
* ale:

  * konfliktný
  * centralizovaný

---

---

### ❌ Hlasovanie o víťazovi

* intuitívne
* ale:

  * manipulovateľné

---

---

## 6. 🔗 Väzby na ďalšie ADR

---

* ADR-022 (Project Formation)
* ADR-023 (Distributed Model)
* ADR-027 (View Layer)
* ADR-028 (Support Model)

---

---

## 7. 📏 Pravidlá implementácie (high-level)

---

Systém musí:

* umožniť vznik vetiev
* sledovať lifecycle vetvy
* archivovať neaktívne vetvy
* zachovať históriu
* umožniť obnovu vetvy

---

---

# 🔥 8. Zhrnutie

---

👉
**Projekt je dynamický strom variantov, kde jednotlivé vetvy vznikajú, vyvíjajú sa a zanikajú prirodzene na základe podpory a aktivity.**

---

---

# 🧭 Finálna veta

---

👉
**Namiesto toho, aby systém nútil jednu pravdu, umožňuje, aby rôzne pravdy existovali – a realita ukáže, ktorá prežije.**

---

---

# 👉 Čo teraz

---

Teraz máš veľmi silný základ:

* distributed projekt
* branching
* lifecycle
* support
* view

---

👉 chýba posledný kľúčový kus:

👉 **ADR-015: Participation Filter**

---

Ten rozhodne:

* kto sa zapojí
* kto nie
* a prečo

---
