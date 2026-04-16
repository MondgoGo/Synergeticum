# 🧾 ADR-078: Topic vs Project Transition Contract

## 1. 📌 Status

**Accepted** (revízia 2.0)

---

## 2. 🎯 Kontext

Synergetikum má dve samostatné vrstvy:

| Vrstva | ADR | Účel |
|--------|-----|------|
| **Topic Layer** | ADR-031 | Kolektívna diskusia, explorácia, bez záväzku |
| **Project Layer** | ADR-022, 023, 024 | Kolektívna akcia, záväzok, realizácia |

V reálnej implementácii vzniká problém:

> **Kedy presne sa z Topicu stane Project? A čo sa pri tom prenáša a čo nie?**

Bez jasného kontraktu hrozia:

- **Predčasné projekty** – Topic s jednou odpoveďou sa stane projektom
- **Uviaznuté topicy** – diskusia nikdy neprejde do akcie, lebo nikto nevie ako
- **Strata kontextu** – projekt nevie, z akej diskusie vznikol
- **Duplicita** – rovnaká diskusia vedie k trom projektom

---

## 3. ⚖️ Rozhodnutie

Systém zavádza **explicitný transition contract** medzi Topicom a Projectom.

> **Topic → Project je jednosmerná, explicitná, merateľná transformácia, ktorá vytvára nový Project Node a zachováva Topic ako historický kontext. Transition nie je moment – je to proces dozrievania.**

---

## 4. 🧠 Definícia Topic vs Project (ostré hranice)

| Vlastnosť | **Topic** | **Project** |
|-----------|-----------|--------------|
| **Primárny účel** | Explorácia, diskusia, zber perspektív | Akcia, realizácia, rozhodovanie |
| **Záväzok** | Žiadny (môžeš len čítať) | Explicitný (commitment signal) |
| **Výstupy** | Odpovede, názory, perspektívy | Vetvy, argumenty, úlohy, rozhodnutia |
| **Lifecycle** | Krátky (dni až týždne) | Dlhý (týždne až roky) |
| **Stabilita** | Nízka (mení sa každou odpoveďou) | Vyššia (zmeny sú vetvy, nie prepis) |
| **Kto môže vytvoriť** | Ktokoľvek (otázkou) | Len systém (cez transition) alebo user explicitne |
| **Expectation** | "Budeme sa rozprávať" | "Budeme niečo robiť" |

---

## 5. 🔄 Transition Trigger Conditions

Projekt môže vzniknúť z Topicu **iba ak**:

### 5.1 Dynamické thresholdy (nie hardcoded)

Thresholdy **nie sú pevné čísla**. Sú **dynamické / konfigurovateľné**:

| Úroveň | Kto nastavuje | Príklad |
|--------|---------------|---------|
| **Systémové defaulty** | Vývojári | `interest > 0.6`, `commitment >= 3`, `participants >= 2` |
| **Komunitné** | Trusted circle | Môžu si upraviť podľa seba |
| **Per-topic** | Personal AI (odhad) | Náročná téma = nižší threshold |

### 5.2 Adaptívne thresholdy podľa kontextu

| Faktor | Vplyv na threshold |
|--------|-------------------|
| **Veľkosť siete** | Čím väčšia sieť, tým **vyšší** threshold (inak príliš veľa projektov) |
| **Typ témy** | `technical` → vyšší, `social` → nižší |
| **História úspešnosti** | Témy, ktoré často viedli k mŕtvym projektom → vyšší threshold |

### 5.3 Časové okno (stabilita záujmu)

Jeden deň špičky nestačí. Záujem musí byť **stabilný**:

```
interest > threshold po dobu aspoň 7 dní (rolling window)
commitments dosiahnuté kedykoľvek v histórii topicu
participants aktívni v posledných 14 dňoch
```

### 5.4 Základné podmienky (systémové defaulty)

| Podmienka | Hodnota | Poznámka |
|-----------|---------|----------|
| `interest_level` | > 0.6 (default) | Dynamické podľa kontextu |
| `commitment_count` | >= 3 (default) | Dynamické podľa kontextu |
| `participant_count` | >= 2 (default) | Dynamické podľa kontextu |

---

## 6. 🔄 Local Suggestion vs Network Consensus

### 6.1 Problém: Kto rozhoduje?

Personal AI jednotlivých používateľov môžu mať **rôzne názory** na to, či je Topic pripravený na transition.

### 6.2 Riešenie: Dve úrovne

| Typ | Kto | Účel |
|-----|-----|------|
| **Local suggestion** | Každá Personal AI individuálne | "Mne to príde ako dobrý kandidát" – zobrazí sa len majiteľovi tej AI |
| **Network consensus signal** | Agregát z viacerých AI | "X% participantov si myslí, že téma je pripravená" – až to spúšťa transition |

