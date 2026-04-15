# 🧾 ADR-025: Argumentation & Reasoning Model (Hardened v3)

> **Zmeny oproti Version 2 (Hardened v2):**
> - **Doplnená časť 3.16:** Metriky kvality argumentu (logická konzistencia, relevantnosť, podloženosť faktami)
> - **Doplnená časť 3.17:** Užívateľské hodnotenie argumentov
> - **Doplnená časť 3.18:** Oddelenie kvality od popularity
> - **Rozšírená časť 3.7:** Hodnotenie argumentov (nové kritériá)
> - **Rozšírená časť 6:** Väzby na ADR-003, ADR-028, ADR-042

---

## 1. 📌 Status

**Accepted (v3 – Hardened with Quality Metrics)**

---

## 2. 🎯 Kontext

Systém Personal AI Network:

* umožňuje vznik projektov (ADR-022)
* podporuje multi-variant vývoj (ADR-024)
* používa support model (ADR-028)
* filtruje participáciu (ADR-015)

Tieto mechanizmy dobre fungujú pre:

* realizačné projekty (napr. pizzéria)

### Problém

Pre **konceptuálne projekty** (napr. ústava, pravidlá, hodnoty):

* neexistuje jednoznačné „overenie realitou“
* commitment nestačí
* hlasovanie je manipulovateľné
* dominujú:

  * silné osobnosti
  * emócie
  * rétorika

👉 systém potrebuje inú vrstvu:

> **Argumentačnú vrstvu, ktorá nielen štrukturuje diskusiu, ale umožňuje, aby argumenty mali reálny dopad na vývoj projektu bez vzniku mocenských hierarchií.**

### Kľúčová otázka

👉 **Ako umožniť kvalitnú kolektívnu diskusiu a rozhodovanie bez dominancie ega, manipulácie a chaosu?**

### Dodatočný problém (v3)

Aj pri štruktúrovanej argumentácii hrozí riziko:

* **popularity bias** – populárny argument vyzerá ako „dobrý“
* **neoveriteľná kvalita** – nevieme rozlíšiť kvalitný argument od zlého
* **manipulácia davom** – veľa ľudí môže podporiť nekvalitný argument

👉 systém potrebuje **metriky kvality** a **oddelenie kvality od popularity**.

---

## 3. ⚖️ Rozhodnutie

### 🧠 3.1 Zavedenie „Argumentation Layer“

Systém zavádza:

👉 **Argumentation & Reasoning Layer**

Táto vrstva:

* štrukturuje diskusiu
* oddeľuje argument od osoby
* umožňuje porovnanie návrhov
* prepojuje argumenty na vetvy projektu
* **hodnotí kvalitu argumentov (nové v v3)**
* **oddeľuje kvalitu od popularity (nové v v3)**

---

### 🧩 3.2 Argument ako základná jednotka

Základná entita nie je:

❌ komentár
❌ názor

👉 ale:

👉 **argument**

---

### 🧾 3.3 Štruktúra argumentu

Každý argument obsahuje:

| Komponent | Popis |
|-----------|-------|
| **Claim (tvrdenie)** | čo tvrdím |
| **Reasoning (odôvodnenie)** | prečo to tvrdím |
| **Assumptions (predpoklady)** | na čom to stojí |
| **Implications (dôsledky)** | čo to spôsobí |
| **Evidence (dôkazy)** | čím to podkladám (nepovinné) |

👉 bez tejto štruktúry argument nemá plnú váhu.

---

### ⚔️ 3.4 Povinná existencia protiargumentu

Každý významný návrh musí mať:

👉 **counter-argument**

To znamená:

* systém aktívne generuje opozíciu
* alebo ju vyžaduje

👉 cieľ: vyvážiť bias

---

### 🧠 3.5 Oddelenie osoby od argumentu

Systém NESMIE:

* zvýhodňovať autora argumentu
* zvýhodňovať popularitu

👉 hodnotí sa **argument, nie človek**

> Hodnotenie argumentu je úplne oddelené od identity autora. Tento princíp je základom pre elimináciu mocenského biasu (ADR-038).

---

### 🧠 3.6 Argument graph

