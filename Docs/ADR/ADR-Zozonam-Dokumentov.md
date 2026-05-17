# Zoznam ADR pre systém **Synergetikum**

## ADR-001 — Personal AI Ownership Model

Definuje, že každý používateľ vlastní svoj osobný AI model. Model je lokálny, súkromný a neexistuje centrálny „master profil“ používateľa.

## ADR-002 — Human-in-the-loop Decision Model

Definuje, že AI nikdy nerozhoduje za človeka. AI pomáha myslieť, sumarizuje, simuluje a kladie otázky, ale finálne rozhodnutie je vždy ľudské.

## ADR-003 — Non-Manipulation Principle

Definuje, že systém nesmie používateľa manipulovať, tlačiť do „správnych“ odpovedí ani skryto riadiť jeho smerovanie. Môže ponúkať možnosti, ale nie vnucovať smer.

## ADR-004 — Privacy-first Architecture

Definuje súkromie ako architektonický princíp. Dáta ostávajú primárne lokálne, systém minimalizuje zdieľanie a vyhýba sa centralizovanému profilovaniu.

---

## ADR-005 — Personal AI Model Definition

Definuje, čo je Personal AI ako entita systému. Čo reprezentuje, aké má vstupy, výstupy a akú úlohu hrá vo vzťahu k používateľovi.

## ADR-006 — Learning Model (Behavior-based Learning)

Definuje, ako sa Personal AI učí. Teda z čoho sa učí, ako sa mení profil a aké signály majú väčšiu alebo menšiu váhu.

## ADR-007 — User Profile Representation

Definuje, ako je reprezentovaný používateľ v systéme. Nie ako pevná nálepka, ale ako rozloženie signálov, preferencií a vzorcov správania.

## ADR-008 — Data Sources Strategy

Definuje, z akých typov dát sa systém učí. Rozlišuje priame odpovede, implicitné správanie, prípadné externé zdroje a ich hranice.

---

## ADR-009 — Question-based Interaction Model

Definuje, že základným interakčným mechanizmom systému sú otázky. Používateľ nevstupuje do systému cez chat, ale cez vedome navrhnuté otázky.

## ADR-010 — Perspective System

Definuje rozklad každej otázky na viac perspektív. Tá istá otázka môže byť formulovaná prakticky, analyticky, systémovo alebo akčne.

## ADR-011 — Personalization Strategy

Definuje, ako sa obsah personalizuje. Nezmení sa pravda otázky, ale mení sa poradie perspektív a priorita tém podľa používateľa.

## ADR-012 — Engagement Model

Definuje úrovne zapojenia používateľa. Rozlišuje pasívne pozretie, odpoveď, diskusiu, záväzok a reálnu participáciu.

## ADR-013 — Reflection Loop Design

Definuje reflexnú slučku. AI neodpovedá len smerom k používateľovi, ale vracia mu otázky späť, aby sa učila z jeho hodnôt a priorít.

---

## ADR-014 — Network Communication Model (P2P)

Definuje, že systém funguje ako peer-to-peer sieť. Modely komunikujú priamo alebo cez pomocné mechanizmy, bez centrálnej inteligencie.

## ADR-015 — Participation Filter Model

Definuje, kto sa zapája do otázok, topicov a projektov. Každý uzol lokálne rozhoduje, či je téma relevantná a či má zmysel participovať.

## ADR-016 — Collective Query System

Definuje, ako sa otázka distribuuje do siete a ako vznikajú kolektívne odpovede. Nie ľudia priamo, ale ich modely vracajú odpovede a signály.

## ADR-017 — Collaboration Model

Definuje, ako zo siete vzniká reálna spolupráca medzi ľuďmi. Ako sa zodpovedá otázka „kto sa vie zapojiť a ako“.

---

## ADR-018 — Simulation Engine

Definuje simuláciu možných scenárov. AI ukazuje možné budúcnosti a dôsledky, ale nevyhlasuje jednu „pravdivú“ predikciu.

## ADR-019 — Opportunity Engine

Definuje, ako systém identifikuje možnosti zapojenia. Napríklad kto je vhodný ako realizátor, investor, organizátor alebo analytik.

## ADR-020 — Growth Model

Definuje, ako systém podporuje rozvoj používateľa bez nátlaku. Teda ako rozširuje perspektívu, ale nevnucuje smer.

## ADR-021 — Parent Node Architecture

Definuje úlohu desktopu alebo stále online zariadenia ako rozšírenia osobnej AI. Parent node môže robiť ťažšiu analýzu a stabilnejšie učenie.

---

## ADR-022 — Collective Project Formation

Definuje, ako z kolektívnych otázok, odpovedí a signálov vzniká projekt. Zavádza koncept Living Project Document.

## ADR-023 — Distributed Project Object Model

