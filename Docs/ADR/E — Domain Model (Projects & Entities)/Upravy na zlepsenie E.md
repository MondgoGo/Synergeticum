Jasné. Pôjdem **po jednom dokumente zvlášť** a budem sa držať cieľa, ktorý si pomenoval:

> dostať kapitolu E do stavu, kde sa podľa nej dá **navrhnúť reálny domain model a spraviť pilot**,
> s dátami, ktoré budú:
>
> * kvalitné,
> * stabilné v čase,
> * ale nie prehnane rigidné.

Nebudem teda hodnotiť „ako pekne je to napísané“, ale:

* čo je silné,
* kde je riziko pre budúcu implementáciu,
* čo by som zmenil, aby sa to nemuselo bolestivo prerábať.

---

# ADR-022 — Collective Project Formation



## Čo je silné

Tento dokument dobre pomenúva, že systém potrebuje vrstvu, ktorá transformuje:

* diskusiu,
* signály záujmu,
* návrhy,
* zdroje

na projekt. To je správne. Tiež je dobré, že hovorí o **human validation** a že realizáciu neriadi samotný systém. To drží projekt na zemi. 

## Hlavný problém

Toto ADR je zatiaľ **príliš konceptuálne** a ešte nie je dosť tvrdé ako podklad pre architekta.

Najväčší problém je, že zavádza:

* *Project Formation Layer*
* *Living Project Document*
* lifecycle fáz
* typy dokumentov

ale stále presne nehovorí:

> **čo je input, čo je output a čo je transition contract**

Inými slovami:

* kedy vzniká Topic,
* kedy candidate,
* kedy Project,
* čo presne sa vytvorí v storage modeli.

## Čo by som zmenil

### 1. Prestať používať `Living Project Document` ako hlavnú doménovú entitu

To je dobrý metaforický názov, ale pre architekta je nebezpečný.

Odporúčam:

* nechať ho ako vysvetľujúci pojem,
* ale **doménovo pracovať s `Project Node` / `Project Aggregate`**.

Lebo inak sa ľahko stane, že:

* niekto bude modelovať dokument,
* iný workflow objekt,
* iný grafový uzol.

### 2. Doplniť presný formation contract

Toto ADR by malo explicitne povedať:

**Input:**

* Topic signály
* commitmenty
* participanti
* directions

**Output:**

* nový Project v `forming`
* initial branch
* link na origin topic

Bez toho je to skôr vízia než architektonický kontrakt.

### 3. Typy „Insight / Proposal / Project Document“ by som nebral ako core doménové typy

Toto je veľké riziko.

To skôr vyzerá ako:

* prezentačný stav,
* alebo view/materialization,
* nie ako stabilný domain model.

Odporúčanie:

* buď to presuň do view layer,
* alebo to pomenuj ako **presentation profiles**, nie ako doménové dokumenty.

## Verdikt

Toto ADR má správny zámer, ale ešte nie je dosť „tvrdé“.
Pre pilot je použiteľné len vtedy, ak sa previaže natvrdo s ADR-077 a ADR-078.

### Najvyšší ROI úpravy

* nahradiť jazyk „Living Project Document“ presnejším doménovým jazykom
* doplniť formation input/output contract
* presunúť Insight/Proposal/Project Document skôr do prezentačnej alebo workflow vrstvy

---

# ADR-023 — Distributed Project Object Model



## Čo je silné

Tento dokument správne definuje, že projekt:

* nie je viazaný na jedno zariadenie,
* nemá master node,
* konflikty sa neriešia prepísaním, ale vetvením,
* musí prežiť offline režim. 

To je veľmi dôležité a filozoficky konzistentné.

## Hlavný problém

Toto ADR je zatiaľ príliš „čisté“ a málo realistické pre prvý pilot.

Najmä tieto body sú riskantné:

* „projekt prežije, ak existuje aspoň jeden uzol, ktorý ho drží“ 
* „pravda je výsledok synchronizácie medzi uzlami“ 

To sú správne princípy, ale architekt potrebuje vedieť aj:

* koľko replík je minimum pre zdravý projekt,
* čo je degradovaný stav,
* čo je orphaned project,
* ako sa discovery deje v pilotnom režime.

## Čo by som zmenil

### 1. Zaviesť survival thresholds

Doplniť aspoň tieto pojmy:

* **healthy replicated project**
* **degraded project**
* **single-holder project**
* **orphaned project**