Argumenty tvoria:

👉 **graph (sieť vzťahov)**

Obsahuje:

* podporujúce argumenty
* protiargumenty
* alternatívne pohľady

👉 projekt nie je lineárna diskusia, ale **mapa argumentov**

> Argument graph nie je len vizualizácia diskusie, ale primárna štruktúra, z ktorej sa odvodzuje vývoj projektu.

---

### ⚖️ 3.7 Hodnotenie argumentov (Rozšírené v v3)

Argument sa hodnotí podľa:

| Kritérium | Popis | Hodnotí |
|-----------|-------|---------|
| **Koherencia** | je logicky konzistentný | AI + užívatelia |
| **Pokrytie** | zohľadňuje viac aspektov | AI + užívatelia |
| **Robustnosť** | odoláva protiargumentom | AI (analýza grafu) |
| **Praktické dôsledky** | je aplikovateľný | užívatelia |
| **Relevantnosť** | týka sa priamo témy | AI + užívatelia |
| **Podloženosť faktami** | má evidenciu alebo dáta | AI + užívatelia |

👉 NIE podľa počtu hlasov.

> Hodnotenie slúži na orientáciu v kvalite argumentov, nie na vytváranie hierarchie medzi používateľmi.

---

### 🤖 3.16 Metriky kvality argumentu (NOVÉ v v3)

Systém používa **kombinované hodnotenie kvality** z dvoch zdrojov:

#### A. Automatické hodnotenie (lokálna AI)

Personal AI používateľa **lokálne** vyhodnocuje každý argument podľa:

| Metrika | Popis | Spôsob výpočtu |
|---------|-------|----------------|
| **Logická konzistencia** | argument si neprotirečí | analýza textu (nerozporuje si claim a reasoning) |
| **Relevantnosť k téme** | argument sa týka diskutovanej témy | embedding podobnosť s topicom projektu |
| **Faktická podloženosť** | argument obsahuje overiteľné tvrdenia | detekcia faktických tvrdení (bez overenia pravdy) |
| **Štrukturálna úplnosť** | argument má všetky časti (claim, reasoning, assumptions) | kontrola štruktúry |
| **Jasnosť** | argument je zrozumiteľný | analýza zložitosti viet, nejednoznačností |

**Dôležité:** Automatické hodnotenie je **lokálne** – každý používateľ má svoju AI, ktorá hodnotí podľa jeho vlastných kritérií. Neexistuje centrálna „pravda“ o kvalite argumentu.

#### B. Užívateľské hodnotenie (nové – sekcia 3.17)

Používatelia môžu argumenty hodnotiť manuálne. Tieto hodnotenia sú tiež lokálne a synchronizujú sa len v rámci Trusted Circles (ADR-004).

#### C. Kombinované skóre kvality

Pre každého používateľa existuje **lokálne kombinované skóre** argumentu:

```
quality_score = w1 * AI_logical_consistency +
                w2 * AI_relevance +
                w3 * AI_factual_basis +
                w4 * AI_structural_completeness +
                w5 * AI_clarity +
                w6 * user_rating_aggregate
```

Váhy `w1` až `w6` sú súčasťou **Preference Modelu** používateľa (ADR-034) – každý si môže nastaviť, čo je preňho dôležité.

---

### 👥 3.17 Užívateľské hodnotenie argumentov (NOVÉ v v3)

Používatelia môžu k argumentu pridať **štruktúrovanú spätnú väzbu**:

| Typ hodnotenia | Význam | Vplyv na kvalitu |
|----------------|--------|------------------|
| `agree` | súhlasím s argumentom | + (pozitívny signál) |
| `disagree` | nesúhlasím | - (negatívny signál, ale neznižuje kvalitu – len alternatívny pohľad) |
| `clarify` | argument je nejasný, potrebuje vysvetlenie | 0 (žiadny vplyv, ale upozornenie) |
| `off-topic` | argument nesúvisí s témou | -- (znižuje relevance skóre) |
| `insightful` | argument je mimoriadne hodnotný, otvára nový pohľad | ++ (výrazne zvyšuje kvalitu) |
| `evidence_request` | žiadam o dôkazy / podklady | 0 (signal, že chýba faktická podloženosť) |

