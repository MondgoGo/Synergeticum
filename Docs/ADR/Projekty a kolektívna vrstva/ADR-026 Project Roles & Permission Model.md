# 🧾 ADR-026 — Project Roles & Permission Model

---

## 1. 📌 Status

Proposed

---

## 2. 🎯 Kontext

Synergetikum umožňuje vznik projektov z kolektívnych otázok a diskusií (ADR-016, ADR-022, ADR-024).

Projekt:

* nie je vlastníctvom jednej osoby
* vzniká z kolektívneho procesu
* môže sa vetviť a evolvovať

---

### Problém

> **Ako definovať roly a oprávnenia v projekte bez toho, aby vznikla centralizovaná moc alebo vlastník projektu?**

---

Bez riešenia:

* projekty sa môžu zvrhnúť na osobné vlastníctvo
* vzniknú konflikty a boj o kontrolu
* alebo naopak chaos bez zodpovednosti

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Core princíp

---

> **Projekt nemá vlastníka. Má účastníkov s rôznymi rolami a capability.**

---

👉 rola:

* neznamená moc nad projektom
* znamená **typ participácie**

---

---

## 🧩 3.2 Role model (základné roly)

---

### 🧱 Initiator

* osoba, ktorá projekt založila
* definuje počiatočný smer

---

👉 nemá:

* absolútnu kontrolu
* právo blokovať ostatných

---

---

### 🧠 Contributor

* prispieva:

  * nápadmi
  * argumentmi
  * návrhmi

---

👉 základná rola systému

---

---

### ⚙️ Executor

* realizuje konkrétne časti projektu
* vykonáva akcie (napr. stavba, implementácia)

---

---

### 🧭 Coordinator (implicitná rola)

* pomáha organizovať
* prepája ľudí

---

👉 nie je autorita

---

---

## 🧠 3.3 Capability model (namiesto „permissions“)

---

> **Systém nepoužíva klasické „permissions“, ale capability (schopnosti vykonávať akcie).**

---

Capability sú:

* kontextové
* viazané na aktivitu
* viazané na konkrétnu vetvu projektu

---

---

### Príklady capability:

* navrhnúť zmenu
* vytvoriť vetvu
* spojiť vetvy
* realizovať úlohu
* označiť milestone

---

---

👉 capability:

* nie sú trvalé
* nie sú viazané na status
* nevytvárajú hierarchiu

---

---

## 🧠 3.4 Initiator ≠ vlastník

---

> **Iniciátor má vyšší vplyv na začiatku projektu, ale tento vplyv prirodzene klesá.**

---

Dôvod:

* projekt rastie
* zapájajú sa ďalší ľudia

---

👉 systém:

* neudržiava permanentnú výhodu iniciátora

---

---

## 🧠 3.5 Evolúcia rolí

---

Roly:

* nie sú statické
* vznikajú z aktivity

---

👉 používateľ môže byť:

* contributor v jednom projekte
* executor v inom
* iniciátor v ďalšom

---

---

## 🧠 3.6 Neexistencia centrálnej autority

---

Systém:

❌ nemá admina projektu
❌ nemá „owner“ rolu
❌ nemá absolútne práva

---

👉 všetko je:

> výsledok interakcie účastníkov

---

---

## 🧠 3.7 Konflikty a riešenie

---

Pri konflikte:

* nevzniká „víťaz“
* vznikajú vetvy (ADR-024)

---

👉 každá vetva:

* reprezentuje alternatívny smer

---

---

## 🧠 3.8 Vzťah k argumentácii (ADR-025)

---

* roly nemajú vplyv na kvalitu argumentov
* argument > rola

---

---

## 🧠 3.9 Vzťah k moci (ADR-038)

---

* roly nesmú vytvárať mocenskú hierarchiu
* vplyv nevzniká z roly

---

---

## 🧠 3.10 Vzťah k participácii (ADR-036)

---

* vyššia participácia → viac capability (dočasne)
* ale:

> participácia ≠ autorita

---

---

## 🧠 3.11 Vzťah k identity (ADR-037)

---

* roly nie sú súčasť identity
* sú kontextové

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitíva

---

### ⚖️ Žiadna centralizácia moci

* projekt nemôže byť „ukradnutý“

---

### 🔄 Flexibilita

* roly sa menia prirodzene

---

### 🤝 Podpora spolupráce

* každý sa môže zapojiť

---

---

## ❗ Negatíva / Trade-offs

---

### 🧭 Nejasná zodpovednosť

* nie je jeden „boss“

---

### ⚙️ Komplexita koordinácie

* vyžaduje dobrý UX

---

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Owner-based model

* jeden vlastník projektu

👉 vedie k centralizácii

---

---

### ❌ Role hierarchy (admin/moderátor)

* fixné roly

👉 vedie k moci

---

---

### ❌ Permission system (ACL)

* statické oprávnenia

👉 nepružné

---

---

## 6. 🔗 Väzby na ďalšie ADR

---

* ADR-022 — Project Formation
* ADR-024 — Branching Model
* ADR-025 — Argumentation
* ADR-036 — Reciprocity
* ADR-038 — Power & Influence
* ADR-040 — Graceful Degradation

---

---

## 7. 📏 Pravidlá implementácie

---

Systém musí:

* umožniť rôzne formy participácie
* nepriraďovať trvalé roly
* viazať capability na aktivitu
* neumožniť absolútnu kontrolu
* podporovať vetvenie pri konflikte

---

---

# 🔥 8. Zhrnutie

---

👉
**Projekt v Synergetiku nemá vlastníka – má účastníkov, ktorí ho spolu vytvárajú.**

---

---

# 🧭 Finálna veta

---

👉
**Rola neurčuje moc. Určuje len spôsob, akým sa podieľaš na tvorbe.**

---