Lebo „existuje aspoň jeden uzol“ je technicky pravda, ale produktovo už takmer smrť.

### 2. Pre pilot explicitne zjednodušiť discovery

Aktuálne je discovery len pomenované. To nestačí.

Pre pilot by som odporučil:

* discovery len medzi participant nodes,
* žiadne všeobecné open network hľadanie,
* parent node alebo participant list ako bootstrap.

Inak je to príliš otvorené a ťažko implementovateľné.

### 3. „Projekt ako strom“ by som zjemnil

Ak je branching dôležitý, tak projekt je skôr:

* **rooted graph with lineage constraints**
  než čistý strom.

Lebo:

* merge,
* references,
* argumenty,
* eventy

zvyknú veľmi rýchlo rozbiť čistý tree model.

Architektovi by som teda odporúčal:

* nehovoriť o „tree“ ako o presnej dátovej štruktúre,
* ale o **project graph with rooted branch lineage**.

## Verdikt

Silný dokument na princípy, ale ešte príliš hrubý pre implementáciu.

### Najvyšší ROI úpravy

* doplniť project survival thresholds
* zjednodušiť discovery pre pilot
* nespájať celý projekt s tree modelom príliš tvrdo

---

# ADR-024 — Multi-Variant Project Governance & Branch Lifecycle



## Čo je silné

Toto ADR veľmi dobre zachytáva jadro tvojej filozofie:

* konflikt nemá viesť k centralizovanému víťazovi,
* ale k paralelným vetvám. 

Tiež je silné, že už zavádza:

* `Active`
* `Dormant`
* `Archived`
  pre vetvy. To je veľmi realistický krok. 

## Hlavný problém

Dokument ešte stále hovorí **prečo branching**, ale málo hovorí **koľko ho systém unesie**.

To je kritické.

Bez ceny za vetvu sa stane:

* every disagreement = new branch
* systém sa rozpadne na fragmenty
* view layer bude len hasiť dôsledky

## Čo by som zmenil

### 1. Zaviesť explicitný branch creation threshold

Nová vetva nesmie vzniknúť len preto, že niekto nesúhlasí.

Mal by existovať aspoň jeden z:

* zásadný rozdiel v riešení,
* nový resource model,
* nový execution path,
* nový risk profile.

### 2. Doplniť branch justification

Každá vetva by mala mať povinné minimum:

* prečo vznikla,
* v čom sa líši,
* na čo reaguje,
* parent branch.

To dramaticky zvýši kvalitu dát.

### 3. Doplniť merge semantics

Dokument hovorí, že zlučovanie je voliteľné. 
To je v poriadku, ale architekt bude potrebovať vedieť, či:

* merge vytvára novú vetvu,
* alebo mení existujúcu,
* alebo len vytvára relation.

Toto treba povedať presnejšie.

## Verdikt

Veľmi dobrý strategický dokument, ale pre pilot ešte potrebuje brzdy proti vetvovej explózii.

### Najvyšší ROI úpravy

* threshold pre vznik vetvy
* branch justification payload
* presnejšia merge semantika

---

# ADR-026 — Project Roles & Permission Model



Pozor: obsah nahratého pointera `turn14file3` je v skutočnosti ADR-036, nie ADR-026.
Takže k ADR-026 sa neviem korektne odcitovať z načítaného pointera a nechcem si vymýšľať. Držím sa toho, čo si o ňom písal predtým a čo naň odkazujú ostatné ADR. Ak chceš, pošli znova samotný súbor ADR-026 alebo ho otvoríme presne.

Aj tak ti dám **architektonický komentár k role modelu**, ale bez predstierania, že citujem jeho konkrétny text.

## Čo by malo byť silné

Roly sú nutné, lebo bez nich nevieš:

* kto môže meniť projekt,
* kto len pozoruje,
* kto nesie zodpovednosť.

## Najväčšie riziko

Väčšina role modelov padne na toto:

* owner zmizne,
* contributor už neprispieva,
* participant nič nerobí,
* ale práva ostanú naveky.

## Čo by som určite chcel mať v ADR-026

### 1. Role decay

* rola nie je večná
* aktivita má vplyv na praktické oprávnenia

### 2. Capability vs permission

Treba oddeliť:

* kto **môže**
* kto **vie**
* kto **reálne robí**

Inak bude systém miešať sociálnu rolu a praktické oprávnenie.