### 6.3 Výpočet network consensus

```
network_consensus = (počet AI, ktoré hlásia "splnené") / (celkový počet participantov)

Ak network_consensus > 0.5 → transition je navrhnutý všetkým participantom
```

---

## 7. 🔄 Transition Process (krok za krokom)

### Krok 1: Detection (Network Consensus)

Systém agreguje signály z Personal AI participantov. Ak `network_consensus > 0.5`, Topic je označený ako `candidate_for_project`.

### Krok 2: Notification

Systém zobrazí **všetkým participantom**:

```
📢 Téma "{{topic.name}}" dosiahla dostatočný záujem na vznik projektu (na základe konsenzu participantov).

Kto chce byť súčasťou projektu?
[✓] Chcem sa zapojiť
[ ] Nie, len diskutujem
```

### Krok 3: Confirmation

Minimálne **2 participant** (alebo 50% aktívnych participantov, podľa toho, čo je viac) musia kliknúť "Chcem sa zapojiť".

### Krok 4: Project Creation

Systém automaticky vytvorí nový Project Node.

### Krok 5: Initial Branch Creation

Systém vytvorí **počiatočnú vetvu** projektu, ktorá obsahuje:

- Problem statement (extrahovaný z Topicu)
- Navrhované smery (z topic.signals.directions)
- Zoznam participantov, ktorí potvrdili záujem

### Krok 6: Cooling-off period (odvolacia lehota)

Po vytvorení projektu je **48-hodinová lehota** na odvolanie:

- Ak participant (alebo owner) odvolá podporu, transition sa zruší
- Topic sa vráti do stavu `active`
- Project sa označí ako `aborted` (alebo sa vymaže)

### Krok 7: Topic Update (Topic zostáva aktívny)

Topic **nie je read-only**. Topic ostáva aktívny, ale mení vzťah k projektu:

| Stav | Čo sa deje |
|------|-----------|
| **Topic po transition** | Môže sa doň stále prispievať (odpovede, argumenty) |
| **Ale:** | Nové príspevky už **neovplyvňujú projekt** (projekt má vlastný argumentačný graf) |
| **Ale:** | Ak diskusia generuje **zásadne nový smer** → môže vzniknúť **nový Topic** (a z neho ďalší projekt) |

### Krok 8: Notification to all participants

Všetci, ktorí kedy odpovedali v Topici, dostanú notifikáciu:

```
💡 Z témy "{{topic.name}}" vznikol projekt.
👉 Pozrieť projekt
```

---

## 8. 📦 Čo sa prenáša (Topic → Project)

| Položka | Prenáša sa? | Ako |
|---------|-------------|-----|
| Názov | ✅ | Base name + " (projekt)" |
| Popis | ✅ | Extrahovaný problem statement |
| Autor otázky | ✅ | Stáva sa owner projektu |
| Participants | ✅ | Tí, čo potvrdili, sú initial contributors |
| Odpovede | ❌ | Zostávajú v Topicu ako historický kontext |
| Perspektívy | ⚠️ | Extrahujú sa do initial branch ako "considerations" |
| Commitment signals | ✅ | Prepočítajú sa na initial alignment |
| Diskusia | ❌ | Nie je súčasťou projektu (len odkaz) |

---

## 9. 🚫 Čo sa NEPRENÁŠA (a prečo)

| Položka | Dôvod |
|---------|-------|
| Všetky odpovede | Projekt by bol preťažený diskusiou, nie akciou |
| Hlasovania | Topic nemá hlasovania, len signály |
| Neaktívni participant | Len aktívni chcú byť v projekte |
| Dočasné agregáty | Projekt začína čistým štítom (okrem kontextu) |

---

## 10. 🔄 Život Topicu po transition (Topic nie je mŕtvy)

Topic **nie je read-only**. Je to **zásobník nápadov**:

```
Topic (pôvodný) → Project A
         ↘
      (nový smer z diskusie)
         ↘
    Topic (fork) → Project B
```

| Situácia | Správanie |
|----------|-----------|
| Nová odpoveď v Topicu | Uloží sa, ale **neovplyvní** existujúci projekt |
| Nový smer v diskusii | Môže vzniknúť **nový Topic** (fork) |
| Forknutý Topic | Môže viesť k **ďalšiemu projektu** |

> Topic nie je mŕtvy. Je to zásobník nápadov, ktoré môžu viesť k ďalším projektom.

---

## 11. 🔗 Vzťah po transition

Po transition (po uplynutí cooling-off periódy):