**Dôležité pravidlá:**

1. **Hodnotenie je oddelené od identity autora** – hodnotí sa argument, nie kto ho napísal.
2. **Hodnotenie nie je hlasovanie** – `agree` neznamená „hlasujem za“, ale „považujem to za kvalitný argument“.
3. **Každý používateľ má vlastné hodnotenia** – nie sú globálne.
4. **Hodnotenia sa synchronizujú len v rámci Trusted Circles** – žiadne globálne skóre.

---

### 🎯 3.18 Oddelenie kvality od popularity (NOVÉ v v3)

**Princíp:**

> **Populárny argument nemusí byť kvalitný. Kvalitný argument nemusí byť populárny. Systém musí rozlišovať tieto dve dimenzie.**

Systém sleduje dve **nezávislé metriky** pre každý argument:

| Metrika | Popis | Z čoho sa počíta |
|---------|-------|------------------|
| **Quality Score** | aká je kvalita argumentu | AI metriky + užívateľské `insightful` / `clarify` / `off-topic` |
| **Popularity Score** | koľko ľudí s argumentom súhlasí | počet `agree` (bez váhy) |

**Prečo je to dôležité:**

* Kvalitný, ale nepopulárny argument môže byť prelomový.
* Populárny, ale nekvalitný argument môže byť manipulácia davom.
* Používateľ musí vidieť obe dimenzie, nie len jednu.

**Použitie v View Layer (ADR-027):**

Používateľ si môže vybrať, ako chce argumenty zoradiť:

* podľa kvality (default pre konceptuálne projekty)
* podľa popularity (voliteľné)
* podľa kombinácie (vlastné váhy)

**Prepojenie na ADR-003 (Non-Manipulation):**

Systém **NESMIE**:

* predvolene zoraďovať len podľa popularity
* skrývať kvalitné, ale nepopulárne argumenty
* prezentovať popularitu ako indikátor kvality

Systém **MÔŽE**:

* zvýrazniť kvalitné argumenty (bez ohľadu na popularitu)
* umožniť používateľovi vidieť „skryté drahokamy“ (vysoká kvalita, nízka popularita)

---

### 🧠 3.8 Úloha AI

AI:

* extrahuje argumenty z textu
* štrukturuje ich
* odstraňuje emócie
* generuje protiargumenty
* sumarizuje diskusiu
* **hodnotí logickú konzistenciu (nové v v3)**
* **hodnotí relevantnosť a faktickú podloženosť (nové v v3)**

AI NESMIE:

* rozhodovať
* určovať „pravdu“
* **skrývať kvalitné argumenty kvôli nízkej popularite (nové v v3)**

---

### 🔥 3.9 Argument → Impact Model

Argumenty nemajú len existovať.

👉 musia mať **dopad na realitu projektu**

**Princíp:**

> **Argument ovplyvňuje vývoj projektu iba nepriamo – cez štruktúru, nie cez autoritu.**

**To znamená:**

Argument môže:

* posilniť vetvu (branch)
* oslabiť vetvu
* vytvoriť novú vetvu
* zmeniť smer diskusie

❌ **nikdy:**

* nepridáva „váhu hlasu“
* nepridáva autoritu používateľovi
* neovplyvňuje systém cez identitu autora
* **nepridáva váhu na základe popularity (nové v v3)**

👉 **Kvalita argumentu ovplyvňuje jeho impact, nie popularita.**

---

### 🧠 3.10 Vzťah k vetvám (Branching)

Každá vetva projektu:

* je podložená konkrétnymi argumentmi
* obsahuje vlastný argument graph

👉 vetva prežíva, ak:

* má robustné argumenty (vysoká kvalita)
* prechádza protiargumentmi

👉 vetva zaniká (ADR-029), ak:

* nemá podporu
* alebo jej argumenty neobstoja (nízka kvalita)

**Prepojenie na ADR-028 (Support & Alignment):**

Kvalita argumentov ovplyvňuje **alignment** – používateľ, ktorý vidí kvalitné argumenty pre vetvu A a nekvalitné pre vetvu B, prirodzene zmení svoj support.

