# 🧾 ADR-041 — Recovery Storage & Snapshot Strategy

---

## 1. 📌 Status

Proposed (Implementation-level ADR)

---

## 2. 🎯 Kontext

ADR-037 definuje:

* **čo je identity recovery**
* prečo je kritická pre dôveru
* aké princípy musí spĺňať

Tento dokument dopĺňa:

> **ako je recovery implementované na úrovni dát, storage a synchronizácie**

---

### Problém

Recovery musí byť:

* spoľahlivý
* bezpečný
* decentralizovaný
* rekonštruovateľný

A zároveň:

* nesmie vytvárať single point of failure
* nesmie kompromitovať súkromie

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Čo sa obnovuje (Recovery Scope)

---

Recovery obsahuje:

* posledný **snapshot Personal AI modelu**
* **delta udalosti** od posledného snapshotu
* metadáta potrebné na rekonštrukciu

---

👉 systém obnovuje:

> **kompletný stav identity v čase posledného validného checkpointu**

---

---

## 🧩 3.2 Snapshot stratégia

---

### 📦 Typ snapshotu:

* full snapshot + incremental updates

---

### ⏱️ Frekvencia:

* default: **1× za 24 hodín**
* adaptívne (vyššia aktivita → častejšie snapshoty)

---

### 📉 Maximálna strata dát (Recovery window):

* default: **max 24 hodín interakcií**

---

👉 systém garantuje:

> obnova nikdy nestratí viac než definované recovery okno

---

---

## 🔐 3.3 Šifrovanie

---

### 🔑 Model:

* end-to-end encryption

---

### 🔒 Algoritmus:

* AES-256 (symetrické šifrovanie dát)
* kľúč spravovaný používateľom

---

👉 systém:

* nikdy nemá prístup k dešifrovaným dátam
* pracuje len so šifrovanými blobmi

---

---

## 🧩 3.4 Split knowledge (Secret sharing)

---

Recovery dáta sú rozdelené:

### 📊 Parametre:

* počet shardov: **5**
* threshold pre obnovu: **3 z 5**

---

### 📍 Umiestnenie:

* parent node: 1 shard
* trusted circle: 3 shards
* offline seed (user): 1 shard / key

---

👉 princíp:

> žiadna entita nemá kompletný prístup k identite

---

---

## 🧠 3.5 Recovery cesty

---

### 🔹 Primárna

* parent node

---

### 🔹 Sekundárna

* kombinácia shardov (trusted circle)

---

### 🔹 Fallback

* offline seed + dostupné shards

---

---

## 🔁 3.6 Verziovanie a kompatibilita

---

Každý snapshot obsahuje:

* version identifier modelu
* version identifier storage formátu

---

👉 systém:

* podporuje migráciu medzi verziami
* zabezpečuje spätnú kompatibilitu

---

---

## 🔄 3.7 Conflict handling (branching)

---

Pri obnove:

* vzniká nová vetva identity
* snapshot sa nikdy neprepíše existujúci stav

---

👉 merge je:

* manuálny
* riadený používateľom

---

---

## 🧠 3.8 Validácia recovery

---

Systém:

* umožňuje simulovanú obnovu (dry run)
* pravidelne kontroluje dostupnosť shardov

---

### ⏱️ Interval validácie:

* default: 1× mesačne

---

👉 cieľ:

> zabezpečiť recovery confidence (ADR-037)

---

---

## 🧠 3.9 Single point of failure eliminácia

---

Systém garantuje:

> žiadny jednotlivý komponent nie je nevyhnutný pre obnovu identity

---

---

## 🧠 3.10 Storage formát

---

Recovery dáta sú uložené ako:

* komprimovaný binárny formát
* verzovaný
* optimalizovaný pre prenos

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitíva

---

### 🔒 Silná bezpečnosť

* šifrovanie + split knowledge

---

### 🔁 Robustnosť

* viacero recovery ciest

---

### 🧠 Predvídateľnosť

* garantované recovery okno

---

---

## ❗ Negatíva / Trade-offs

---

### ⚙️ Komplexita

* shard management
* sync koordinácia

---

### 📉 Overhead

* snapshot storage
* sieťová komunikácia

---

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Central cloud backup

* porušuje ownership

---

---

### ❌ Single backup file

* single point of failure

---

---

### ❌ Bez snapshotov (len event log)

* príliš pomalá rekonštrukcia

---

---

## 6. 🔗 Väzby na ďalšie ADR

---

* ADR-037 — Identity Recovery
* ADR-030 — Sync & Persistence
* ADR-014 — P2P Communication
* ADR-036 — Reciprocity

---

---

## 7. 📏 Pravidlá implementácie

---

Systém musí:

* snapshotovať pravidelne
* šifrovať všetky dáta
* distribuovať shards
* validovať recovery
* umožniť fallback recovery

---

---

# 🔥 8. Zhrnutie

---

👉
**Recovery v Synergetiku je kombinácia snapshotov, šifrovania a distribuovaného uloženia bez jediného bodu zlyhania.**

---

---

# 🧭 Finálna veta

---

👉
**Používateľova identita nie je uložená na jednom mieste – je rekonštruovaná z viacerých bezpečných fragmentov.**

