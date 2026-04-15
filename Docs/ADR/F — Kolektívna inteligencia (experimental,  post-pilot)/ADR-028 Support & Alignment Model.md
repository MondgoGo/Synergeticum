# 🧾 ADR-028: Support & Alignment Model (v2)

> **Zmeny oproti Version 1:**
> - **Doplnená časť 3.11:** Vplyv kvality argumentov na alignment (prepojenie na INSPIRATION-10 a ADR-025)
> - **Doplnená časť 3.12:** Quality-weighted alignment – výpočet váženého alignmentu
> - **Doplnená časť 3.13:** Vplyv rôznych typov hodnotení na alignment
> - **Rozšírená časť 3.5:** Profil podpory variantu o quality-weighted alignment
> - **Rozšírená časť 3.10:** Personalizované vnímanie o zohľadnenie vlastných váh kvality
> - **Rozšírená časť 6:** Väzby na ADR-025, ADR-034
> - **Nová sekcia 8.4:** Príklad pre quality-weighted alignment

---

## 1. 📌 Status

**Accepted (v2 – with Quality-Weighted Alignment)**

---

## 2. 🎯 Kontext

Systém Personal AI Network:

* obsahuje projekty s viacerými variantmi (ADR-024)
* prezentuje ich cez View Layer (ADR-027)
* umožňuje používateľom reagovať a zapájať sa
* **obsahuje argumentačnú vrstvu s hodnotením kvality argumentov (ADR-025 v3/v4)**

---

Každý variant projektu:

* má podporovateľov
* má kritikov
* má rôznu mieru zapojenia
* **je podložený argumentmi s rôznou kvalitou (ADR-025)**

---

### Problém

Je potrebné definovať:

👉 **ako reprezentovať podporu jednotlivých variantov**

tak, aby:

* nedošlo k redukcii na jednoduché hlasovanie
* nedochádzalo k manipulácii
* bol zachovaný kontext a kvalita rozhodovania
* **kvalita argumentov ovplyvňovala váhu podpory (v2)**

---

### Kľúčová otázka

👉
**Ako vyjadriť podporu variantu bez degradácie systému na „like/dislike“ model – a zároveň zohľadniť kvalitu argumentov, ktoré variant podporujú?**

---

## 3. ⚖️ Rozhodnutie

---

## 🧠 3.1 Zavedenie „Support Model“

Systém zavádza:

👉 **multi-dimenzionálny model podpory**

**V2 rozšírenie:** Podpora je **vážená kvalitou argumentov**, ktoré daný variant podporujú.

---

## 📊 3.2 Dimenzie podpory

Každý používateľ môže vyjadriť voči variantu:

---

### 🔹 Alignment (súlad)

👉 „tento smer mi dáva zmysel“

---

### 🔹 Commitment (záväzok)

👉 „som ochotný sa zapojiť“

---

### 🔹 Confidence (istota)

👉 „ako veľmi tomu verím“

---

👉 tieto dimenzie sú nezávislé

---

## ⚖️ 3.3 Váhovanie podpory

---

Podpora variantu NIE JE:

❌ počet hlasov

👉 ale:

👉 kombinácia:

* alignment
* commitment
* confidence
* (voliteľne) expertíza
* **kvalita argumentov podporujúcich variant (v2)**

---

## 🧠 3.4 Rozdiel medzi názorom a akciou

---

👉 systém explicitne rozlišuje:

---

### 🟢 Názor

* alignment
* nízka váha

---

### 🔴 Akcia

* commitment
* vysoká váha

---

👉
**akcia má vyššiu váhu než názor**

---

## 🧠 3.5 Profil podpory variantu (Rozšírené v v2)

---

Každý variant obsahuje:

---

### 📊 Alignment score

koľko ľudí s ním súhlasí

---

### 🤝 Commitment score

koľko ľudí sa zapája

---

### 🎯 Confidence score

ako silno tomu veria

---

### 🧠 Expertise signal (voliteľné)

aká je kvalita zdrojov

---

### ⭐ Quality-weighted alignment (NOVÉ v v2)

alignment vážený kvalitou argumentov podporujúcich variant

---

👉 vzniká:

**profil podpory, nie výsledok hlasovania**

---

## ⚖️ 3.6 Žiadne priame hlasovanie

---

Systém NESMIE:

