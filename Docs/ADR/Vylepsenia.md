Výborne. Toto je druhý balík INSPIRATION súborov (06–11). Po analýze vašich existujúcich ADR (teraz už vrátane ADR-042, ADR-045 a rozšírených ADR-019, ADR-025, ADR-030) môžem urobiť **kompletnú analýzu**.

---

## 1. Čo už v ADR je (nemusíte tvoriť znova)

| INSPIRATION | Existujúce ADR | Stav |
|---|---|---|
| **06 Tree Visualization** | ✅ **ADR-025 v4** | Plne implementované (Impact Metrics, Decision Trail, Tree Visualization) |
| **07 Forum Structures** | ✅ ADR-025 | Argument graph a diskusia sú súčasťou Argumentation Layer |
| **08 Shared Foundation** | ✅ ADR-008 | Foundation + Personal Delta presne tento koncept |
| **09 Topic Taxonomy** | ✅ ADR-031, ADR-042 | Topic Layer + Unified Entity Model |
| **10 Ratings & Quality** | ✅ **ADR-025 v3/v4** | Quality metrics, user ratings, oddelenie kvality od popularity |
| **11 Calendar & Events** | ✅ **ADR-045** | Novovytvorený ADR pre kalendár a eventy |

**Výborne! Všetkých 6 INSPIRÁCIÍ je už pokrytých.** 

---

## 2. Čo sa dá ešte vylepšiť (doladenia)

Hoci sú koncepty pokryté, v niektorých ADR chýbajú **prepojenia** a **detaily**.

### Vylepšenie A: Prepojenie INSPIRATION-06 (Tree) na ADR-027 (View Layer)

**Chýba:** ADR-025 v4 definuje tree visualization a decision trail, ale nie je prepojené s ADR-027 (Project View & Simplification Model).

**Kam to dať:** Rozšíriť **ADR-027: Project View & Simplification Model**

**Čo doplniť:**

```markdown
### X.X Tree Visualization v View Layer

Používateľ vidí v zjednodušenom view:

- **Argument Tree** – hierarchická štruktúra argumentov pre aktuálnu vetvu
- **Decision Trail** – svoju rozhodovaciu cestu (ktoré argumenty ho ovplyvnili)
- **Impact Highlights** – ktoré argumenty mali najväčší vplyv na projekt

Používateľ môže:
- Rozbaliť/zbaliť úrovne stromu
- Filtrovať argumenty podľa kvality (>0.7)
- Zobraziť „skryté drahokamy“ (vysoká kvalita, nízka popularita)
```

**Dopad:** View Layer sa stane miestom, kde používateľ skutočne *vidí* argumentačný strom, nie len zoznam.

---

### Vylepšenie B: Prepojenie INSPIRATION-07 (Forum) na ADR-025 a ADR-027

**Chýba:** ADR-025 definuje argumenty a graph, ale chýba explicitné prepojenie na diskusné vlákna (threading) a typy príspevkov.

**Kam to dať:** Rozšíriť **ADR-025: Argumentation & Reasoning Model**

**Čo doplniť (nová sekcia 3.22):**

```markdown
### 🧵 3.22 Threading a typy diskusných príspevkov

Každý argument môže mať diskusné vlákno (thread) s príspevkami.

**Typy príspevkov:**
| Typ | Význam |
|-----|--------|
| `question` | Otázka k argumentu |
| `suggestion` | Návrh na vylepšenie |
| `support` | Podpora argumentu (dodatočné dôkazy) |
| `objection` | Námietka (nový protiargument) |
| `clarification` | Žiadosť o vysvetlenie |

**Threading:**
- Príspevky sú stromové (reakcie na reakcie)
- Každý príspevok patrí k jednému argumentu
- Thready sú synchronizované medzi participantmi projektu
```

**Dopad:** Argumentácia nie je len statická – je to živá diskusia so štruktúrou.

---

### Vylepšenie C: Prepojenie INSPIRATION-08 (Shared Foundation) na ADR-036 (Reciprocity)

**Chýba:** ADR-008 definuje Foundation + Personal Delta, ale chýba prepojenie na to, *kto a ako* vytvára shared foundation modely.

