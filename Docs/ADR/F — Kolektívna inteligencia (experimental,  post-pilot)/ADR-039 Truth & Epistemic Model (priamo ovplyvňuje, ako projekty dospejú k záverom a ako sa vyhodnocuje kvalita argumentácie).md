# 🧾 ADR-039 — Truth & Epistemic Model

---

## 1. 📌 Status

Draft (Exploration – implementácia až po pilotovi)

---

## 2. 🎯 Kontext

Synergetikum je decentralizovaný systém bez centrálnej autority, ktorý:

* podporuje diskusiu a argumentáciu (ADR-025)
* umožňuje vznik projektov a ich vetvenie (ADR-024)
* odmieta manipuláciu a autoritatívne riadenie (ADR-003, ADR-002)

To vytvára zásadnú otázku:

> **Čo je pravda v systéme, kde nikto nemá finálne slovo?**

---

### Problém

V tradičných systémoch:

* pravdu určuje autorita
* alebo väčšina

V Synergetiku:

* autorita neexistuje
* väčšina môže byť nesprávna
* expert môže byť prehliadaný
* silný rečník môže dominovať

---

👉 bez epistemického modelu:

* systém môže konvergovať k nesprávnym záverom
* alebo nikdy nekonverguje vôbec

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Core princíp

---

> **Pravda nie je explicitne určená systémom – je výsledkom prežitia argumentov v konfrontácii.**

---

To znamená:

* systém neurčuje „pravdu“
* systém poskytuje prostredie, kde sa pravda môže objaviť

---

---

## 🧩 3.2 Pravda ≠ konsenzus

---

> **Konsenzus nie je automaticky pravda.**

---

* väčšina môže byť nesprávna
* konsenzus môže byť výsledok tlaku alebo nedostatku informácií

---

👉 systém:

* eviduje konsenzus
* ale neoznačuje ho ako pravdu

---

---

## 🧠 3.3 Pravda ≠ autorita

---

> **Expertíza je signál, nie dôkaz pravdy.**

---

* expert môže byť nesprávny
* laik môže mať správny insight

---

👉 systém:

* umožňuje zohľadniť expertízu
* ale neprivilegizuje ju automaticky

---

---

## 🧠 3.4 Pravda ako proces

---

> **Pravda je dynamický proces, nie statický stav.**

---

To znamená:

* závery sa môžu meniť
* nové argumenty môžu zmeniť smer
* staré „pravdy“ môžu zaniknúť

---

👉 systém:

* uchováva históriu vývoja pravdy

---

---

## 🧠 3.5 Argument ako nositeľ pravdy

---

> **Jediným nositeľom pravdy v systéme je argument.**

---

Nie:

* hlas
* identita
* počet podporovateľov

---

👉 ale:

* kvalita reasoning
* odolnosť voči protiargumentom
* konzistentnosť

---

---

## 🧠 3.6 Pluralita pravdy (multi-branch realita)

---

> **Systém umožňuje existenciu viacerých paralelných „pravd“.**

---

To znamená:

* viac vetiev projektu môže byť validných
* neexistuje povinná konvergencia

---

👉 používateľ:

* si vyberá vetvu
* alebo sleduje viac vetiev

---

---

## 🧠 3.7 Epistemická pokora systému

---

> **Systém si je vedomý, že môže byť nesprávny.**

---

To znamená:

* neuzatvára diskusiu definitívne
* umožňuje návrat k starým témam
* podporuje revíziu záverov

---

---

## 🧠 3.8 Vzťah k argumentácii (ADR-025)

---

* argument graph reprezentuje „mapu pravdy“
* silné argumenty ovplyvňujú vetvy
* protiargumenty testujú stabilitu záverov

---

---

## 🧠 3.9 Vzťah k moci (ADR-038)

---

Epistemický model:

* bráni tomu, aby pravdu určovali silní jednotlivci
* oddeľuje pravdu od vplyvu

---

> Pravda nevzniká z moci.
> Moc nesmie definovať pravdu.

---

---

## 🧠 3.10 Vzťah k participácii (ADR-036)

---

* viac participácie ≠ viac pravdy
* participácia zlepšuje kvalitu modelu, nie pravdivosť tvrdení

---

---

## 🧠 3.11 Vzťah k chaosu (ADR-040)

---

* systém toleruje šum
* pravda sa formuje aj v neideálnych podmienkach

---

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitíva

---

### 🧠 Odolnosť voči manipulácii

* nemožno „prehlasovať pravdu“

---

### 🌐 Pluralita pohľadov

* systém nie je rigidný

---

### 🔄 Adaptivita

* pravda sa môže vyvíjať

---

---

## ❗ Negatíva / Trade-offs

---

### ⚖️ Absencia finálnej odpovede

* systém nemusí dospieť k jednému záveru

---

### 🧠 Vyššia kognitívna náročnosť

* používateľ musí interpretovať výsledky

---

### ⚙️ Potenciál fragmentácie

* veľa vetiev → zložitosť

---

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Majority voting

* vedie k tyranii väčšiny

---

---

### ❌ Expert authority model

* vedie k elitizmu

---

---

### ❌ Central truth authority

* porušuje decentralizáciu

---

---

### ❌ AI ako arbiter pravdy

* porušuje human-in-the-loop

---

---

## 6. 🔗 Väzby na ďalšie ADR

---

* ADR-002 — Human-in-the-loop
* ADR-003 — Non-manipulation
* ADR-025 — Argumentation
* ADR-036 — Reciprocity
* ADR-038 — Power & Influence
* ADR-040 — Graceful Degradation

---

---

## 7. 📏 Pravidlá implementácie (high-level)

---

Systém musí:

* reprezentovať argumenty ako základ pravdy
* umožniť existenciu viacerých vetiev
* nevyhodnocovať pravdu centrálne
* umožniť revíziu záverov
* oddeliť pravdu od identity a moci

---

---

# 🔥 8. Zhrnutie

---

👉
**Synergetikum neurčuje pravdu – vytvára priestor, kde pravda môže vzniknúť a byť neustále testovaná.**

---

---

# 🧭 Finálna veta

---

👉
**V Synergetiku nie je pravda to, na čom sa všetci zhodnú, ale to, čo prežije konfrontáciu s pochybnosťou.**