* používať jednoduché hlasovanie
* určovať víťaza na základe počtu hlasov

👉 cieľ nie je „vyhrať“

---

## 🧠 3.7 Zobrazenie používateľovi

---

Používateľ vidí:

---

* ktoré varianty sú populárne
* ktoré majú reálnu podporu
* ktoré majú zdroje
* **ktoré varianty majú vysokú kvalitu argumentov (v2)**

👉 nie:

* kto vyhráva

---

## 🔄 3.8 Dynamika v čase

---

Podpora:

* sa mení
* reaguje na nové informácie
* nie je fixná
* **reaguje na nové argumenty a ich kvalitu (v2)**

---

## 🚫 3.9 Ochrana proti manipulácii

---

Systém musí:

* ignorovať spam
* znižovať váhu neaktívnych účtov
* oddeľovať hlasitosť od kvality
* **zabraňovať tomu, aby populárne, ale nekvalitné argumenty deformovali alignment (v2)**

---

## 🔍 3.10 Personalizované vnímanie (Rozšírené v v2)

---

Personal AI používateľa:

* interpretuje podporu
* zvýrazní relevantné signály
* filtruje šum
* **používa vlastné váhy kvality (z Preference Modelu) pri výpočte alignmentu (v2)**

---

## ⭐ 3.11 Vplyv kvality argumentov na alignment (NOVÉ v v2)

**Princíp:**

> **Alignment nie je len o počte ľudí, ktorí podporujú vetvu. Je to o kvalite argumentov, ktoré túto podporu odôvodňujú.**

### A. Prečo je to dôležité

| Situácia | Bez váženia kvalitou | S vážením kvalitou |
|----------|---------------------|-------------------|
| 100 ľudí podporuje vetvu slabými argumentmi | Vysoký alignment | Nízky alignment |
| 10 expertov podporuje vetvu silnými argumentmi | Nízky alignment | Vysoký alignment |
| Populárny, ale nekvalitný argument | Deformuje alignment | Nemá vplyv |
| Kvalitný, ale nepopulárny argument | Ignorovaný | Zohľadnený |

### B. Faktory ovplyvňujúce alignment

| Faktor | Vplyv na alignment | Zdroj |
|--------|-------------------|-------|
| **Quality score** argumentov pre vetvu | Vyššia kvalita → vyššia váha alignment signálu | ADR-025 |
| **Popularity score** argumentov | Nižší vplyv (oddelené od kvality) | ADR-025 |
| **Insightful ratings** | Výrazne zvyšuje váhu argumentu | ADR-025 |
| **Off-topic ratings** | Znižuje váhu argumentu | ADR-025 |
| **Evidence presence** | Mierne zvyšuje váhu | ADR-025 |

### C. Kvalita ako súčasť alignment signálu

Keď používateľ vyjadrí alignment s vetvou, systém zohľadňuje:

1. **Jeho vlastný alignment** (áno/nie)
2. **Kvalitu argumentov, ktoré čítal** pred vyjadrením alignmentu
3. **Kvalitu argumentov, ktoré vetvu podporujú**

```json
{
  "user_id": "user_miro_001",
  "branch_id": "branch_crowdfunding",
  "alignment": 0.8,
  "context": {
    "arguments_read_before": ["arg_cf_001", "arg_cf_002"],
    "avg_quality_of_read_arguments": 0.78,
    "avg_quality_of_branch_arguments": 0.85
  },
  "weighted_alignment_contribution": 0.8 * 0.85  // = 0.68
}
```

---

## 📊 3.12 Quality-weighted alignment – výpočet (NOVÉ v v2)

**Princíp:**

> **Alignment vetvy je vážený priemernou kvalitou argumentov, ktoré túto vetvu podporujú.**

### A. Základný vzorec

```
branch_alignment_weighted = 
    Σ (user_alignment * avg_argument_quality_for_branch) / Σ avg_argument_quality_for_branch
```

Kde:
- `user_alignment` je alignment konkrétneho používateľa (0-1)
- `avg_argument_quality_for_branch` je priemerná kvalita argumentov podporujúcich danú vetvu (podľa preferencií používateľa)

### B. Rozšírený vzorec (s commitment a confidence)

```
branch_support_score = 
    w1 * alignment_weighted +
    w2 * commitment_score +
    w3 * confidence_score +
    w4 * expertise_signal
```