### 3. Owner replacement protocol

Musí existovať jasné pravidlo:

* čo ak owner zmizne,
* kto môže prevziať,
* po akej dobe,
* za akých podmienok.

### 4. Pilot simplification

Pre pilot by som role držal veľmi jednoduché:

* owner
* contributor
* observer

Všetko viac je skoro.

## Verdikt

ADR-026 je kritický dokument pre realitu, ale nechcel by som, aby bol „organizačne krásny a behaviorálne naivný“.

---

# ADR-042 — Unified Entity Model (Node)



## Čo je silné

Toto je jeden z najdôležitejších dokumentov v celej architektúre.
Správne zavádza:

* jednotný model Node,
* jednotné relations,
* fraktálnu reprezentáciu,
* zjednotenie storage/sync/rights. 

To je výborný základ.

## Hlavný problém

Tento dokument je veľmi silný, ale aj veľmi nebezpečný.

Najväčší risk je tu:

> `Node` sa môže stať „všetkým pre všetkých“

To znamená:

* slabé typové hranice,
* slabé invarianty,
* všetko skončí v `content`,
* a po čase budeš mať generic blob.

## Čo by som zmenil

### 1. Zúžiť core Node

Node by mal byť maximálne stabilný a maximálne malý:

* identity
* timestamps/version
* type
* minimal rights/visibility
* relations

A čo najviac doménovej variability by malo byť nad ním.

### 2. Relations typy stabilizovať skoro

Ak sa budú relation types meniť často, rozbiješ:

* sync,
* query layer,
* view layer,
* validation.

Takže relation enum treba držať veľmi konzervatívne.

### 3. `Thread` by som zvážil

V tejto fáze si nie som istý, či `Thread` naozaj patrí ako plnohodnotný first-class Node typ. 
Môže to byť:

* buď entita,
* alebo len view/materialization nad argumentmi a odpoveďami.

Pre pilot by som sa pýtal, či ho fakt potrebuješ.

### 4. `rights` model je príliš mäkký

`edit/view/comment` ako zoznam userov alebo rolí je pochopiteľný, ale do budúcna môže byť:

* ťažko synchronizovateľný,
* ťažko dedičný,
* ťažko auditovateľný. 

Pre pilot OK. Pre budúcnosť by som chcel:

* ACL policy model,
* alebo inheritance pravidlá.

## Verdikt

Výborný foundation ADR, ale musí sa držať minimalisticky, inak sa z neho stane kontajner pre všetok chaos.

### Najvyšší ROI úpravy

* zúžiť stabilné core polia Node
* byť veľmi opatrný s novými node_type
* relation typy zmrazovať skoro
* nechať čo najviac variability nad Node, nie v ňom

---

# ADR-075 — Domain State vs Process Boundary



## Čo je silné

Toto ADR je architektonicky veľmi správne.
Presne pomenúva problém miešania:

* stavu,
* workflow,
* transitionov,
* a command logiky. 

A správne zavádza:

* Domain State
* Process
* Commands
* Preconditions / Postconditions

To je veľmi vysoké ROI.

## Hlavný problém

Je tu jedna jemná, ale dôležitá vec:

Dokument miestami tvrdí:

* Domain State nevie o Process
* a zároveň pripúšťa derived fields a invarianty. 

To je v poriadku, ale treba veľmi presne držať hranicu, aby sa tam nezačali pašovať:

* `CanTransitionToActive()`
* `TryPublish()`
* workflow hints

## Čo by som zmenil

### 1. Explicitne povedať, že derived fields nesmú závisieť na external context

Napríklad:

* `DormancyDays` je OK, ak je jasné `now`
* ale derived logic nesmie volať repository, policy service, ani AI

### 2. Commands rozdeliť na:

* domain commands
* orchestration commands

Lebo:

* `AddParticipant`
  je iný typ zmeny než
* `ConvertTopicToProject`

To druhé je skoro orchestration workflow.

### 3. Pre pilot odporúčam držať Process vrstvu malú

Len:

* create
* publish
* activate
* dormancy
* archive
* add/remove participant
* convert topic → project

Nič viac.

## Verdikt

Veľmi silný dokument.
Ak ho udržíš disciplinovaný, výrazne znížiš budúci chaos.

### Najvyšší ROI úpravy

* spresniť derived fields boundary
* rozdeliť domain commands vs orchestration commands
* pre pilot držať procesný set malý

