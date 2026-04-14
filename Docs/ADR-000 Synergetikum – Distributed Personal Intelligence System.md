# 🧾 ADR-000: Synergetikum – Distributed Personal Intelligence System

## 1. 📌 Status

**Accepted**

---

## 2. 🎯 Kontext a dôvod vzniku

### 2.1 Problém súčasných systémov

Súčasné digitálne platformy a AI systémy majú zásadné nedostatky:

| Problém | Popis |
|---------|-------|
| **Centrálna kontrola** | Modely a dáta vlastní a riadi centrálna autorita (korporácia, štát) |
| **Strata súkromia** | Používateľove dáta sú komoditou, nie súkromným vlastníctvom |
| **Manipulácia** | Algoritmy optimalizujú na engagement, nie na prospech používateľa |
| **Informačné bubliny** | Echo chambers a filter bubbles obmedzujú pohľad na svet |
| **Nedôvera** | Používateľ nevie, ako systém funguje, čo vie a prečo mu čo ukazuje |
| **Pasívna konzumácia** | Používateľ je konzument, nie spolutvorca |

---

### 2.2 Prečo potrebujeme Synergetikum

Tento systém vzniká preto, lebo:

1. **Každý človek myslí inak.** Jeden univerzálny model nevyhovuje nikomu dobre.

2. **Súkromie nie je luxus, ale základné právo.** Dáta o tom, ako človek rozmýšľa, sú tie najcitlivejšie.

3. **Kolektívna inteligencia nemusí znamenať centrálnu inteligenciu.** Najlepšie riešenia často vznikajú zdola, nie zhora.

4. **Technológia má slúžiť človeku, nie ho ovládať.** AI má byť nástrojom na zlepšenie myslenia, nie na jeho nahradenie.

5. **Spolupráca nevyžaduje podriadenie sa.** Ľudia môžu spolupracovať bez toho, aby stratili svoju autonómiu.

---

## 3. 👤 Perspektíva používateľa

### 3.1 Čo používateľ vidí a zažíva

Používateľ vníma Synergetikum ako:

- **Osobného digitálneho sprievodcu**, ktorý ho spoznáva, učí sa z jeho rozhodnutí a pomáha mu lepšie premýšľať.
- **Nástroj na objavovanie**, ktorý mu ukazuje relevantné otázky, témy, projekty a ľudí – podľa jeho záujmov a schopností.
- **Priestor na spoluprácu**, kde sa môže zapojiť do projektov, ktoré dávajú zmysel jemu a jeho komunite.
- **Dôveryhodné prostredie**, kde má plnú kontrolu nad tým, čo sa zdieľa a s kým.

---

### 3.2 Čo používateľ necíti (a to je zámer)

- Necíti, že je sledovaný centrálnou autoritou.
- Necíti, že je manipulovaný algoritmom.
- Necíti, že jeho dáta sú komoditou.
- Necíti tlak na neustálu aktivitu alebo engagement.

---

### 3.3 Prečo by ho mal používateľ chcieť

| Dôvod | Vysvetlenie |
|-------|-------------|
| **Lepšie rozhodnutia** | Personal AI mu pomáha vidieť viac perspektív a dôsledkov |
| **Relevantný obsah** | Systém mu ukazuje to, čo ho naozaj zaujíma, nie to, čo ho má udržať na platforme |
| **Zmysluplná spolupráca** | Nájde projekty a ľudí, s ktorými môže reálne niečo vytvoriť |
| **Súkromie a kontrola** | Nikto nemá prístup k jeho spôsobu myslenia bez jeho súhlasu |
| **Žiadny vendor lock-in** | Jeho model patrí jemu, môže ho preniesť, zálohovať, vypnúť |

---

## 4. 🧭 Základné princípy

### 4.1 Personal Ownership First

> Každý používateľ vlastní svoj Personal AI model. Nie je to požičaná služba, je to jeho digitálna inteligencia.

**Dôsledky:**
- Model je primárne lokálny (mobil, desktop)
- Žiadny centrálny model používateľa neexistuje
- Používateľ rozhoduje o zdieľaní

---

### 4.2 Human-in-the-loop

> AI nerobí rozhodnutia za človeka. AI kladie otázky, pomáha myslieť, ale rozhoduje človek.

**Dôsledky:**
- Žiadne automatické "schvaľovanie" za používateľa
- Každé rozhodnutie je explicitné a auditovateľné
- AI je asistent, nie náhrada

---

### 4.3 Decentralization by Default

> Neexistuje centrálny riadiaci uzol. Systém je sieť rovnocenných osobných inteligencií.

**Dôsledky:**
- Komunikácia je peer-to-peer
- Žiadny single point of failure
- Žiadna centrálna cenzúra alebo manipulácia

---

### 4.4 Emergence over Control

> Projekty, témy a spolupráce vznikajú prirodzene z interakcií, nie sú riadené zhora.

**Dôsledky:**
- Žiadny "admin", ktorý rozhoduje, čo je dôležité
- Kvalita emerguje z participácie, nie z dekrétu
- Systém je organický, nie mechanický