Kde `w1` až `w4` sú konfigurovateľné váhy (default: w1=0.4, w2=0.3, w3=0.2, w4=0.1)

### C. Príklad výpočtu

| Vetva | Alignment (počet) | Priemerná kvalita argumentov | Vážený alignment | Commitment | Confidence | Celkové skóre |
|-------|------------------|------------------------------|------------------|------------|------------|---------------|
| Úver | 34 ľudí | 0.72 | 24.5 | 12 | 0.65 | 0.58 |
| Crowdfunding | 67 ľudí | 0.85 | 57.0 | 28 | 0.78 | 0.76 |
| Investor | 23 ľudí | 0.68 | 15.6 | 5 | 0.55 | 0.42 |

> Napriek tomu, že Crowdfunding má najviac podporovateľov (67), jeho výhoda je ešte výraznejšia po zvážení kvalitou (57.0 vs 24.5).

---

## 🏷️ 3.13 Vplyv rôznych typov hodnotení na alignment (NOVÉ v v2)

Rôzne typy užívateľských hodnotení (ADR-025) majú rôzny vplyv na alignment:

| Typ hodnotenia | Vplyv na alignment vetvy | Vysvetlenie |
|----------------|-------------------------|-------------|
| `agree` | + (základný) | Súhlas s argumentom podporujúcim vetvu |
| `disagree` | - (žiadny priamy) | Nesúhlas s argumentom, ale nemusí znamenať nesúhlas s vetvou |
| `insightful` | ++ (výrazný) | Argument je mimoriadne hodnotný – zvyšuje váhu vetvy |
| `off-topic` | -- (výrazné zníženie) | Argument nesúvisí s témou – znižuje váhu vetvy |
| `evidence_request` | 0 (žiadny) | Chýbajúce dôkazy – signal, ale nie priamy vplyv |
| `clarify` | 0 (žiadny) | Nejaskosť – signal pre vylepšenie |

**Príklad vplyvu:**

```json
{
  "branch_id": "branch_crowdfunding",
  "arguments": [
    {
      "id": "arg_cf_001",
      "quality": 0.91,
      "user_ratings": { "insightful": 8, "agree": 45 },
      "weight_multiplier": 1.3  // insightful zvyšuje váhu
    },
    {
      "id": "arg_cf_002",
      "quality": 0.65,
      "user_ratings": { "off_topic": 3 },
      "weight_multiplier": 0.5  // off-topic znižuje váhu
    }
  ],
  "effective_branch_quality": 0.82  // vážený priemer s multipliermi
}
```

---

## 🔗 3.14 Vzťah k personalizácii

Každý používateľ môže mať **vlastné váhy** pre výpočet alignmentu:

```json
{
  "user_id": "user_miro_001",
  "preferences": {
    "alignment_weights": {
      "quality_weight": 0.5,
      "commitment_weight": 0.3,
      "confidence_weight": 0.2,
      "expertise_weight": 0.1
    },
    "min_argument_quality_to_consider": 0.6  // ignoruje argumenty pod touto hranicou
  }
}
```

---

## 4. 🧠 Dôsledky rozhodnutia

---

## ✅ Pozitíva

| Oblasť | Dôsledok |
|--------|----------|
| **Odolnosť voči manipulácii** | nie je možné „prehlasovať“ systém |
| **Kvalitnejšie rozhodovanie** | viac dimenzií + kvalita argumentov |
| **Prepojenie na realitu** | zapojenie = reálny signál |
| **Ochrana pred davom (v2)** | populárna vetva s nekvalitnými argumentmi nemá automaticky vysoký alignment |
| **Zvýraznenie kvality (v2)** | kvalitné, ale nepopulárne argumenty ovplyvňujú alignment |
| **Personalizácia (v2)** | každý používateľ vidí alignment podľa vlastných preferencií |

---

## ❗ Negatíva / Trade-offs

| Oblasť | Dôsledok |
|--------|----------|
| **Komplexita** | ťažšie pochopiteľné |
| **Menej intuitívne než hlasovanie** | treba UI vysvetlenie |
| **Výpočtová náročnosť (v2)** | výpočet váženého alignmentu vyžaduje spracovanie kvality argumentov |
| **Riziko prepočítania (v2)** | nesprávne nastavené váhy môžu skresliť výsledok |

---

## 5. 🚫 Alternatívy (zamietnuté)