Nie je to automatické – ale systém ukazuje súvislosť medzi kvalitou argumentov a podporou.

---

### 🧠 3.11 Argument ≠ rozhodnutie

Systém:

❌ nikdy nevyberá „víťazný argument“

👉 poskytuje:

* mapu možností
* dôsledky
* konflikty
* **kvalitu jednotlivých argumentov (nové v v3)**

👉 rozhoduje **človek** (ADR-002)

---

### 🧠 3.12 Ochrana pred mocou (prepojenie na ADR-038)

Argumentation Layer je navrhnutá tak, aby:

* minimalizovala vplyv silných osobností
* zabránila dominancii hlasných používateľov
* oddelila kvalitu argumentu od jeho prezentácie
* **zabránila dominancii populárnych, ale nekvalitných argumentov (nové v v3)**

> Argument môže získať vplyv len tým, že obstojí v konfrontácii s inými argumentmi – nie tým, kto ho vyslovil alebo koľko ľudí s ním súhlasí.

---

### 🔄 3.13 Výstup projektu

Výsledok je:

👉 **structured reasoning output + branch landscape + quality assessment**

Obsahuje:

* hlavné vetvy
* argumenty pre každú vetvu (s kvalitou)
* protiargumenty (s kvalitou)
* dôsledky
* konflikty medzi vetvami
* **argumenty s najvyššou kvalitou (aj keď nepopulárne)**

---

### 🧠 3.14 Prepojenie na View Layer

Používateľ vidí:

* zjednodušené argumenty
* hlavné konflikty
* kľúčové rozhodovacie body
* **kvalitu argumentov (zobrazenú oddelene od popularity)**
* **možnosť prepínať medzi „podľa kvality“ a „podľa popularity“**

👉 nie celý graph.

---

### 🧠 3.15 Governance napojenie

Argumentation Layer:

* dopĺňa Support Model (ADR-028)
* dopĺňa Branching (ADR-024)
* je základom pre Power balancing (ADR-038)
* **poskytuje vstup pre alignment cez kvalitu argumentov (nové v v3)**

> Používa sa hlavne pre conceptual / normative projekty.

---

## 4. 🧠 Dôsledky rozhodnutia

### ✅ Pozitívne

| Oblasť | Dôsledok |
|--------|----------|
| **Kvalitnejšie myslenie** | menej emócií, viac logiky |
| **Ochrana pred manipuláciou** | argument > ego, kvalita > popularita |
| **Škálovateľnosť diskusie** | veľké témy zvládnuteľné |
| **Vyváženie moci** | argument > autor, vplyv nevzniká z aktivity, ale z kvality reasoning |
| **Ochrana pred davom** | populárny ≠ dobrý, systém to nezamlčuje |
| **Transparentnosť hodnotenia** | používateľ vidí, prečo je argument považovaný za kvalitný |
| **Personalizácia kvality** | každý používateľ má vlastné váhy pre metriky kvality |

### ❗ Negatívne / Trade-offs

| Oblasť | Dôsledok |
|--------|----------|
| **Komplexita** | vyšší vstupný náklad |
| **Kognitívna náročnosť** | vyžaduje dobrý UX (ADR-027) |
| **Výpočtová komplexita** | argument graph môže rásť exponenciálne, plus metriky kvality |
| **Riziko „falošnej objektivity“** | AI metriky nie sú dokonalé, môžu byť biasované |
| **Lokálna výpočtová náročnosť** | hodnotenie kvality beží na zariadení používateľa |

---

## 5. 🚫 Alternatívy (zamietnuté)

### ❌ Voľná diskusia (komentáre)

* jednoduchá
* ale: chaotická, manipulovateľná

### ❌ Hlasovanie

* intuitívne
* ale: skreslené, náchylné na dav

### ❌ Čistá AI evaluácia bez užívateľského vstupu

* konzistentná
* ale: AI môže mať vlastný bias, ignoruje kontext

### ❌ Iba popularita bez kvality

* jednoduché
* ale: manipulovateľné davom, porušuje ADR-003