---

### 4.5 Collaboration without Submission

> Spolupráca nevyžaduje stratu autonómie. Ľudia môžu spolupracovať a zostať sami sebou.

**Dôsledky:**
- Projekty majú vetvy, nie jednu "správnu" cestu
- Rozhodnutia sú lokálne, nie centrálne
- Forkovanie je mechanizmus riešenia konfliktov

---

### 4.6 Transparency and Auditability

> Každé dôležité rozhodnutie je zaznamenané a overiteľné. Žiadne skryté mechanizmy.

**Dôsledky:**
- História zmien je nemenná
- Osobné rozhodnutia sú kryptograficky podpísané
- Systém je open-source a overiteľný

---

### 4.7 Privacy as Core Feature

> Súkromie nie je doplnok. Je to základná vlastnosť, na ktorej je systém postavený.

**Dôsledky:**
- Citlivé dáta nikdy neopúšťajú zariadenie
- Zdieľajú sa len agregované alebo anonymizované informácie
- Používateľ má plnú kontrolu nad tým, čo sa zdieľa

---

## 5. 🎯 Ciele systému

### 5.1 Primárne ciele

| Cieľ | Popis |
|------|-------|
| **Zlepšiť individuálne myslenie** | Pomôcť každému človeku lepšie premýšľať o komplexných témach |
| **Prepojiť myslenie ľudí** | Umožniť, aby rôzne perspektívy našli spoločnú reč |
| **Transformovať myslenie na projekty** | Premeniť diskusiu na reálnu spoluprácu a akciu |
| **Chrániť súkromie a autonómiu** | Žiadna centrálna autorita nemá prístup k spôsobu myslenia jednotlivca |

---

### 5.2 Sekundárne ciele

| Cieľ | Popis |
|------|-------|
| **Odstrániť informačné bubliny** | Personal AI má zabudovaný "exploration factor" – ukazuje aj nové, neznáme podnety |
| **Znížiť manipuláciu** | Žiadny centrálny algoritmus nemôže optimalizovať na engagement |
| **Umožniť offline fungovanie** | Systém funguje aj bez pripojenia, nie je závislý na cloude |
| **Byť open-source a overiteľný** | Každý si môže overiť, čo systém robí a ako |

---

## 6. ⚖️ Dôsledky rozhodnutí

### 6.1 Pozitívne dôsledky

| Oblasť | Dôsledok |
|--------|----------|
| **Súkromie** | Vysoká ochrana dát, žiadne centrálne profilovanie |
| **Dôvera** | Používateľ má kontrolu, systém je transparentný |
| **Personalizácia** | Skutočne individuálne prispôsobenie, nie priemerný profil |
| **Odolnosť** | Žiadny single point of failure, systém môže fungovať offline |
| **Sloboda** | Používateľ nie je viazaný na žiadnu centrálnu platformu |

---

### 6.2 Negatívne dôsledky / Trade-offs

| Oblasť | Dôsledok | Mitigácia |
|--------|----------|-----------|
| **Výpočtová náročnosť** | Modely bežia na zariadení, nie v cloude | Optimalizované malé modely (MobileNet, TinyBERT), Parent Node pre výkonnejšie výpočty |
| **Synchronizácia** | Distribuovaný systém je zložitejší | Trusted circles, epizodická synchronizácia, konzervatívny merge |
| **Cold start** | Nový používateľ nemá vybudovaný model | Shared foundation modely, onboarding otázky, voliteľný community assist |
| **Globálna uniformita** | Neexistuje jeden "pravdivý" model | To je vlastnosť, nie chyba – personalizácia je cieľ |
| **Komplexita UX** | Používateľ musí rozumieť režimom (Solo, Trusted Circle, Community Assist) | Jednoduché prednastavené režimy, advanced len pre pokročilých |

---

### 6.3 Produktové dôsledky

| Dôsledok | Popis |
|----------|-------|
| **Onboarding je kritický** | Používateľ musí pochopiť hodnotu lokálneho modelu a dôverovať mu |
| **Dôvera sa buduje postupne** | Systém začína v Solo režime, až potom ponúka zdieľanie |
| **Žiadne reklamy ani engagement optimalizácia** | Systém nemôže byť financovaný predajom pozornosti používateľa |
| **Open source je nutnosť** | Bez overiteľnosti nie je dôvera možná |

---

## 7. 🗺️ Smery vývoja

### 7.1 Fáza 1: Základná osobná inteligencia (Solo)

- Health model (najjednoduchší, najpraktickejší)
- Lokálne učenie, žiadne zdieľanie
- Jednoduchá fusion layer
- Základné otázky a odpovede

**Cieľ:** Overiť, že lokálny model môže byť užitočný aj sám o sebe.

---

### 7.2 Fáza 2: Trusted circles pre Health model

- Pridanie peer discovery a základnej synchronizácie
- Konzervatívny merge a validation gate
- Wi‑Fi / battery gating

**Cieľ:** Overiť mikro-federáciu na najjednoduchšom module.

---

### 7.3 Fáza 3: Behavior model a lepšia personalizácia

