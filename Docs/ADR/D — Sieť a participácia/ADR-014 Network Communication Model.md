# 🧾 ADR-014: Network Communication Model (Peer-to-Peer)

---

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

Systém Personal AI Network pozostáva z:

* lokálnych Personal AI modelov (ADR-001)
* používateľov, ktorí interagujú cez otázky
* potreby zdieľať perspektívy a umožniť spoluprácu

---

### Problém

Je potrebné definovať:

👉 **ako medzi sebou komunikujú Personal AI modely**

tak, aby:

* bol zachovaný decentralizovaný charakter systému
* nebola porušená súkromnosť používateľov
* vznikla kolektívna inteligencia
* nevznikol centralizovaný kontrolný bod

---

### Kľúčová otázka

👉
**Má byť systém centralizovaný alebo peer-to-peer?
A ak peer-to-peer – za akých pravidiel komunikujú uzly (modely)?**

---

## 3. ⚖️ Rozhodnutie

---

## 🌐 3.1 Architektúra

👉
**Systém bude implementovaný ako peer-to-peer (P2P) sieť Personal AI modelov.**

---

To znamená:

* každý uzol = Personal AI model používateľa
* uzly komunikujú priamo medzi sebou
* neexistuje centrálna AI autorita

---

---

## 🔗 3.2 Typ komunikácie

Komunikácia medzi uzlami prebieha:

👉 **na úrovni modelových výstupov, nie dát**

---

Zdieľané môžu byť:

* odpovede na otázky
* agregované názory
* návrhy riešení
* anonymizované signály

---

NIE:

* surové dáta
* kompletné modely
* osobné informácie

---

---

## 🧠 3.3 Autonómia uzla

Každý Personal AI model:

👉 **rozhoduje autonómne o svojej účasti v sieti**

---

To znamená:

* rozhodne:

  * či sa zapojí do otázky
  * či odpovie
  * čo odpovie

---

👉 neexistuje globálne riadenie

---

---

## 🚦 3.4 Podmienky komunikácie

Komunikácia medzi uzlami prebieha len ak sú splnené:

---

### 🔹 Relevancia

* otázka je relevantná pre profil používateľa

---

### 🔹 Povolenie

* používateľ umožnil participáciu

---

### 🔹 Lokálne rozhodnutie modelu

* model vyhodnotí, že má čo pridať

---

---

## 🔄 3.5 Typy P2P interakcie

---

### 1. Collective Query

* otázka sa distribuuje do siete
* uzly odpovedajú

---

### 2. Signal Sharing

* uzly zdieľajú slabé signály (napr. záujem o tému)

---

### 3. Opportunity Matching

* identifikácia, kto sa vie zapojiť

---

---

## 🔐 3.6 Ochrana súkromia

---

Systém musí zabezpečiť:

* anonymizáciu komunikácie
* minimalizáciu zdieľaných informácií
* nemožnosť rekonštrukcie identity

---

👉 Personal AI nikdy neposiela interný stav modelu

---

---

## ⚖️ 3.7 Neexistencia centrálneho riadenia

---

Systém NESMIE obsahovať:

* centrálny rozhodovací uzol
* centrálny model
* globálny ranking používateľov

---

👉 sieť je:

**emergentná, nie riadená**

---

---

## 🌐 3.8 Hybridné prvky (povolené)

---

Systém môže obsahovať:

* discovery mechanizmy
* bootstrap uzly
* pomocné služby

---

ALE:

👉 nesmú:

* kontrolovať rozhodnutia
* vlastniť dáta
* byť kritickým bodom systému

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitívne

---

### 🔒 Súkromie

* dáta ostávajú lokálne

---

### 🧩 Škálovateľnosť

* sieť rastie prirodzene

---

### ⚖️ Nezávislosť

* žiadna centrálna kontrola

---

### 🌐 Kolektívna inteligencia

* vzniká emergentne

---

---

## ❗ Negatíva / Trade-offs

---

### ⚙️ Komplexita

* P2P je zložitejšie než server model

---

### 📉 Nekonzistentnosť

* odpovede nie sú deterministické

---

### 🔄 Latencia

* odpovede závisia od siete

---

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Centrálna serverová architektúra

* jednoduchá
* ale:

  * strata súkromia
  * strata kontroly

---

---

### ❌ Federated learning (klasický)

* čiastočne decentralizovaný
* ale:

  * stále koordinovaný centrálne

---

👉 zamietnuté kvôli:

* nedostatočnej autonómii uzlov

---

---

## 6. 🔗 Väzby na ďalšie ADR

---

* ADR-001 (Ownership Model)
* ADR-015 (Participation Filter)
* ADR-016 (Collective Query System)
* ADR-017 (Collaboration Model)

---

---

## 7. 📏 Pravidlá implementácie (high-level)

---

Systém musí:

* umožniť P2P komunikáciu medzi uzlami
* zabezpečiť autonómiu rozhodovania uzla
* minimalizovať zdieľané dáta
* zabezpečiť anonymitu

---

---

# 🔥 8. Zhrnutie

---

👉
**Systém je decentralizovaná peer-to-peer sieť Personal AI modelov, kde každý uzol autonómne rozhoduje o svojej účasti a komunikuje len cez anonymizované výstupy.**

---

---

# 🧭 Finálna veta

👉
**Bez P2P architektúry by systém stratil svoju decentralizovanú podstatu – a stal by sa klasickou AI službou.**

---