---

## 6. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-002 | Human-in-the-loop – rozhoduje človek, nie systém |
| ADR-003 | Non-Manipulation – systém nesmie uprednostňovať populárne pred kvalitnými |
| ADR-004 | Privacy-first – hodnotenia sú lokálne, synchronizujú sa len v Trusted Circles |
| ADR-011 | Personalization – váhy kvality sú súčasťou Preference Modelu |
| ADR-015 | Participation Filter – filtruje relevantných účastníkov |
| ADR-024 | Branching – vetvy projektu sú podložené argumentmi |
| ADR-027 | View Layer – zjednodušený pohľad pre používateľa, prepínanie kvalita/popularita |
| ADR-028 | Support Model – kvalita argumentov ovplyvňuje alignment |
| ADR-029 | Branch Lifecycle – vetvy zanikajú na základe kvality argumentov |
| ADR-030 | Sync & Persistence – argument graph a hodnotenia musia byť perzistentné |
| ADR-034 | Personal AI Internal Model – váhy pre metriky kvality sú v Preference Modeli |
| ADR-038 | Power & Influence Balancing – argumentation layer je nástrojom na elimináciu moci |
| ADR-039 | Truth & Epistemic Model – kvalita argumentov je vstupom pre epistemické hodnotenie |
| ADR-040 | Graceful Degradation – nízka kvalita vstupov vedie k nižšej kvalite hodnotení |
| **ADR-042** | **Unified Entity Model – Argument je typ Node** |

---

## 7. 📏 Pravidlá implementácie (high-level)

Systém musí:

* umožniť štruktúrované argumenty
* podporovať protiargumenty
* vytvárať argument graph
* oddeliť autora od obsahu
* prepojiť argumenty na vetvy projektu
* zabezpečiť, že argument neovplyvňuje systém cez identitu autora
* prepojiť s AI summarization
* **automaticky hodnotiť logickú konzistenciu, relevantnosť a podloženosť (lokálne)**
* **umožniť užívateľské hodnotenie (agree, disagree, clarify, off-topic, insightful, evidence_request)**
* **oddeliť quality score od popularity score**
* **zobraziť obe dimenzie v View Layer**
* **nikdy neskrývať kvalitné argumenty kvôli nízkej popularite**
* **prepojiť kvalitu argumentov s alignmentom (ADR-028)**

---

## 8. 🔥 Príklady

### Príklad 1: Argument s hodnotením kvality

```json
{
  "argument_id": "arg_001",
  "claim": "Pizzéria by mala používať lokálne suroviny",
  "reasoning": "Lokálne suroviny znižujú uhlíkovú stopu a podporujú miestnych farmárov",
  "assumptions": ["Lokálni farmári dokážu pokryť dopyt", "Zákazníci ocenia ekologický prístup"],
  "implications": ["Vyššie náklady na suroviny", "Lepšia marketingová pozícia"],
  "evidence": ["Štúdia X ukazuje, že 70% zákazníkov uprednostňuje lokálne suroviny"],
  
  "quality_metrics": {
    "logical_consistency": 0.92,
    "relevance": 0.95,
    "factual_basis": 0.78,
    "structural_completeness": 1.0,
    "clarity": 0.88
  },
  
  "user_ratings": {
    "agree": 45,
    "disagree": 12,
    "clarify": 3,
    "off_topic": 1,
    "insightful": 8,
    "evidence_request": 2
  },
  
  "quality_score": 0.87,
  "popularity_score": 0.79
}
```

### Príklad 2: View Layer – zobrazenie argumentov

```
--- Argumenty pre vetvu "Lokálne suroviny" ---

Zoradené podľa: [Kvality ▼]  [Popularity]  [Kombinované]

1. 🧠 Q=0.92 | P=0.45
   "Pizzéria by mala používať lokálne suroviny"
   ✅ Logicky konzistentné | 📊 Podložené štúdiou
   [8 people found this insightful]

2. 🧠 Q=0.88 | P=0.82
   "Lokálne suroviny sú príliš drahé"
   ⚠️ Vyžaduje evidenciu | 2 ľudia žiadajú podklady

3. 🧠 Q=0.45 | P=0.91
   "Moja sesternica má pizzériu a nepoužíva lokálne"
   ❌ Nízka relevantnosť | Anecdotálne, nie všeobecné

--- Skryté drahokamy (vysoká kvalita, nízka popularita) ---
👉 "Lokálne suroviny vytvárajú komunitný pocit" (Q=0.89, P=0.12)
```