- **Topic zostáva** – ako historický kontext, zdroj argumentov a zásobník nových nápadov
- **Topic je aktívny** – možno doň stále prispievať
- **Project má odkaz na Topic** – `origin_topic_id`
- **Topic má odkaz na Project** – `project_reference`

Používateľ môže kedykoľvek:
- Prejsť z Projectu do pôvodného Topicu (historický kontext)
- Prejsť z Topicu do Projectu (ak existuje)
- Vytvoriť nový Topic z diskusie v pôvodnom Topici

---

## 12. 🧠 Manuálny override

Používateľ (ktorýkoľvek participant) môže **explicitne vytvoriť projekt z Topicu** aj bez splnenia trigger conditions.

Ale systém zobrazí varovanie:

```
⚠️ Téma ešte nemá dostatočný záujem (network consensus = 0.2, potrebných 0.5).
Projekt vytvorený z nej bude mať nízku šancu na úspech.
Naozaj chcete pokračovať?
[Áno] [Nie]
```

---

## 13. 🚫 Čo sa NESMIE stať

| Zakázaná situácia | Prečo |
|-------------------|-------|
| Topic → Project bez zachovania Topic | Stráca sa historický kontext |
| Automatický transition bez potvrdenia | Používatelia stratia kontrolu |
| Projekt bez `origin_topic_id` | Nevie sa, odkiaľ prišiel |
| Topic s `project_reference`, ktorý neexistuje | Orphaned reference |
| Viacero projektov z jedného Topicu | Iba jeden (ale Topic môže byť forknutý) |
| Topic read-only po transition | Diskusia často pokračuje |
| Okamžitý transition bez časového okna | Jeden deň špičky ≠ stabilný záujem |
| Žiadne odvolacie obdobie | Chybný transition sa nedá vrátiť |

---

## 14. 📊 Príklad kompletného transition

### Before: Topic

```json
{
  "node_id": "topic_community_garden_001",
  "node_type": "Topic",
  "name": "Komunitná záhrada v Petržalke",
  "lifecycle": {
    "stage": "candidate_for_project"
  },
  "aggregates": {
    "interest_level": 0.78,
    "participant_count": 5,
    "response_count": 23
  },
  "signals": {
    "commitment_count": 4,
    "directions": [
      "hľadáme pozemok",
      "potrebujeme grant"
    ]
  },
  "transition": {
    "network_consensus": 0.8,
    "candidate_since": "2026-04-09T10:00:00Z"
  }
}
```

### After: Project

```json
{
  "node_id": "proj_community_garden_001",
  "node_type": "Project",
  "name": "Komunitná záhrada v Petržalke (projekt)",
  "content": {
    "origin_topic_id": "topic_community_garden_001",
    "lifecycle_stage": "forming",
    "transition_completed_at": "2026-04-16T10:00:00Z"
  },
  "relations": [
    { "target_id": "topic_community_garden_001", "relation_type": "originated_from" }
  ]
}
```

### After: Topic (updated)

```json
{
  "node_id": "topic_community_garden_001",
  "node_type": "Topic",
  "lifecycle": {
    "stage": "converted_to_project"
  },
  "project_reference": "proj_community_garden_001",
  "converted_at": "2026-04-16T10:00:00Z",
  "cooling_off_until": "2026-04-18T10:00:00Z",
  "is_active": true
}
```

---

## 15. 🧠 Dôsledky rozhodnutia

### ✅ Pozitívne

| Dôsledok | Popis |
|----------|-------|
| **Jasné hranice** | Developer vie, čo je Topic a čo Project |
| **Žiadne predčasné projekty** | Dynamické thresholdy + časové okno bránia chaosu |
| **Historický kontext** | Topic zostáva, nestráca sa diskusia |
| **Používateľská kontrola** | Transition vyžaduje explicitné potvrdenie |
| **Škálovateľnosť** | Väčšina topicov zostane v diskusii |
| **Funguje v každej komunite** | Dynamické thresholdy podľa veľkosti siete |
| **Žiadna jediná AI nerozhoduje** | Network consensus medzi AI |
| **Topic nie je mŕtvy** | Môže generovať nové projekty |
| **Odvolateľnosť** | Cooling-off period umožňuje vrátiť chybný transition |

### ❗ Negatívne / Trade-offs

| Dôsledok | Mitigácia |
|----------|-----------|
| **Vyššia komplexita** | Ale je to explicitné a testovateľné |
| **Možnosť uviaznutia** | Manuálny override + fade mechanism |
| **Thresholdy môžu byť nesprávne** | Nastaviteľné na viacerých úrovniach |

---

## 16. 🚫 Alternatívy (zamietnuté)

