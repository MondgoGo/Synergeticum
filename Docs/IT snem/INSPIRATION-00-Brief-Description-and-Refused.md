## ❌ ZAMietnuté koncepty (čo NEMÁ zmysel prenášať)

### 1. Komunikátor (email, SMS, messenger integrácia)
**Prečo nie:** Nový systém nie je komunikačná platforma. Integrácia externých messengerov je mimo scope – to je práca pre iné aplikácie. Synergetikum sa zameriava na **myslenie a spoluprácu**, nie na univerzálnu správu.

### 2. Detailné "práva" ako v pôvodnom dokumente
**Prečo nie:** Pôvodný systém mal extrémne granularne práva (founder, editor, suggester, commenter, viewer – každé s vlastnými pravidlami). To je zbytočne komplexné. Nový systém vystačí s:
- `owner` (zakladateľ projektu)
- `participant` (aktívny člen)
- `observer` (len sleduje)

Zvyšok rieši Trust Circle mechanizmus.

### 3. "Snem spolutvorcov" a formálne hlasovanie
**Prečo nie:** Pôvodný systém mal ťažkopádny mechanizmus "snemov" a "hlasovaní". Nový systém je postavený na **emergence** – rozhodnutia vznikajú prirodzene z participácie a commitmentu, nie z formálnych hlasovaní. Kto sa reálne zapojí, ten rozhoduje.

### 4. Detailné "typy buniek" (Topic, Product, Place, Event, Group, People)
**Prečo nie:** Univerzálny Node model (INSPIRATION-01) to zjednodušuje. Nemusíme mať 6 rôznych typov s rôznymi atribútmi – stačí jeden typ s `node_type` atribútom a flexibilnými vlastnosťami.

### 5. P2P prostredia (Holochain, Cardano, Zeronet)
**Prečo nie:** Výber konkrétnej technológie je implementačný detail, nie architektonický princíp. ADR-001 a Micro-Federation už definujú princípy – konkrétnu P2P vrstvu netreba riešiť na úrovni inšpirácií.

### 6. Blockchain a kryptomeny
**Prečo nie:** "Hodina práce" ako kryptomena je mimo scope. Systém nemá vlastnú menu. Financovanie projektov je samostatný problém, ktorý môže byť riešený inými nástrojmi.

### 7. Detailné "vlastné atribúty" buniek
**Prečo nie:** Pôvodný systém umožňoval pridávať ľubovoľné vlastné atribúty k bunkám. To je cesta k neudržateľnému chaosu. Nový systém by mal mať pevnú, ale rozšíriteľnú schému (napr. cez `attributes` JSON, ale s validáciou).

### 8. "Parovanie dokumentov" ako samostatný koncept
**Prečo nie:** Párovanie (INSPIRATION-03) je už dostatočne všeobecné. Netreba ho špecializovať na "dokumenty".

### 9. "Ukladanie komunikácie P2P"
**Prečo nie:** Znova – komunikácia nie je jadro systému. Stačí, že projektové dáta a argumenty sú distribuované. Ukladanie každej správy je zbytočné.

### 10. "Štatutári", "OZ", "neziskovka"
**Prečo nie:** Právna forma je organizačná, nie architektonická. Nie je relevantná pre technický návrh systému.

---

## ✅ FINÁLNY ZOZNAM INŠPIRÁCIÍ (po redukcii)

| # | Názov | Status |
|---|-------|--------|
| 01 | Unified Entity Model | ✅ **Ponechať** – základný kameň |
| 02 | Advanced Filtering | ✅ **Ponechať** – jadro View Layer |
| 03 | Pairing Engine | ✅ **Ponechať** – Opportunity Layer |
| 04 | Process Rules | ✅ **Ponechať** – pre projektový životný cyklus |
| 05 | Transparency & Auditability | ✅ **Ponechať** – pre dôveru |
| 06 | Tree Visualization | ✅ **Ponechať** – pre metriky vplyvu |
| 07 | Forum Structures | ✅ **Ponechať** – pre argumentáciu |
| 08 | Shared Foundation | ✅ **Ponechať** – pre community assist |
| 09 | Topic Taxonomy | ✅ **Ponechať** – pre Interest Model |
| 10 | Ratings & Quality | ✅ **Ponechať** – pre kvalitu argumentov |
| 11 | Calendar & Events | ✅ **Ponechať** – pre koordináciu |

**Celkom: 11 inšpirácií** (z pôvodných 16 navrhovaných)

## 📋 Zhrnutie

Pôvodný `IT tématický Snem - Synergeticum.md` obsahoval:
- **Veľa dobrých myšlienok** – tie sme extrahovali do 11 inšpirácií
- **Veľa zbytočností** – komunikačné nástroje, blockchain, formálne hlasovania, právne formy
- **Veľa implementačných detailov** – technológie, databázy, konkrétne API

Nový systém by sa mal sústrediť na **j adro**: osobné modely, decentralizáciu, argumentáciu, projekty. Zvyšok je šum.

---