- Pridanie Behavior modelu (sekvenčné dáta, návyky)
- Foundation + personal delta princíp
- Weighted merge podľa trustu a kvality

**Cieľ:** Personalizácia založená na reálnom správaní, nie len na explicitných odpovediach.

---

### 7.4 Fáza 4: Vision model a bohatšie artefakty

- Pridanie Vision modelu (mobilné kamery, fotografie)
- Richer sync payloads (embeddings, logits, centroids)
- Community assist režim (anonymizované znalosti)

**Cieľ:** Rozšírenie vstupných modalít a zdieľanie bez straty súkromia.

---

### 7.5 Fáza 5: Text model a argumentácia

- Pridanie Text modelu (veľmi opatrne kvôli citlivosti)
- Prepojenie na argumentačný graf
- Rozšírenie projektov o komplexné rozhodovanie

**Cieľ:** Podpora pre komplexné témy (ústava, pravidlá, etika).

---

### 7.6 Fáza 6: Plná P2P a otvorená sieť

- Rotating leader alebo full P2P koordinácia
- Reputačný model bez centralizácie
- Škálovanie na väčšie komunity

**Cieľ:** Maximálna decentralizácia a odolnosť.

---

## 8. 🔗 Prepojenie na existujúce ADR

Tento ADR je zakladajúci. Definuje "prečo" a "čo" – všetky ostatné ADR definujú "ako".

| ADR | Vzťah |
|-----|-------|
| ADR-001 (Vlastníctvo osobného modelu) | Implementuje princíp "Personal Ownership First" |
| ADR-034 (Personal AI Internal Model) | Definuje, čo Personal AI obsahuje |
| ADR-014 (Network Communication) | Implementuje decentralizáciu a P2P |
| ADR-015 (Participation Filter) | Implementuje Human-in-the-loop a selekciu |
| ADR-007 (Validation Gate) | Implementuje konzervatívny merge a dôveru |
| ADR-009 (Heterogénne uzly) | Implementuje mobil + desktop architektúru |

---

## 9. 🚫 Čo systém NIE JE (Anti-scope)

Toto je rovnako dôležité ako to, čo systém JE.

| Nie je | Vysvetlenie |
|--------|-------------|
| **Sociálna sieť** | Nie je cieľom udržať používateľa na platforme čo najdlhšie |
| **Chatbot platforma** | AI kladie otázky, nevedie voľné konverzácie |
| **Centralizovaná AI služba** | Model patrí používateľovi, nie prevádzkovateľovi |
| **Hlasovací systém** | Rozhodnutia sú lokálne, nie centrálne agregované |
| **Nástroj na manipuláciu** | Systém nemôže byť použitý na ovplyvňovanie používateľov bez ich vedomia |
| **Big Data projekt** | Dáta zostávajú lokálne, nie sú centrálne zbierané |

---

## 10. 📏 Úspešnosť systému (Metriky)

Ako vieme, že systém funguje?

| Metrika | Popis |
|---------|-------|
| **Používateľ si buduje model** | Personal AI sa učí a zlepšuje v čase |
| **Používateľ sa zapája do projektov** | Z otázok vznikajú reálne projekty a spolupráce |
| **Používateľ dôveruje systému** | Nízka miera opt-outu zo synchronizácie, vysoká miera zapnutých trusted circles |
| **Kvalita rozhodnutí** | Používateľ subjektívne vníma, že sa rozhoduje lepšie |
| **Decentralizácia funguje** | Systém beží bez centrálneho uzla, žiadny single point of failure |

---

## 11. 🔥 Zhrnutie (Prečo to celé robíme)

> **Synergetikum je distribuovaný systém osobných digitálnych inteligencií, ktoré patria používateľom, učia sa lokálne, komunikujú peer-to-peer a premieňajú individuálne myslenie na kolektívnu inteligenciu a reálnu spoluprácu.**

Robíme to preto, lebo:

1. **Súčasné systémy sú centralizované a nedôveryhodné.** Používateľ je produkt, nie zákazník.

2. **Súkromie nie je možné zaistiť centrálne.** Jediný spôsob, ako ochrániť spôsob myslenia človeka, je nenechať ho opustiť jeho zariadenie.

3. **Kolektívna inteligencia nevyžaduje centrálnu autoritu.** Najlepšie riešenia často vznikajú zdola, nie zhora.

4. **AI má slúžiť človeku, nie ho nahradiť.** Našou úlohou je pomáhať ľuďom lepšie myslieť, nie myslieť za nich.

5. **Spolupráca je možná bez podriadenia sa.** Ľudia môžu spolupracovať a zostať slobodní.

---

## 12. 🧭 Finálna veta

> **Bez vlastníctva modelu používateľom systém stráca dôveru. Bez dôvery stráca zmysel. Bez zmyslu nemá právo existovať.**

Toto je dôvod, prečo Synergetikum vzniká. A toto je dôvod, prečo ho používatelia budú chcieť.

---

**Dátum:** 2026-04-14  
**Stav:** Accepted  
**Typ:** Foundation ADR (úroveň 0)