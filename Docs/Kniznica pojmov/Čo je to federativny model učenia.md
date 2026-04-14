# Federatívne učenie

Federatívne učenie (federated learning) je spôsob, ako trénovať model **bez centralizovania dát**.

Namiesto toho, aby si všetky dáta poslal na server, spravíš presný opak:

👉 **model ide k dátam, nie dáta k modelu**

---

## 🧠 Základná definícia

**Federatívne učenie = distribuovaný tréning modelu, kde:**

* dáta ostávajú lokálne (mobil, desktop)
* každý uzol trénuje lokálne
* zdieľajú sa len **updatey modelu** (nie dáta)
* updatey sa agregujú do spoločného modelu

---

# ⚙️ Ako to funguje (flow)

### 1. Inicializácia

* existuje základný model (napr. ONNX model)
* pošle sa na zariadenia

### 2. Lokálny tréning

Každý node:

* použije svoje dáta
* upraví model (gradienty / váhy)

### 3. Zdieľanie updateov

Node pošle:

* nie dáta
* ale **zmeny modelu** (napr. gradienty)

### 4. Agregácia

Koordinátor (alebo peer):

* spojí updatey (napr. priemerovanie)

### 5. Distribúcia

* nový model sa pošle späť

### 6. Opakovanie

* cyklus sa opakuje

---

# 🧩 Hlavné princípy federatívneho učenia

Tu máš presne to, čo chceš — čisté princípy:

---

## 1. Lokalita dát (Data locality)

**Dáta neopúšťajú zariadenie**

* mobil = tréningové miesto
* server = len koordinácia

👉 základ privacy princíp

---

## 2. Distribuovaný tréning

Model sa netrénuje na jednom mieste.

* každý node = mini tréningový uzol
* paralelný tréning

👉 škálovanie bez centralizácie

---

## 3. Agregácia namiesto zdieľania dát

Zdieľaš:

* gradienty
* váhy
* alebo iné artefakty

Nezdieľaš:

* raw dáta

👉 kľúčový rozdiel oproti klasickému ML

---

## 4. Iteratívne zlepšovanie

Model sa zlepšuje v cykloch:

* train → share → aggregate → repeat

👉 postupná konvergencia

---

## 5. Heterogenita dát (Non-IID)

Každý node má iné dáta:

* iné správanie
* iné prostredie
* iné distribúcie

👉 toto je najväčší problém federatívneho učenia

---

## 6. Partial participation

Nie všetky zariadenia sú online:

* niekto spí
* niekto nemá Wi-Fi
* niekto nemá batériu

👉 systém musí fungovať aj s náhodným výberom node-ov

---

## 7. Trust a bezpečnosť

Nie každý update je dôveryhodný:

* môže byť chybný
* môže byť škodlivý

👉 potrebuješ:

* validation
* filtering
* weighting

---

## 8. Synchronizačný model

Môže byť:

### Centralized FL

* server zbiera updatey

### Decentralized FL (tvoj prípad)

* peer-to-peer
* rotating leader

👉 bez centrálneho bodu

---

## 9. Kompresia a efektivita

Updatey musia byť malé:

* mobil nemôže posielať 100MB modely

Používa sa:

* kvantizácia
* sparsity
* delta updates

---

## 10. Personalizácia vs globalita

Klasické FL:

* jeden spoločný model

Tvoj prístup:

* každý má svoj model
* federácia = len pomoc

👉 toto je **pokročilejšia verzia FL**

---

# 🧠 Typy federatívneho učenia

## 1. Federated Averaging (FedAvg)

* najzákladnejšie
* priemerovanie váh

👉 jednoduché, ale slabé pri heterogénnych dátach

---

## 2. Personalized FL

* každý má svoj model
* shared layer je len pomocný

👉 toto je presne tvoj smer

---

## 3. Decentralized FL

* bez servera
* peer-to-peer

👉 tvoj P2P model

---

## 4. Split Learning (len pre info)

* model rozdelený medzi klient a server

👉 menej relevantné pre teba

---

# ⚠️ Tvrdá realita (čo ľudia prehliadajú)

Tu ťa trochu „uzemním“:

## ❗ 1. FL ≠ magic privacy

* updatey môžu leaknúť info
* treba riešiť differential privacy (ak ideš deep)

---

## ❗ 2. Heterogenita zabíja kvalitu

* priemerovanie rôznych ľudí často zhorší model

---

## ❗ 3. Malé skupiny ≠ veľké zlepšenie

* 5 kamarátov ≠ Google-scale data

---

## ❗ 4. Komplexita exploduje

* versioning
* merge
* trust
* sync

👉 preto robíš správne, že to držíš:

* small circles
* selective sync

---

# 🧭 Ako to sedí na tvoj systém

Tvoj systém je:

👉 **Personalized Decentralized Federated Learning**

To znamená:

* personal model = základ
* federácia = doplnok
* P2P = infra
* trust circles = filter

To je:
👉 o level vyššie než klasické FL

---

# 🎯 Najpresnejšia veta

Ak by som to zhrnul jednou vetou:

👉 **Federatívne učenie je spôsob, ako sa modely učia spolu bez toho, aby si vymieňali dáta — len znalosti.**

---

Ak chceš, ďalší krok ti viem spraviť:

👉 porovnanie:

* federated learning vs tvoj model
* kde presne sa líšia
* čo si vlastne vymýšľaš nové (a čo už existuje)

To by bolo veľmi zaujímavé, lebo si blízko vlastnej architektúry.