Definuje, kde a ako projekt existuje v sieti. Projekt nežije na jednom zariadení, ale ako distribuovaný objekt medzi relevantnými uzlami.

## ADR-024 — Multi-Variant Project Governance & Branch Lifecycle

Definuje, že projekt nie je jedna pravda, ale strom vetiev. Zavádza branching, paralelné varianty a základný lifecycle vetiev.

## ADR-025 — Argumentation & Reasoning Model

Definuje argumentačný model pre konceptuálne a normatívne projekty. Rieši prácu s argumentmi, protiargumentmi a kvalitou reasoning procesu.

## ADR-026 — Project Roles & Permission Model

Definuje roly v projekte, napríklad iniciátor, contributor, executor, a aké capability alebo oprávnenia majú. Rieši aj to, že iniciátor nie je absolútny vlastník projektu.

## ADR-027 — Project View & Simplification Model

Definuje, ako sa komplexný projekt zjednodušuje pre konkrétneho používateľa. Zavádza rozdiel medzi plnou realitou projektu a personalizovaným view.

## ADR-028 — Support & Alignment Model

Definuje, ako sa meria podpora vetiev projektu. Rozlišuje alignment, commitment, confidence a voliteľne expertízu, namiesto jednoduchého hlasovania.

## ADR-029 — Branch Lifecycle Model

Definuje detailný životný cyklus vetvy projektu: vznik, rast, stabilizáciu, dormant stav, archiváciu a oživenie.

## ADR-030 — Sync & Persistence Strategy

Definuje stratégiu ukladania a synchronizácie. Hybridný local-first model, kde sa synchronizujú zmeny a eventy, snapshoty stabilizujú stav a konflikty rieši branching.

## ADR-031 — Topic Layer & Project Formation

Definuje medzivrstvu medzi otázkou a projektom. Zavádza Topic Object ako diskusný objekt, z ktorého sa až následne môže stať projekt.

## ADR-032 — Question & Prompt Generation System

Definuje, ako vznikajú otázky, z akých zdrojov, ako sa štruktúrujú a ako sa personalizuje ich poradie a perspektívy.

## ADR-033 — Participation Filter & Relevance Engine

Rozšírená verzia participation logiky. Definuje participation score, relevance engine a typy participácie, ak sa to chce oddeliť detailnejšie od ADR-015.

## ADR-034 — Personal AI Internal Model

Definuje vnútorný model Personal AI. Teda čo presne obsahuje: záujmy, schopnosti, preferencie, participačné vzorce, kognitívne rozloženie a eventy.

## ADR-035 — Sustainability & Incentive Model

Definuje, ako je systém financovaný a udržateľný bez porušenia princípov súkromia a vlastníctva. Rieši granty, parent node služby, open-source model a to, prečo nie je závislý na predaji dát alebo reklame.

## ADR-036 — Reciprocity & Value Exchange Model

Definuje, ako systém prirodzene odmeňuje participáciu kvalitnejšími výstupmi – bez bodov, rebríčkov alebo gamifikácie. Rieši cost asymmetry (neparticipácia = absencia benefitov, nie penalizácia) a to, ako používateľ vníma, že jeho participácia má zmysel.

## ADR-037 — Identity Recovery & Continuity Model

Definuje, ako používateľ nestráca svoju digitálnu identitu pri strate zariadenia. Rieši multi-path recovery (parent node, trusted circle, offline seed), identity branching (obnova = nová vetva) a psychologickú kontinuitu „ja“ v systéme.

---

## ADR-038 — Power & Influence Balancing

Definuje, kto má vplyv v systéme a prečo. Rieši mitigáciu dominancie hlasných alebo silných aktérov, reputáciu bez centralizácie a ochranu pred vznikom „elitného“ systému. (Návrh, implementácia až po pilotovi.)

## ADR-039 — Truth & Epistemic Model

Definuje, čo je pravda v distribuovanom systéme bez centrálnej autority. Rieši vzťah medzi konsenzom, expertízou a kvalitou argumentácie – a to, že väčšina nie je automaticky pravda. (Návrh, implementácia až po pilotovi.)

## ADR-040 — Graceful Degradation

Definuje, ako systém zostáva použiteľný aj pri nízkej kvalite vstupov, hluku, pasivite alebo útokoch. Rieši toleranciu voči chaosu a to, že degradácia je plynulá, nie prudká. (Princíp je v ADR-000, detailné mechanizmy tu.)

## ADR-041 — Recovery Storage & Snapshot Strategy

Definuje konkrétne technické parametre recovery – frekvenciu snapshotov, šifrovací algoritmus, počet a umiestnenie shardov, threshold pre obnovu a maximálnu stratu dát. (Implementačný detail k ADR-037.)