---

### ❌ Like / dislike systém

* jednoduchý
* ale manipulovateľný

---

### ❌ Jednoduché hlasovanie

* demokratické
* ale skreslené

---

### ❌ Iba alignment bez váženia kvalitou (v2)

* jednoduchšie
* ale ignoruje kvalitu argumentov, podporuje populizmus

---

### ❌ Plne automatický alignment bez užívateľského vstupu (v2)

* konzistentné
* ale ignoruje ľudský úsudok, porušuje ADR-002

---

## 6. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-002 | Human-in-the-loop – alignment je ľudský, nie automatický |
| ADR-003 | Non-Manipulation – kvalita > popularita |
| ADR-015 | Participation Filter – kto môže vyjadriť alignment |
| ADR-019 | Opportunity Engine – alignment vstupom pre príležitosti |
| ADR-024 | Multi-variant Governance – vetvy sú hodnotené |
| **ADR-025** | **Argumentation – quality score, insightful, off-topic (v2)** |
| ADR-027 | View Layer – zobrazenie alignmentu |
| **ADR-034** | **Personal AI Internal Model – preferencie pre váhy alignmentu (v2)** |
| ADR-038 | Power & Influence – alignment nesmie vytvárať mocenskú hierarchiu |

---

## 7. 📏 Pravidlá implementácie (high-level)

Systém musí:

* podporovať viac dimenzií podpory
* oddeliť názor od záväzku
* zabrániť hlasovaciemu modelu
* zobrazovať profil podpory
* **zohľadňovať kvalitu argumentov pri výpočte alignmentu (v2)**
* **používať quality-weighted alignment pre vetvy (v2)**
* **umožniť personalizáciu váh alignmentu (v2)**
* **zabraňovať tomu, aby populárne, ale nekvalitné argumenty deformovali alignment (v2)**

---

## 8. 🔥 Príklady

### Príklad 1: Vyjadrenie podpory používateľom

```json
{
  "user_id": "user_miro_001",
  "branch_id": "branch_crowdfunding",
  "support": {
    "alignment": 0.8,
    "commitment": 0.6,
    "confidence": 0.7,
    "expertise": 0.4
  },
  "context": {
    "arguments_read": ["arg_cf_001", "arg_cf_002"],
    "avg_quality_of_read_arguments": 0.78
  }
}
```

### Príklad 2: Profil podpory variantu (pôvodný)

```json
{
  "branch_id": "branch_crowdfunding",
  "support_profile": {
    "alignment_count": 67,
    "commitment_count": 28,
    "average_confidence": 0.78,
    "alignment_score": 0.65,
    "commitment_score": 0.42
  }
}
```

### Príklad 3: Quality-weighted alignment (NOVÉ v v2)

```json
{
  "branch_id": "branch_crowdfunding",
  "support_profile": {
    "raw_alignment_count": 67,
    "raw_alignment_score": 0.65,
    "avg_argument_quality": 0.85,
    "quality_weighted_alignment": 0.76,
    "quality_weighted_alignment_score": 0.72,
    "calculation": {
      "formula": "Σ(user_alignment * arg_quality) / Σ(arg_quality)",
      "total_weighted": 57.0,
      "total_quality": 75.0,
      "result": 0.76
    }
  }
}
```

### Príklad 4: Porovnanie variantov s quality-weighting (NOVÉ v v2)

```json
{
  "project_id": "proj_pizzeria_001",
  "branches_comparison": {
    "branch_loan": {
      "raw_alignment": 0.55,
      "avg_argument_quality": 0.72,
      "quality_weighted_alignment": 0.58,
      "commitment_count": 12,
      "final_score": 0.58
    },
    "branch_crowdfunding": {
      "raw_alignment": 0.65,
      "avg_argument_quality": 0.85,
      "quality_weighted_alignment": 0.76,
      "commitment_count": 28,
      "final_score": 0.76,
      "highlighted": true
    },
    "branch_investor": {
      "raw_alignment": 0.40,
      "avg_argument_quality": 0.68,
      "quality_weighted_alignment": 0.42,
      "commitment_count": 5,
      "final_score": 0.42
    }
  },
  "insights": {
    "crowdfunding_advantage": "Vetva Crowdfunding má najvyšší alignment aj po zvážení kvalitou argumentov (0.76 vs 0.58 pre úver)",
    "hidden_strength": "Vetva Investor má nízky raw alignment, ale argumenty sú kvalitné (0.68) – potenciál pre rast"
  }
}
```