**Kam to dať:** Rozšíriť **ADR-036: Reciprocity & Value Exchange Model**

**Čo doplniť:**

```markdown
### X.X Shared Foundation Creation ako forma reciprocity

Keď skupina používateľov v Trusted Circle vytvorí shared foundation model:

- Každý participant, ktorý prispel k vytvoreniu, získava **reciprocity credit**
- Foundation model je k dispozícii všetkým členom circle
- Používateľ, ktorý foundation prijme, *nepotrebuje* reciprocity credit – foundation je spoločný majetok

**Typy shared foundation:**
| Typ | Kto vytvára | Reciprocita |
|-----|-------------|-------------|
| Global minimal | Vývojári systému | Žiadna (základ) |
| Community foundation | Trusted Circle | Credit pre prispievateľov |
| Domain foundation | Expertná komunita | Credit pre expertov |
| Project foundation | Participant projektu | Credit pre aktívnych členov |
```

**Dopad:** Shared foundation nie je len technický koncept – je to *spoločensky hodnotný akt*, ktorý generuje reciprocitu.

---

### Vylepšenie D: Prepojenie INSPIRATION-09 (Topic Taxonomy) na ADR-034 (Interest Model)

**Chýba:** ADR-031 definuje Topic Layer, ADR-034 definuje Interest Model, ale chýba explicitné prepojenie – ako sa Interest Model *mapuje* na taxonómiu.

**Kam to dať:** Rozšíriť **ADR-034: Personal AI Internal Model**

**Čo doplniť do Interest Model sekcie:**

```markdown
### X.X Interest Model a taxonómia

Interest Model je štruktúrovaný podľa dvojúrovňovej taxonómie:

1. **Kategórie (strom)** – hierarchické, dedičné
   - Príklad: `business → small_business → food_business`
   - Záujem o podkategóriu automaticky znamená záujem o nadkategóriu (s nižšou váhou)

2. **Tagy (graf)** – voľné, nehierarchické
   - Príklad: `#sustainable`, `#community`, `#local`
   - Tagy sú nezávislé od kategórií

**Sémantické vzťahy medzi témami (pre šírenie záujmu):**
| Vzťah | Význam | Vplyv na Interest Model |
|-------|--------|------------------------|
| `parent_of` | Hierarchia | Záujem o child → záujem o parent |
| `related_to` | Príbuzné | Záujem o A → slabší záujem o B |
| `contradicts` | Protichodné | Záujem o A → zníženie záujmu o B |

Personal AI sa učí nielen *čo* používateľa zaujíma, ale aj *aké vzťahy medzi témami* preferuje.
```

**Dopad:** Interest Model nie je plochý zoznam – je to bohatá štruktúra, ktorá umožňuje inteligentné šírenie záujmu.

---

### Vylepšenie E: Prepojenie INSPIRATION-10 (Ratings) na ADR-028 (Support & Alignment)

**Chýba:** ADR-025 v3/v4 definuje ratings a kvalitu, ale chýba prepojenie na to, ako ratings *ovplyvňujú* support a alignment.

**Kam to dať:** Rozšíriť **ADR-028: Support & Alignment Model**

**Čo doplniť:**

```markdown
### X.X Vplyv kvality argumentov na alignment

Alignment nie je len o počte ľudí, ktorí podporujú vetvu.

**Kvalita argumentov ovplyvňuje alignment nasledovne:**

| Faktor | Vplyv na alignment |
|--------|-------------------|
| Quality score argumentov pre vetvu | Vyššia kvalita → vyššia váha alignment signálu |
| Popularity score argumentov | Nižší vplyv (oddelené od kvality) |
| Insightful ratings | Výrazne zvyšuje váhu argumentu |
| Off-topic ratings | Znižuje váhu argumentu |

**Príklad výpočtu váženého alignmentu:**

```
branch_alignment_weighted = 
    Σ (user_support * avg_argument_quality) / Σ avg_argument_quality
```