## ADR-042 — Unified Entity Model (Node)
Definícia `Node` ako univerzálneho typu s poliami: `node_id`, `node_type`, `name`, `attributes`, `relations`, `rights`
Definícia `node_type` enum: `Project`, `Topic`, `Argument`, `Question`, `Event`, `Person`, `Branch`
Vzťahy medzi Node-mi (orientovaný graf): `parent_of`, `classified_as`, `supported_by`, `opposes`, `forked_from`, `merged_into`

## ADR-045 — Calendar & Event Coordination
Definícia `Event` ako špecifický typ Node – s atribútmi `start_time`, `end_time`, `location`, `recurrence`.
Eventy môžu byť spojené s projektom, vetvou, argumentom alebo témou.
Notifikačný mechanizmus (opt-in, lokálne rozhodnutie, nie centrálny server).

* ADR-043 — Participation Reality Model**
* ADR-046 — Auditability
* ADR-047 — Open Source Governance
* ADR-048 — Parent Node Certification
* ADR-049 — Minimal Service Level Guarantee**

- ADR-050 — Identity Noise
- ADR-051 — Cold Start
- ADR-052 — Anti-Gaming
- ADR-053 — Context Model
- ADR-054 — UX Contract
- ADR-055 — Simplicity Constraints
- ADR-056 — Failure & Recovery

- 057 — Friction
- 058 — Behavior vs Answer
- 059 — Question Quality
- 060 — Reality Exposure
- 061 — Immediate Value
- 062 — Commitment UX
- 063 — Attention
- 064 — Failure

- 065 — Network reality
- 066 — Filter failure
- 067 — Sybil
- 068 — Rate limiting
- 069 — Collaboration reality
- 070 — Influence signals
- 071 — Network health

- 072 — Bootstrap
- 073 — Trust
- 074 — Latency
---

# Poznámka k poradiu a prekryvom

Niektoré ADR sa čiastočne prekrývajú, hlavne:

* **ADR-015** a **ADR-033**
  Obe riešia participation filter. ADR-033 je detailnejšie rozpracovanie. Buď ich necháte obe, alebo ADR-033 zlúčite do ADR-015.

* **ADR-022** a **ADR-031**
  ADR-022 rieši vznik projektu, ADR-031 dopĺňa chýbajúcu medzivrstvu Topic Layer. Obe dávajú zmysel držať oddelene.

* **ADR-024** a **ADR-029**
  ADR-024 rieši branching a governance na vysokej úrovni, ADR-029 ide detailne do lifecycle vetiev. Obe sú v poriadku.

---

# Praktické zoskupenie ADR

Ak si to chceš usporiadať do logických balíkov:

## A. Filozofia a princípy

* ADR-001
* ADR-002
* ADR-003
* ADR-004

## B. Personal AI a používateľský model

* ADR-005
* ADR-006
* ADR-007
* ADR-008
* ADR-034

## C. Otázky, interakcia a personalizácia

* ADR-009
* ADR-010
* ADR-011
* ADR-012
* ADR-013
* ADR-032

## D. Sieť a participácia

* ADR-014
* ADR-015
* ADR-016
* ADR-017
* ADR-033

## KAPITOLA E — Domain Model (Projects & Entities)
👉 „čo projekt JE“
* ADR-022 — Collective Project Formation
* ADR-023 — Distributed Project Object Model
* ADR-024 — Multi-Variant Project Governance
* ADR-026 — Project Roles & Permission Model
* ADR-042 — Unified Entity Model (Node)
* ADR-075 — Domain State vs Process Boundary
* ADR-076 — Typed Projections over Node
* ADR-077 — Minimal Viable Project Definition
* ADR-078 — Topic vs Project Transition Contract
* ADR-082 — Domain Validation Rules

## KAPITOLA F — Kolektívna inteligencia (Reasoning & Truth)
👉 „ako projekt myslí“
* ADR-025 — Argumentation & Reasoning Model
* ADR-028 — Support & Alignment Model
* ADR-038 — Power & Influence Balancing
* ADR-039 — Truth & Epistemic Model

## KAPITOLA G — Vznik a príležitosti (Formation Layer)
👉 „prečo niečo vznikne“
* ADR-018 — Simulation Engine
* ADR-019 — Opportunity Engine
* ADR-031 — Topic Layer & Project Formation

## KAPITOLA H — Život projektu (Lifecycle & Operations)
👉 „ako projekt žije alebo zomiera“
* ADR-029 — Branch Lifecycle Model
* ADR-027 — Project View & Simplification Model
* ADR-020 — Growth Model
* ADR-045 — Calendar & Event Coordination

## Kapitola I — Infra & Runtime Layer
* ADR-I-000 — Infra Layer Definition
* ADR-I-001 — Runtime Execution Model
* ADR-I-002 — Compute Budget & Model Constraints
* ADR-I-003 — Event Processing & Scheduling Model
* ADR-021 Parent Node Architecture
* ADR-030 Sync & Persistence Strategy
* ADR-041 (Recovery Storage & Snapshot Strategy)
---