### Príklad 5: Personalizované nastavenie alignmentu (NOVÉ v v2)

```json
{
  "user_id": "user_miro_001",
  "alignment_preferences": {
    "quality_weight": 0.6,
    "commitment_weight": 0.2,
    "confidence_weight": 0.15,
    "expertise_weight": 0.05,
    "min_argument_quality": 0.6,
    "ignore_off_topic_arguments": true,
    "boost_insightful_arguments": true
  },
  "personalized_view": {
    "branch_crowdfunding": 0.82,  // vyššie kvôli vysokej váhe kvality
    "branch_loan": 0.52,
    "branch_investor": 0.38
  }
}
```

---

## 9. 🔥 Zhrnutie

> **Podpora variantu je reprezentovaná ako viacrozmerný profil (alignment, commitment, confidence), nie ako jednoduché hlasovanie.**

**V2 prináša:**

* **Quality-weighted alignment** – alignment je vážený kvalitou argumentov
* **Vplyv hodnotení** – insightful zvyšuje váhu, off-topic znižuje
* **Personalizácia** – každý používateľ môže mať vlastné váhy
* **Ochrana pred populizmom** – populárna vetva s nekvalitnými argumentmi nemá automaticky vysoký alignment

---

## 10. 🧭 Finálna veta

> **Nie všetky hlasy majú rovnakú váhu – a systém to musí vedieť bez toho, aby potlačil slobodu. Kvalitný argument má väčšiu váhu než tisíc lajkov. A každý používateľ vidí alignment podľa vlastných preferencií.**

---

**Dátum:** 2026-04-15  
**Stav:** Accepted  
**Typ:** Core Mechanism ADR (Support & Alignment)  
**Verzia:** 2.0 (with Quality-Weighted Alignment)

---

## 📝 Zoznam zmien oproti Version 1

| Sekcia | Zmena |
|--------|-------|
| 1. Status | Zmenené na v2 |
| 2. Kontext | Doplnené o argumentačnú vrstvu a kvalitu argumentov |
| 2. Problém | Doplnené o potrebu zohľadniť kvalitu argumentov |
| 2. Kľúčová otázka | Rozšírená o kvalitu argumentov |
| 3.1 | Doplnené o váženie kvalitou argumentov |
| 3.3 | Doplnené o kvalitu argumentov do váhovania |
| 3.5 | Doplnené Quality-weighted alignment do profilu podpory |
| 3.7 | Doplnené o zobrazenie kvality argumentov |
| 3.8 | Doplnené o reakciu na nové argumenty a ich kvalitu |
| 3.9 | Doplnené o ochranu pred populárnymi, ale nekvalitnými argumentmi |
| 3.10 | Rozšírené o vlastné váhy kvality z Preference Modelu |
| 3.11 | **Nová sekcia:** Vplyv kvality argumentov na alignment |
| 3.12 | **Nová sekcia:** Quality-weighted alignment – výpočet |
| 3.13 | **Nová sekcia:** Vplyv rôznych typov hodnotení na alignment |
| 3.14 | **Nová sekcia:** Vzťah k personalizácii |
| 4. Pozitíva | Doplnené ochrana pred davom, zvýraznenie kvality, personalizácia |
| 4. Negatíva | Doplnené výpočtová náročnosť, riziko prepočítania |
| 5. Alternatívy | Doplnené "iba alignment bez váženia kvalitou" a "plne automatický alignment" |
| 6. Väzby | Doplnené ADR-025, ADR-034 |
| 7. Implementácia | Doplnené pravidlá pre quality-weighted alignment, personalizáciu |
| 8.1 | Pôvodný príklad (vyjadrenie podpory) |
| 8.2 | Pôvodný príklad (profil podpory) |
| 8.3 | **Nová sekcia:** Príklad 3 – Quality-weighted alignment |
| 8.4 | **Nová sekcia:** Príklad 4 – Porovnanie variantov s quality-weighting |
| 8.5 | **Nová sekcia:** Príklad 5 – Personalizované nastavenie alignmentu |
| 9. Zhrnutie | Doplnené v2 princípy |
| 10. Finálna veta | Rozšírená o kvalitu argumentov a personalizáciu |