---

# ADR-076 — Typed Projections over Node



## Čo je silné

Táto verzia je už oveľa bližšie realite.
Máš tam správne:

* read vs write separation,
* command layer,
* mapper registry,
* projection profily,
* version-aware mapping,
* partial projection availability. 

To je veľký posun.

## Hlavný problém

Teraz už hlavný problém nie je koncept, ale **rozsah**.

Ak nebudeš disciplinovaný, vznikne:

* priveľa projekcií,
* priveľa mapperov,
* priveľa variantov,
* a tím sa v tom stratí.

## Čo by som zmenil

### 1. Zaviesť naming convention pre projekcie

Napríklad striktne:

* `ProjectSummary`
* `ProjectDetail`
* `ProjectDomainView`

A nič mimo toho bez silného dôvodu.

### 2. Explicitne povedať, čo je source of truth pre read-only metódy

Keď `Project.CanTransitionToActive()` žije na projekcii, musí byť jasné:

* či používa len lokálne polia,
* alebo aj policy thresholds.

Ja by som odporučil:

* projekcia vie overiť len **structural readiness**
* finálne rozhodnutie robí command handler / policy layer

### 3. Partial projection availability je super, ale treba UX contract

Ak `Detail` neviem vytvoriť a `Summary` áno, UI musí vedieť:

* čo zobraziť,
* čo je degradovaný stav,
* čo je chyba dát.

Toto treba previazať s view layer.

## Verdikt

Tento ADR je už veľmi silný a skoro pripravený pre implementáciu.

### Najvyšší ROI úpravy

* prísna naming convention
* hranica read-only metód vs policy
* previazať s degradačným správaním UI

---

# ADR-077 — Minimal Viable Project Definition



## Čo je silné

Táto nová verzia je výrazne lepšia než pôvodná.
Správne zavádza:

* existenčnú validitu,
* operačnú pripravenosť,
* solo `forming`,
* collective `active`,
* commitment TTL/decay,
* event-driven revalidáciu,
* hysterézu. 

To je presne smer, ktorý z neho robí reálne použiteľný dokument.

## Hlavný problém

Dokument je už veľmi dobrý, ale ešte by som dal pozor na dve veci:

### 1. `participant_count >= 2` pre active je stále trochu hrubé

Pre pilot OK.
Do budúcna ale možno nebude stačiť len počet.

Treba rátať s tým, že:

* druhý participant môže byť pasívny,
* alebo commitment môže byť nízkej kvality.

To znamená, že do budúcna by active readiness mala byť:

* count +
* active commitment quality +
* recent activity.

### 2. `owner must be alive and valid`

To je správny princíp, ale architekt bude potrebovať vedieť:

* čo presne znamená alive,
* ako sa to zisťuje v decentralizovanom svete.

Pre pilot by som to zjednodušil na:

* owner exists
* owner not revoked
* owner not inactive beyond threshold

## Čo by som zmenil

### 1. Zadefinovať `active participant`

Nie len participant count.

### 2. Oddeľ:

* invariantné minimum projektu
* policy windows (30 dní, 14 dní, 7 dní)

Tie druhé by som nenechával ako implicitnú pravdu domény.

### 3. Reaktivácia z dormant

Hysteréza je super, ale odporúčam ešte explicitne povedať:

* či reaktivácia ide späť do `forming`
* alebo rovno do `active`

Momentálne sa to dá čítať oboma smermi.

## Verdikt

Toto je už veľmi dobrý dokument a skutočne použiteľný podklad pre architekta.

### Najvyšší ROI úpravy

* zadefinovať „active participant“
* oddeliť domain invariant vs policy windows
* spresniť pravidlo reaktivácie

---

# ADR-078 — Topic vs Project Transition Contract



## Čo je silné

Táto verzia je výrazne lepšia než prvá:

* transition je proces, nie moment,
* sú tam dynamické thresholdy,
* local suggestion vs network consensus,
* time window,
* cooling-off,
* Topic ostáva živý. 

To je výborné.

## Hlavný problém

Je tu jedno veľké riziko:

> **dávaš do transition príliš veľa inteligencie príliš skoro**

Konkrétne:

* komunitné thresholdy,
* per-topic AI odhady,
* adaptívne thresholdy,
* network consensus medzi AI. 

To je super dlhodobo. Ale pre pilot to môže byť zbytočne zložité.

## Čo by som zmenil