### Príklad 3: Lokálne nastavenie váh kvality

```json
{
  "user_id": "user_miro_001",
  "preferences": {
    "argument_quality_weights": {
      "logical_consistency": 0.35,
      "relevance": 0.25,
      "factual_basis": 0.30,
      "structural_completeness": 0.05,
      "clarity": 0.05
    },
    "default_sorting": "quality",
    "show_unpopular_quality_arguments": true
  }
}
```

---

## 9. 🔥 Zhrnutie

> **Argumenty nie sú len diskusia – sú mechanizmus, ktorým sa mení štruktúra projektu bez toho, aby vznikala mocenská hierarchia.**

Konceptuálne projekty sú riadené **kvalitou argumentov**, nie hlasovaním ani silou osobností.

Argument môže posilniť, oslabiť alebo vytvoriť vetvu – ale nikdy nepridáva váhu hlasu svojmu autorovi.

**V3 prináša:**

* **Metriky kvality** – logická konzistencia, relevantnosť, podloženosť
* **Užívateľské hodnotenie** – agree, disagree, clarify, off-topic, insightful
* **Oddelenie kvality od popularity** – populárny ≠ kvalitný
* **Prepojenie na Support & Alignment** – kvalita ovplyvňuje, ako používatelia menia svoju podporu

---

## 10. 🧭 Finálna veta

> **V Synergetiku nemá vplyv ten, kto hovorí – ale to, čo prežije konfrontáciu s inými myšlienkami. A ani popularita nerobí argument pravdou – len kvalita rozhoduje o jeho sile.**

V komplexných problémoch nevyhráva ten, kto kričí najhlasnejšie – ale ten, koho argument prežije najviac otázok. A nevyhráva ani ten, koho argument má najviac lajkov.

---

**Dátum:** 2026-04-15  
**Stav:** Accepted  
**Typ:** Core Mechanism ADR (Conceptual Projects)  
**Verzia:** 3.0 (Hardened with Quality Metrics)

---

## 📝 Zoznam zmien oproti Version 2

| Sekcia | Zmena |
|--------|-------|
| 1. Status | Zmenené na v3 |
| 2. Kontext | Doplnený dodatočný problém (popularity bias, kvalita) |
| 3.1 | Doplnené hodnotenie kvality a oddelenie od popularity |
| 3.3 | Doplnené `Evidence` do štruktúry argumentu |
| 3.7 | Rozšírené kritériá hodnotenia (relevantnosť, podloženosť) |
| 3.16 | **Nová sekcia:** Metriky kvality argumentu |
| 3.17 | **Nová sekcia:** Užívateľské hodnotenie argumentov |
| 3.18 | **Nová sekcia:** Oddelenie kvality od popularity |
| 3.8 | Rozšírené úlohy AI o hodnotenie kvality |
| 3.9 | Doplnené, že impact závisí od kvality, nie popularity |
| 3.10 | Prepojenie na ADR-028 (Support & Alignment) |
| 3.14 | Rozšírený View Layer o prepínanie kvalita/popularita |
| 4. Pozitíva | Doplnené ochrana pred davom, transparentnosť, personalizácia |
| 4. Negatíva | Doplnené riziko falošnej objektivity, lokálna výpočtová náročnosť |
| 5. Alternatívy | Doplnená alternatíva „iba popularita“ |
| 6. Väzby | Doplnené ADR-003, ADR-028, ADR-042 |
| 7. Implementácia | Doplnené pravidlá pre metriky kvality a oddelenie od popularity |
| 8. | **Nová sekcia:** Príklady (argument s hodnotením, view layer, nastavenie váh) |
| 9. Zhrnutie | Doplnené v3 princípy |
| 10. Finálna veta | Rozšírená o popularitu |