Kde `avg_argument_quality` je priemerná kvalita argumentov podporujúcich danú vetvu.
```

**Dopad:** Alignment nie je len "počet hlasov" – je to kvalitou vážený konsenzus.

---

### Vylepšenie F: Prepojenie INSPIRATION-11 (Calendar) na ADR-019 (Opportunity Engine)

**Chýba:** ADR-045 definuje kalendár a eventy, ale chýba prepojenie na Opportunity Engine – eventy môžu byť príležitosťou na zapojenie.

**Kam to dať:** Rozšíriť **ADR-019: Opportunity Engine**

**Čo doplniť:**

```markdown
### X.X Eventy ako príležitosti

Event (ADR-045) môže byť zdrojom Opportunity:

| Typ eventu | Generovaná príležitosť |
|------------|----------------------|
| Stretnutie | "Projekt X organizuje stretnutie – môžeš sa zúčastniť ako participant" |
| Workshop | "Workshop Y potrebuje facilitátora – tvoje schopnosti (8/10)" |
| Deadline | "Deadline pre rozhodnutie Z sa blíži – tvoj input je potrebný" |
| Brigáda | "Brigáda v projekte W potrebuje ľudí – tvoja lokalita je vhodná" |

**Prepojenie na P2P signály:**
- Eventy sa šíria ako dopytové signály (need_broadcast)
- Používateľova AI ich porovná s Capability Modelom a kalendárom
- Ak je match, vytvorí sa Opportunity
```

**Dopad:** Kalendár nie je len pasívny zoznam – je to aktívny zdroj príležitostí na zapojenie.

---

## 3. Prehľadná tabuľka vylepšení

| Vylepšenie | INSPIRATION | Kam to dať | Priorita |
|------------|-------------|------------|----------|
| **A** | 06 (Tree) | ADR-027 (View Layer) | Stredná |
| **B** | 07 (Forum) | ADR-025 (Argumentation) | Stredná |
| **C** | 08 (Shared Foundation) | ADR-036 (Reciprocity) | Nízka |
| **D** | 09 (Topic Taxonomy) | ADR-034 (Personal AI Internal Model) | Stredná |
| **E** | 10 (Ratings) | ADR-028 (Support & Alignment) | Vysoká |
| **F** | 11 (Calendar) | ADR-019 (Opportunity Engine) | Nízka |

---

## 4. Odporúčaný postup implementácie

| Krok | Čo spraviť | Prečo |
|------|-----------|-------|
| **1.** | Rozšíriť ADR-028 o vplyv kvality na alignment (E) | Kľúčové pre kvalitné rozhodovanie |
| **2.** | Rozšíriť ADR-027 o tree visualization (A) | Aby používateľ videl strom, nie len zoznam |
| **3.** | Rozšíriť ADR-034 o taxonómiu a vzťahy (D) | Pre bohatší Interest Model |
| **4.** | Rozšíriť ADR-025 o threading a typy príspevkov (B) | Pre živšiu diskusiu |
| **5.** | Rozšíriť ADR-036 a ADR-019 (C, F) | Menej urgentné, doladenia |

---

## 5. Zhrnutie pre vás

**Dobrá správa:** Všetkých 6 INSPIRÁCIÍ z druhého balíka je už pokrytých existujúcimi ADR (najmä vďaka ADR-025 v4 a ADR-045).

**Čo sa dá vylepšiť:** Chýbajú **prepojenia** medzi týmito konceptmi a ostatnými ADR:

| Chýbajúce prepojenie | Prečo je dôležité |
|---------------------|-------------------|
| Tree → View Layer | Používateľ musí vidieť strom argumentov, nie len zoznam |
| Ratings → Support & Alignment | Alignment musí byť vážený kvalitou, nie len počtom hlasov |
| Taxonomy → Interest Model | Interest Model potrebuje štruktúru (kategórie + tagy + vzťahy) |
| Forum → Argumentation | Diskusia potrebuje threading a typy príspevkov |

**Ak doplníte týchto 6 vylepšení (najmä A, B, D, E), systém bude mať kompletnú argumentačnú a koordinačnú vrstvu.** Vylepšenia C a F sú menej urgentné – môžu počkať.