### 1. Pre pilot zaviesť „Transition Policy Profile“

Namiesto plne dynamických pravidiel.

Napríklad:

* `small-community`
* `default`
* `high-noise`

A hotovo.

To je stále flexibilné, ale omnoho ľahšie implementovateľné a auditovateľné.

### 2. Network consensus by som zjednodušil

Pre pilot by som sa možno ani nesnažil robiť AI consensus.

Jednoduchší model:

* system metrics + human confirmation

AI suggestion môže byť lokálna pomocná vrstva, ale nie jadro rozhodnutia.

### 3. Cooling-off je dobrý, ale treba explicitne povedať, čo sa stane s vytvoreným Project Node

* zmaže sa?
* ostane ako aborted?
* ide do archived?
* je to tombstone?

Toto musí byť definované.

## Verdikt

Výborný smer, ale už sa blížiš k hraničnej komplexite pre pilot.

### Najvyšší ROI úpravy

* Transition Policy Profiles namiesto plne dynamických thresholdov
* zjednodušiť consensus pre pilot
* definovať osud aborted projectu po cooling-off

---

# ADR-082 — Domain Validation Rules



## Čo je silné

Toto je jeden z najsilnejších a najpraktickejších dokumentov celej kapitoly E.
Máš tam:

* hard/soft/sync,
* strict/tolerant/revalidation,
* invariants vs policies,
* continuous validation,
* konkrétne pravidlá podľa typov. 

To je veľmi silné.

## Hlavný problém

Dokument je už veľmi dobrý, ale treba dať pozor na jeden typ budúcich problémov:

> **validation explosion**

Ak bude pribúdať veľa pravidiel bez organizácie, skončíš s:

* 100 pravidlami,
* ťažkou diagnózou,
* a drahým maintenance.

## Čo by som zmenil

### 1. Zaviesť stable rule IDs ako first-class contract

Už ich máš (`P1`, `T1`, `A1`...), čo je super. 
Toto treba udržať striktne a nikdy ich spätne nepreznačovať.

To je veľmi dôležité pre:

* logging,
* telemetry,
* UI error mapping,
* migrácie.

### 2. Validation rules rozdeliť na:

* storage admissibility
* domain consistency
* operational readiness

Lebo dnes sa tam mieša:

* čo sa smie uložiť,
* čo dáva zmysel,
* čo je pripravené na usage.

To je podobná línia ako pri ADR-077.

### 3. Revalidation scope držať explicitne malý

Máš to naznačené, ale odporúčam ešte tvrdšie povedať:

* nikdy nerevalidovať celý graf
* len impacted nodes
* plus limited periodic fallback

To je veľmi dôležité pre budúcu škálovateľnosť.

## Verdikt

Výborný dokument.
Toto je presne ten typ ADR, ktorý veľmi pomôže pilotu prežiť realitu.

### Najvyšší ROI úpravy

* nemenné rule IDs
* rozdelenie storage/domain/operational validity
* ešte tvrdší limit na scope revalidácie

---

# Celkový verdikt ku kapitole E

Táto kapitola je už na veľmi dobrej úrovni.
Najväčšia sila je, že sa ti podarilo skombinovať:

* filozofiu decentralizácie,
* realistický lifecycle,
* a začínajúcu implementačnú disciplínu.

Najväčšie riziko už nie je, že by to bolo príliš abstraktné.
Teraz je riziko skôr opačné:

> že sa pokúsiš implementovať príliš veľa inteligencie naraz.

Pre pilot by som preto držal túto líniu:

## čo je pripravené skoro

upravene * ADR-042
* ADR-075
skontrolovať, či je **ADR-076 plne zladené s touto novou verziou 042** * ADR-076
upravene * ADR-077
* ADR-082

## čo treba zjednodušiť pre pilot

upravene * ADR-022
+* ADR-023
* ADR-024
upravene * ADR-078
* ADR-026

---

# Najdôležitejšia veta na koniec

> Kvalitný domain model nie je ten, ktorý vie všetko.
> Kvalitný domain model je ten, ktorý sa dá dnes implementovať a zajtra nerozbiješ, keď pribudne realita.

Ak chceš, ďalší krok spravím veľmi prakticky:
môžem ti z tohto pripraviť **„Pilot Domain Model Profile“** — teda ktoré entity, polia, vzťahy a stavy naozaj zaviesť v prvej verzii a čo odložiť.