| Alternatíva | Dôvod zamietnutia |
|-------------|-------------------|
| **Žiadny transition (Topic a Project oddelené)** | Nikdy nevzniknú projekty |
| **Automatický transition bez potvrdenia** | Používatelia stratia kontrolu |
| **Topic sa stane Projectom (zmena typu)** | Stráca sa historická diskusia |
| **Len manuálny override** | Väčšina topicov nikdy neprejde do projektu |
| **Hardcoded thresholdy** | Nebudú fungovať v každej komunite |
| **Topic read-only po transition** | Diskusia často pokračuje |
| **Okamžitý transition** | Nestabilný záujem |

---

## 17. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-022 | Project Formation – tento ADR je špecifikácia prechodu |
| ADR-023 | Distributed Project Model – projekt vzniká z Topicu |
| ADR-024 | Branching – initial branch vzniká z Topic signálov |
| ADR-031 | Topic Layer – definuje Topic, z ktorého sa prechádza |
| ADR-015 | Participation Filter – kto vidí transition návrh |
| ADR-036 | Reciprocity – commitment signals ovplyvňujú transition |
| ADR-077 | MVPj – projekt po transition musí spĺňať MVPj |
| ADR-076 | Typed Projections – Topic a Project projekcie |
| ADR-075 | Domain State vs Process Boundary – transition je proces |

---

## 18. 📏 Pravidlá implementácie (high-level)

Systém musí:

1. Ukladať `topic.aggregates` a `topic.signals` pre detekciu triggerov
2. **Implementovať dynamické thresholdy** (systémové defaulty + komunitné + per-topic)
3. **Implementovať adaptívne thresholdy** podľa veľkosti siete a typu témy
4. **Implementovať network consensus** medzi Personal AI participantov
5. **Implementovať časové okno** (7 dní stabilného záujmu)
6. Periodicky (napr. každých 24h) vyhodnocovať topicy
7. Pri splnení podmienok zobraziť notifikáciu participantom
8. Po potvrdení vytvoriť Project Node a prepojiť ho s Topicom
9. **Implementovať cooling-off period** (48 hodín na odvolanie)
10. **Zachovať Topic aktívny** po transition (nie read-only)
11. **Umožniť forkovanie Topicu** na nové diskusie a projekty
12. Nikdy nemažte Topic (len mení stage)

---

## 19. 🧪 Testovacie scenáre (pre pilot)

| Scenár | Očakávaný výsledok |
|--------|---------------------|
| Topic s 1 participantom, vysoký interest | Žiadny transition |
| Topic s 2 participantmi, nízky interest | Žiadny transition |
| Topic s 5 participantmi, interest 0.8 po 7 dní | Transition navrhnutý |
| Iba 1 participant potvrdí záujem | Žiadny transition |
| 2+ participantov potvrdí | Transition prebehne |
| Niekto odvolá počas cooling-off | Transition zrušený, Topic späť v `active` |
| Po cooling-off nikto neodvolá | Projekt oficiálne `forming` |
| V Topici pokračuje diskusia po transition | Nové príspevky sa ukladajú, neovplyvňujú projekt |
| V Topici vznikne nový zásadný smer | Môže vzniknúť nový Topic (fork) |
| Manuálny override pri nespĺňaní podmienok | Varovanie, ale povolené |
| Malá komunita (5 ľudí) | Thresholdy sa automaticky znížia |
| Veľká komunita (1000 ľudí) | Thresholdy sa automaticky zvýšia |

---

## 20. 🔥 Zhrnutie

> **Topic je diskusia. Project je akcia. Prechod z Topicu do Projectu je explicitný, merateľný, dozrieva v čase a vyžaduje konsenzus medzi participantmi. Topic po transition nie je mŕtvy – je to zásobník nápadov pre ďalšie projekty.**

Tento ADR definuje:

- Ostré hranice medzi Topic a Project
- **Dynamické a adaptívne thresholdy** (nie hardcoded)
- **Local suggestion vs network consensus**
- **Časové okno** (7 dní stabilného záujmu)
- **8-krokový transition process**
- **Cooling-off period** (48 hodín na odvolanie)
- **Topic zostáva aktívny** (nie read-only)
- **Forkovanie Topicu** na nové projekty
- Čo sa prenáša a čo nie
- Manuálny override
- Čo sa nesmie stať

---

## 21. 🧭 Finálna veta

> **Projekty nevznikajú z otázok. Vznikajú z diskusií, ktoré dozreli v čase, dosiahli konsenzus medzi participantmi a preukázali stabilný záujem. Až potom – a len vtedy – sa Topic stane Projectom. Ale Topic neumiera. Je to zásobník nápadov, ktoré čakajú na ďalšie projekty.**

---

**Dátum:** 2026-04-16  
**Stav:** Accepted  
**Typ:** Transition Contract ADR  
**Verzia:** 2.0 (revízia podľa pripomienok)