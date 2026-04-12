# Personal AI Micro-Federation

## 1. Cieľ systému

Navrhovaný systém je postavený na princípe, že každý používateľ vlastní svoj osobný AI model, ktorý sa učí primárne z jeho vlastných dát. Zdieľanie učenia medzi ľuďmi nie je povinné ani globálne. Je selektívne, modulárne a riadené používateľom.

Cieľom nie je vytvoriť jeden centrálny model pre všetkých. Cieľom je vytvoriť sieť osobných modelov, ktoré sa môžu podľa povolení a dôveryhodných vzťahov občas synchronizovať a zdieľať časť znalostí.

Tým vzniká architektúra, ktorá kombinuje:

* vysokú personalizáciu,
* silnejšie súkromie,
* možnosť lokálneho učenia na mobile,
* občasný synchronizačný mechanizmus medzi dôveryhodnými uzlami,
* modulárne rozšírenie cez viac typov modelov.

Táto architektúra je realistickejšia než plne otvorený peer-to-peer tréning medzi neznámymi ľuďmi a zároveň decentralizovanejšia než klasický centralizovaný federated learning model.

---

## 2. Základné princípy

### 2.1 Personal model first

Osobný model používateľa je nadradený všetkým externým updateom. Externé synchronizačné mechanizmy slúžia ako pomocná vrstva, nie ako autorita.

### 2.2 Selective sync

Synchronizácia je voliteľná. Nedeje sa automaticky len preto, že je peer dostupný. Musí spĺňať pravidlá používateľa, technické podmienky a dôvernostný model.

### 2.3 Modular sharing

Nie všetky typy modelov sa správajú rovnako. Vision, text, health a behavior moduly majú odlišné nároky na synchronizáciu, dôveru a citlivosť dát. Preto sa každý model federuje zvlášť.

### 2.4 Conservative merge

Nie každý externý update sa prijme. Každý prichádzajúci update musí prejsť validačnou bránou. Lokálny model sa nesmie zhoršiť len preto, že skupina poslala nový checkpoint.

### 2.5 Trust circles instead of open federation

Systém nepredpokladá otvorenú verejnú federáciu. Predpokladá dôveryhodné okruhy používateľov, napríklad priatelia, rodina, tematické skupiny alebo pracovné kruhy.

### 2.6 Episodic coordination

Koordinácia nie je permanentná. Sync prebieha v konkrétnych oknách, typicky len na Wi‑Fi, pri dostatočnej batérii alebo počas nabíjania, a ideálne len v idle stave.

---

## 3. Režimy používania

Na zníženie chaosu a technickej zložitosti systém používa tri pevné režimy.

### 3.1 Solo

Používateľ si buduje model výlučne lokálne. Nezdieľa sa nič. Žiadne sync operácie s inými používateľmi neprebiehajú.

Použitie:

* maximálne súkromie,
* citlivé moduly,
* konzervatívni používatelia,
* počiatočný default pri prvom spustení.

### 3.2 Trusted Circle

Používateľ sa synchronizuje len s whitelist skupinou dôveryhodných peerov. Synchronizácia môže byť obmedzená len na vybrané moduly a len za vybraných podmienok.

Použitie:

* priatelia,
* rodina,
* tematické mikro-komunity,
* úzke pracovné alebo experimentálne kruhy.

### 3.3 Community Assist

Používateľ povoľuje obmedzené zdieľanie menej citlivých znalostných artefaktov, nie plných osobných dát ani nutne plných model updateov. Cieľom nie je priame spoluvlastníctvo modelu, ale pomocné zdieľanie vzorov.

Použitie:

* širšie komunity,
* menej citlivé moduly,
* knowledge sharing bez silného zásahu do osobného modelu.

---

## 4. Typy modelov

Architektúra ráta so štyrmi hlavnými modelovými modulmi. Každý modul má vlastnú životnosť, vlastnú synchronizačnú logiku a vlastný federated loop.

### 4.1 Vision model

Odporúčaný model:

* MobileNetV3 Small

Úloha:

* spracovanie obrazových vstupov,
* klasifikácia objektov,
* feature extraction,
* vizuálne signály z kamery alebo fotografií.

Približná veľkosť:

* cca 2.5–5 MB po mobilnej optimalizácii

Git projekty / zdroje:

* TensorFlow MobileNetV3 Small
* Open Model Zoo MobileNetV3 Small model card

Federácia:

* áno, ale len medzi rovnakými vision architektúrami

Odporúčaný režim:

* Solo alebo Trusted Circle

### 4.2 Text model

Odporúčaný model:

* TinyBERT

Úloha:

* intent klasifikácia,
* krátke texty,
* tagovanie,
* ľahké NLP nad textovým vstupom.

Približná veľkosť:

* cca 5–15 MB podľa variantu a kvantizácie

Git projekty / zdroje:

* Huawei Noah Pretrained Language Models
* TinyBERT branch / README

Federácia:

* áno, ale opatrne kvôli citlivosti textu

Odporúčaný režim:

* Solo alebo Trusted Circle

### 4.3 Health model

Odporúčaný model:

* LightGBM alebo malý MLP

Úloha:

* tabuľkové a biometrické dáta,
* denné agregácie,
* risk scoring,
* klasifikácie a anomálie.

Približná veľkosť:

* cca 100 KB – 2 MB

Git projekty / zdroje:

* LightGBM

Federácia:

* áno, veľmi vhodný kandidát na skorý federovaný rollout

Odporúčaný režim:

* Solo ako default, Trusted Circle pre kompatibilné skupiny

### 4.4 Behavior model

Odporúčaný model:

* small GRU, LSTM alebo 1D CNN

Úloha:

* sekvenčné dáta,
* návyky,
* časové patterny,
* behaviorálne trendové skóre.

Približná veľkosť:

* cca 200 KB – 2 MB

Git projekty / zdroje:

* PyTorch examples
* ONNX Runtime pre deployment a mobile inferenciu

Federácia:

* áno, ale až po dobrom zvládnutí health modulu

Odporúčaný režim:

* Solo alebo Trusted Circle

---

## 5. Prečo viac modelov namiesto jedného

Použitie viacerých malých modelov je výhodnejšie než jeden veľký monolitický model.

Dôvody:

* lepšia mobilná použiteľnosť,
* menšie modely sa ľahšie trénujú lokálne,
* jednoduchšia modularita,
* rôzne moduly môžu mať rôzne pravidlá zdieľania,
* jednoduchšie debugovanie,
* jednoduchší rollout,
* vyššia odolnosť proti systémovým chybám.

Architektonické pravidlo:

* vision federuje s vision,
* text federuje s text,
* health federuje s health,
* behavior federuje s behavior.

Nie je vhodné na začiatku miešať rozdielne architektúry do jedného spoločného federated procesu.

---

## 6. Celková architektúra systému

### 6.1 On-device vrstva

Každý mobil obsahuje:

* VisionModel
* TextModel
* HealthModel
* BehaviorModel
* FusionLayer
* LocalFeatureStore
* LocalModelStore
* SyncManager
* ValidationGate
* PermissionLayer
* TrustGraph

Úlohy tejto vrstvy:

* lokálna inferencia,
* lokálne jemné dolaďovanie modelu,
* správa povolení,
* správa trusted peerov,
* plánovanie synchronizácie,
* lokálne vyhodnotenie, či externý update zlepšuje alebo zhoršuje stav.

### 6.2 Federated coordination vrstva

Namiesto jedného centrálneho federátora systém používa logicky oddelené synchronizačné okruhy:

* Vision Federator
* Text Federator
* Health Federator
* Behavior Federator

Tieto môžu bežať rôzne:

* na ľahkom koordinátore,
* na dočasnom leader uzle,
* v rotating leader režime,
* v malom trusted meshi.

### 6.3 Fusion vrstva

Fusion layer spája výstupy jednotlivých modelov do jedného finálneho výsledku.

Môže spracúvať:

* skóre,
* confidence,
* embeddings,
* trendové indikátory,
* risk levely,
* modulové alerty.

Na začiatok má byť fusion vrstva jednoduchá:

* pravidlá,
* logistická regresia,
* prípadne malý MLP.

Fusion layer by sa v prvej fáze nemal federovať. Je to nadstavbová rozhodovacia vrstva a príliš skorá federácia by skôr priniesla chaos než hodnotu.

---

## 7. Synchronizačný model

### 7.1 Kedy sa sync spúšťa

Synchronizácia sa odporúča len vtedy, keď sú splnené technické podmienky:

* Wi‑Fi only,
* charging = true alebo battery nad zvolený threshold,
* ideálne idle stav,
* dostupný trusted peer alebo trusted skupina,
* dostatok nových dát na meaningful update.

### 7.2 Čo sa synchronizuje

Podľa modulu sa môže synchronizovať:

* model update,
* selected layers,
* embeddings,
* logits,
* distilled knowledge,
* anonymizované agregované štatistiky,
* model version metadata,
* sample count,
* lokálna validačná kvalita.

### 7.3 Čo sa nesynchronizuje

Nevhodné na synchronizáciu:

* raw osobné dáta,
* plné event logy,
* necenzurované citlivé texty,
* všetko automaticky bez validačnej brány.

### 7.4 Merge politika

Každý externý update musí byť posudzovaný konzervatívne.

Odporúčané pravidlá:

* peer musí byť trusted,
* model version musí byť kompatibilná,
* update musí byť dostatočne kvalitný,
* lokálny acceptance test sa nesmie zhoršiť,
* merge influence musí rešpektovať user policy.

---

## 8. Typy koordinácie

### 8.1 Central lightweight coordinator

Najjednoduchší model. Koordinátor nie je vlastník globálnej inteligencie, len synchronizačný bod.

Výhody:

* najnižšia implementačná bolesť,
* jednoduché verzie,
* ľahšia diagnostika.

Nevýhody:

* menšia decentralizácia.

### 8.2 Rotating leader

Počas sync okna sa jeden mobil alebo vybraný uzol dočasne stane leaderom.

Výhody:

* zachovanie decentralizačného ducha,
* nižšia závislosť na jednom serveri,
* dobrý kompromis pre trusted circles.

Nevýhody:

* zložitejšia leader election,
* vyššia citlivosť na nestabilné zariadenia.

### 8.3 Full peer-to-peer coordination

Každý uzol môže koordinovať a agregovať.

Výhody:

* maximálna decentralizácia,
* žiadny permanentný koordinátor.

Nevýhody:

* najvyššia komplexita,
* zložité verzie, trust, merge a konvergencia,
* nevhodné ako počiatočný produktový default.

Odporúčanie:

* začať lightweight coordinator alebo rotating leader,
* full P2P nechať až ako neskoršiu výskumnú vrstvu.

---

## 9. Permission a trust architektúra

Každý používateľ potrebuje mať jasne definované, čo povoľuje a s kým.

### 9.1 Permission layer

Definuje:

* ktoré moduly sa môžu synchronizovať,
* aký typ artefaktov možno zdieľať,
* za akých podmienok sa sync aktivuje,
* akú silu môžu mať externé updatey.

### 9.2 Trust graph

Definuje:

* kto je trusted peer,
* do akej skupiny peer patrí,
* akú váhu má peer pri merge,
* ktoré synchronizačné kruhy sú povolené.

### 9.3 Odporúčané permission režimy

Na zníženie chaosu je vhodné mať prednastavené režimy namiesto nekonečnej voľnosti.

Príklad:

* Solo
* Trusted Circle
* Community Assist

Pokročilí používatelia môžu mať advanced settings, ale jadro UX by malo zostať jednoduché.

---

## 10. Shared foundation a personal delta

Odporúčaný architektonický princíp je rozdeliť modely na dve vrstvy.

### 10.1 Shared foundation

* spoločný základný model,
* mení sa zriedkavo,
* môže byť výsledkom trusted sync mechanizmu.

### 10.2 Personal delta

* lokálna osobná nadstavba,
* vzniká z osobných dát,
* zostáva pod kontrolou používateľa,
* nesmie byť ľahko prepísaná externým updateom.

Tento princíp chráni personalizáciu modelu. Namiesto toho, aby si používatelia navzájom prepisovali celé modely, zdieľajú sa len časti alebo jemné zlepšenia základu.

---

## 11. Odporúčané technológie

### 11.1 Mobile runtime

* TFLite pre vision alebo malé modely
* ONNX Runtime Mobile pre širší cross-platform deployment

### 11.2 Federated orchestration

* Flower ako najpraktickejší štartovací federated framework

### 11.3 Model zdroje

* MobileNetV3 Small
* TinyBERT
* LightGBM
* PyTorch examples + ONNX Runtime pre behavior sekvenčné modely

### 11.4 App vrstva

* Flutter alebo .NET MAUI podľa širšieho ekosystému produktu
* lokálny storage napr. SQLite
* background jobs pre tréning a sync scheduling

---

## 12. Riziká a slabé miesta

### 12.1 Heterogénne dáta

Aj v trusted circle môžu mať používatelia veľmi rozdielne dáta. Shared update nemusí pomáhať každému.

### 12.2 Falošný pocit kolektívnej inteligencie

Malá skupina peerov nemusí produkovať významné zlepšenie. Hlavná hodnota systému ostáva lokálna personalizácia.

### 12.3 Produktová zložitosť

Príliš veľa voľnosti v permissions a merge politike vedie ku chaosu, ťažkému debugovaniu a nejasnému správaniu systému.

### 12.4 Versioning a rollback

Bez dobrej správy verzií sa systém rozpadne. Každý modul musí mať jasné versioning pravidlá, acceptance test a rollback mechanizmus.

### 12.5 Bezpečnosť a poisoning

Aj trusted circles môžu byť kompromitované alebo neúmyselne šíriť nekvalitné updatey. Preto je potrebný validation gate a minimálne sanity checks.

---

## 13. Odporúčaný rollout

### Fáza 1

* Health model
* jednoduchý Behavior model
* Solo režim
* lokalne učenie bez zdieľania
* jednoduchá fusion layer

Cieľ:

* stabilný on-device základ,
* pochopiť dátové toky,
* získať personal value bez federácie.

### Fáza 2

* Trusted Circle sync pre Health model
* jednoduchý coordinating node
* acceptance test a rollback

Cieľ:

* otestovať mikro-federáciu na najjednoduchšom module.

### Fáza 3

* pridať Behavior sync
* vyladiť trust graph a merge rules

### Fáza 4

* pridať Vision model
* skúsiť obmedzený sync vo trusted groups

### Fáza 5

* pridať Text model, ale veľmi opatrne kvôli citlivosti

Toto poradie minimalizuje technický dlh a zvyšuje šancu, že systém bude mať reálnu hodnotu ešte predtým, než sa pustí do najzložitejších modulov.

---

## 14. Návrh adresárovej a modulovej architektúry

```text
app/
  models/
    vision/
    text/
    health/
    behavior/
    fusion/
  sync/
    sync_manager/
    peer_discovery/
    leader_election/
    merge_engine/
    validation_gate/
  permissions/
    permission_layer/
    trust_graph/
    sharing_policies/
  data/
    feature_store/
    model_store/
    local_db/
  runtime/
    background_jobs/
    battery_wifi_checks/
    device_state/
  ui/
    settings/
    sync_mode/
    trust_circle/
    model_status/
```

---

## 15. P2P architektúra

Táto sekcia rozpracúva decentralizovanú synchronizačnú vrstvu detailnejšie. Cieľom nie je plne otvorená anonymná peer-to-peer sieť, ale kontrolovaná mikro-federácia medzi dôveryhodnými zariadeniami.

### 15.1 Charakter P2P siete

Navrhovaná P2P vrstva má tieto vlastnosti:

* uzly sú známe alebo vopred schválené,
* synchronizácia nie je permanentná, ale epizodická,
* sieť nemusí byť stále online,
* každý peer je primárne osobný klient a iba niekedy sa podieľa na koordinácii,
* P2P vrstva je doplnkom k osobným modelom, nie ich náhradou.

To znamená, že nejde o klasickú otvorenú P2P sieť ako verejný blockchainový mesh. Ide skôr o trust-aware sync mesh.

### 15.2 Typy uzlov

Každé zariadenie môže vystupovať v jednej alebo viacerých rolách:

#### Personal Client Node

Základná rola každého zariadenia.

Úlohy:

* lokálna inferencia,
* lokálny tréning,
* ukladanie osobných dát,
* rozhodovanie o tom, čo smie byť zdieľané.

#### Sync Peer Node

Uzol, ktorý vie prijímať a odosielať synchronizačné artefakty v trusted circle.

Úlohy:

* publikovanie vlastných model updateov,
* prijímanie updateov od dôveryhodných peerov,
* výmena verzií a metadát.

#### Temporary Coordinator Node

Dočasne zvolený uzol, ktorý počas konkrétneho sync okna koordinuje výmenu updateov.

Úlohy:

* zozbieranie updateov,
* vykonanie agregácie,
* rozoslanie agregovaného checkpointu,
* vedenie session state pre dané sync okno.

#### Relay Node

Voliteľná rola pre peer, ktorý sprostredkuje komunikáciu medzi časťami trusted siete, ak nie sú priamo viditeľné.

Táto rola sa má používať opatrne a len ak je sieť väčšia alebo členovia nie sú priamo dostupní.

### 15.3 Preferovaná P2P topológia

Odporúčaná topológia pre prvé fázy nie je full mesh medzi všetkými zariadeniami naraz, ale:

* small trusted mesh,
* star-with-rotating-leader,
* alebo clustered circles.

#### Small Trusted Mesh

Malá skupina 3–10 peerov, ktorí sa navzájom poznajú a majú whitelist spojení.

Výhody:

* jednoduchý trust model,
* nižšia komplexita,
* menšie riziko poisoning útokov.

#### Star with Rotating Leader

Na každé sync okno sa zvolí jeden leader. Všetci ostatní peeri komunikujú primárne s ním.

Výhody:

* jednoduchšia agregácia,
* nižšia sieťová zložitosť,
* stále decentralizovanejšie než fixný server.

#### Clustered Circles

Viac trusted circles, ktoré si primárne syncujú lokálne modely vo vnútri skupiny. Medzi skupinami sa zdieľa len veľmi obmedzená vrstva znalostí.

Tento model je vhodný pre neskoršiu fázu škálovania.

### 15.4 Peer discovery

Objavovanie peerov musí byť konzervatívne a bezpečné.

Odporúčaný návrh:

* manuálne pozvanie peerov,
* QR onboarding alebo invitation token,
* lokálne objavenie cez LAN / Wi‑Fi v trusted prostredí,
* možnosť centrálneho bootstrap directory len pre zistenie dostupnosti peerov, nie pre ukladanie dát.

Peer discovery nesmie automaticky znamenať dôveru. Objavenie peeru je len technický krok. Dôvera musí byť explicitne potvrdená.

### 15.5 Trust establishment

Každý peer má:

* device identity,
* user identity,
* group membership,
* kryptografický kľúč alebo podpisový identifikátor,
* úroveň dôvery.

Minimálny trust model:

* peer je explicitne schválený,
* peer patrí do povolenej skupiny,
* peer podpisuje updatey,
* peer má kompatibilnú verziu modelového modulu.

### 15.6 Leader election

Keďže koordinácia je epizodická, leader election je dôležitá.

Odporúčané pravidlá pre výber leadera:

* zariadenie je na Wi‑Fi,
* zariadenie sa nabíja alebo má vysokú batériu,
* zariadenie je idle,
* zariadenie má dostatok výpočtových zdrojov,
* zariadenie je trusted a má kompatibilnú verziu,
* zariadenie bolo nedávno online a má dobrú reputáciu v skupine.

Jednoduchá stratégia:

* deterministic ranking podľa device score,
* fallback na ďalší peer, ak leader odpadne.

### 15.7 Sync session flow

Navrhovaný priebeh jednej synchronizačnej session:

1. Peer discovery nájde dostupných trusted peerov.
2. Permission layer overí, či je sync pre daný modul povolený.
3. Leader election vyberie dočasného koordinátora.
4. Každý peer pošle session metadata:

   * model version,
   * module type,
   * local sample count,
   * quality score,
   * capability info.
5. Leader rozhodne, ktorí peeri sú eligible pre daný sync cyklus.
6. Eligible peeri pošlú model artifacts podľa režimu:

   * full module update,
   * selected layers,
   * embeddings,
   * distilled outputs,
   * anonymized statistics.
7. Leader vykoná agregáciu alebo knowledge merge.
8. Leader rozpošle candidate checkpoint.
9. Každý peer vykoná local validation gate.
10. Ak candidate checkpoint prejde, peer ho prijme alebo čiastočne merge-ne.
11. Session sa ukončí a zapíšu sa výsledky syncu.

### 15.8 P2P synchronizačné artefakty

P2P vrstva nemá byť obmedzená len na plné váhové updatey.

Možné artefakty:

* module weights,
* selected layer deltas,
* embeddings,
* logits,
* prototypes,
* class centroids,
* anonymized summary statistics,
* distilled teacher outputs,
* quality metrics,
* compatibility metadata.

Toto je dôležité pre flexibilitu medzi Solo, Trusted Circle a Community Assist režimami.

### 15.9 P2P bezpečnostná vrstva

Každá sync session musí mať minimálne:

* peer authentication,
* signed payloads,
* version checks,
* session nonce alebo anti-replay mechanizmus,
* update size limits,
* validation gate pred prijatím modelu.

Voliteľne:

* trust score peerov,
* penalizácia peerov s nekvalitnými updateami,
* reputačný systém v trusted circle,
* soft quarantine pre problematické uzly.

### 15.10 P2P limity a realistické hranice

P2P architektúra je vhodná:

* pre malé trusted circles,
* pre epizodické synchronizácie,
* pre silno personalizované modely,
* pre prípady, kde súkromie a autonómia majú vysokú prioritu.

P2P architektúra nie je vhodná ako prvý krok pre:

* masívne otvorené siete,
* vysokofrekvenčný sync,
* veľké foundation modely,
* real-time coordination medzi nestabilnými mobilmi.

Základné odporúčanie ostáva:

* začať s mikro-federáciou,
* držať malé trusted circles,
* používať rotating leader model,
* full open P2P nechať mimo prvých verzií.

---

## 16. Návrh implementácie

Táto sekcia prekladá koncept do implementačnej kostry. Cieľom nie je detailný zdrojový kód, ale dostatočne konkrétny návrh komponentov, tokov a zodpovedností.

### 16.1 Implementačné princípy

* mobile-first,
* offline-first,
* modular first,
* permission-driven sync,
* deterministic versioning,
* conservative acceptance of external updates.

### 16.2 Logické komponenty

#### A. Model Runtime Layer

Zodpovednosť:

* loading modelov,
* inference,
* lokálny tréning,
* export a import model artifacts.

Komponenty:

* VisionRuntime
* TextRuntime
* HealthRuntime
* BehaviorRuntime
* FusionRuntime

#### B. Data and Feature Layer

Zodpovednosť:

* lokálne ukladanie vstupov,
* transformácia raw dát na feature sety,
* správa dataset windows pre tréning.

Komponenty:

* LocalEventStore
* FeatureExtractor
* FeatureCache
* TrainingWindowBuilder
* ModelStore

#### C. Permission and Trust Layer

Zodpovednosť:

* vyhodnotenie pravidiel zdieľania,
* správa trusted circle,
* peer whitelist a group membership.

Komponenty:

* PermissionEngine
* TrustGraphService
* SharingPolicyResolver
* PeerRegistry

#### D. Sync and P2P Layer

Zodpovednosť:

* peer discovery,
* session orchestration,
* leader election,
* exchange payloadov,
* merge a validation.

Komponenty:

* SyncManager
* PeerDiscoveryService
* SessionCoordinator
* LeaderElectionService
* PayloadSerializer
* MergeEngine
* ValidationGate
* RollbackManager

#### E. Versioning and Observability Layer

Zodpovednosť:

* správa verzií modelov,
* audit sync session,
* diagnostika problémov.

Komponenty:

* ModelVersionRegistry
* SyncAuditLog
* QualityMetricsStore
* ExperimentFlags

### 16.3 Odporúčaná implementačná štruktúra

```text
core/
  models/
    vision/
    text/
    health/
    behavior/
    fusion/
  runtime/
    model_runtime/
    training/
    inference/
    export_import/
  data/
    local_event_store/
    feature_extractor/
    training_windows/
    feature_cache/
    model_store/
  permissions/
    permission_engine/
    trust_graph/
    peer_registry/
    sharing_policy/
  sync/
    sync_manager/
    peer_discovery/
    session_coordinator/
    leader_election/
    payloads/
    merge_engine/
    validation_gate/
    rollback/
  versioning/
    model_versions/
    checkpoints/
    audit_logs/
  platform/
    battery_network_state/
    background_jobs/
    device_capabilities/
  ui/
    sync_modes/
    trusted_circle/
    privacy_controls/
    model_status/
```

### 16.4 Kľúčové dátové entity

#### UserModelProfile

Obsah:

* user_id,
* active_mode,
* enabled_modules,
* trust settings,
* sharing preferences,
* merge preferences.

#### ModuleCheckpoint

Obsah:

* module_type,
* version,
* parent_version,
* artifact_type,
* created_at,
* source_type,
* quality_metrics,
* compatibility_metadata.

#### PeerIdentity

Obsah:

* peer_id,
* device_id,
* public_key,
* trust_level,
* group_ids,
* capability_flags,
* last_seen.

#### SyncSession

Obsah:

* session_id,
* module_type,
* leader_peer_id,
* participant_peer_ids,
* start_time,
* end_time,
* sync_mode,
* result,
* accepted_checkpoint_version,
* rollback_flag.

#### SharingPolicy

Obsah:

* module_type,
* share_mode,
* allowed_groups,
* artifact_types_allowed,
* battery_threshold,
* wifi_required,
* charging_required,
* validation_requirements.

### 16.5 Implementačný flow na zariadení

#### Lokálny tréning

1. LocalEventStore zbiera údaje.
2. FeatureExtractor ich transformuje na modulové features.
3. TrainingWindowBuilder vytvorí tréningový input.
4. Model runtime vykoná lokálny fine-tuning.
5. Výsledok sa uloží ako nový local checkpoint.
6. QualityMetricsStore zaznamená lokálnu kvalitu.

#### Lokálna inferencia

1. Modul dostane vstup.
2. Runtime vráti prediction alebo embedding.
3. FusionRuntime spojí výstupy.
4. UI alebo ďalšia logika použije finálny výsledok.

#### Sync rozhodovanie

1. SyncManager zistí device state.
2. PermissionEngine overí sharing policy.
3. PeerDiscoveryService nájde trusted peers.
4. Ak sú splnené podmienky, SessionCoordinator spustí sync session.

### 16.6 Implementačný flow pri Trusted Circle syncu

1. SyncManager spustí peer discovery.
2. PeerRegistry vráti len whitelist peerov.
3. LeaderElectionService vyberie leadera.
4. SessionCoordinator vytvorí SyncSession.
5. Participant peers pošlú eligibility metadata.
6. Leader vyhodnotí kompatibilitu.
7. Peerom požiada payloady podľa povoleného artifact typu.
8. MergeEngine vykoná agregáciu alebo merge.
9. Candidate checkpoint sa pošle účastníkom.
10. ValidationGate spraví local acceptance test.
11. Ak test prejde, RollbackManager uloží predchádzajúci checkpoint a nový sa aktivuje.
12. Audit log zaznamená session výsledok.

### 16.7 Merge stratégie

#### Strategy A: Full module averaging

Použitie:

* len keď ide o rovnaký model,
* malé trusted skupiny,
* nízka heterogenita.

#### Strategy B: Weighted merge

Použitie:

* keď chceš zohľadniť:

  * sample count,
  * trust score,
  * validation quality,
  * freshness.

#### Strategy C: Knowledge merge

Použitie:

* embeddings,
* logits,
* distillation outputs,
* centroids.

#### Strategy D: Foundation plus personal delta update

Použitie:

* shared foundation sa opatrne vylepší,
* personal delta ostáva lokálna.

Odporúčanie:

* začať s B alebo D,
* A používať len tam, kde je to technicky čisté,
* C zaviesť neskôr pre Community Assist.

### 16.8 Validation gate

Každý externý checkpoint musí prejsť cez lokalny validačný mechanizmus.

ValidationGate má overiť:

* kompatibilitu verzie,
* platnosť podpisu,
* rozumnú veľkosť payloadu,
* základné sanity metrics,
* nezhoršenie lokálneho validačného skóre,
* súlad s PermissionEngine pravidlami.

Ak checkpoint neprejde:

* odmietnuť ho,
* zalogovať dôvod,
* ponechať lokálny model nezmenený.

### 16.9 Rollback mechanizmus

Pred prijatím externého checkpointu treba:

* uložiť predchádzajúci aktívny checkpoint,
* označiť candidate checkpoint ako pending,
* po úspešnom validačnom teste ho aktivovať,
* pri probléme sa vrátiť na predchádzajúcu verziu.

Rollback je povinný, nie voliteľný.

### 16.10 Odporúčaný implementačný rollout

#### Iterácia 1

* Health model,
* Solo režim,
* lokálne checkpointy,
* fusion layer len minimálna alebo žiadna.

#### Iterácia 2

* Trusted Circle pre Health,
* peer registry,
* Wi‑Fi + battery gating,
* manual session start alebo nightly sync.

#### Iterácia 3

* rotating leader,
* validation gate,
* rollback,
* audit log.

#### Iterácia 4

* Behavior model,
* foundation + personal delta princíp,
* weighted merge.

#### Iterácia 5

* Vision modul,
* richer sync payloads,
* community assist artifacts.

#### Iterácia 6

* Text modul,
* jemnejšie privacy a sharing policies,
* experimenty s knowledge merge.

### 16.11 UX návrh implementácie režimov

Používateľ nemá vidieť komplexitu federácie. Má si vybrať jednoduchý režim.

#### Solo UI

* "Učiť sa len zo mňa"
* žiadne zdieľanie
* všetko ostáva lokálne

#### Trusted Circle UI

* "Zdieľať len s vybranými ľuďmi"
* správa okruhu dôvery
* výber modulov, ktoré sa môžu synchronizovať

#### Community Assist UI

* "Pomáhať komunite anonymne"
* len obmedzené artefakty
* nie plné osobné model dáta

Advanced settings môžu existovať, ale nesmú byť defaultnou cestou.

### 16.12 Technologické odporúčania pre implementáciu

* model runtimes držať oddelene podľa modulu,
* sync protokol mať modulovo agnostický, ale typovo bezpečný,
* payloady serializovať explicitne s version metadata,
* peer identity riešiť kryptograficky,
* platform state abstrahovať cez BatteryNetworkState service,
* observability logovať po moduloch aj po sessionách.

### 16.13 Implementačné rozhodnutia s vysokým ROI

Najvyšší ROI majú tieto rozhodnutia:

* začať health modulom,
* zaviesť Solo a Trusted Circle skôr než Community Assist,
* rotating leader až po basic coordinator flow,
* validation gate a rollback zaviesť skoro,
* personal delta nikdy nenechať prepísať bez ochrany,
* permissions držať v jednoduchých režimoch a nie v stovkách granular settings.

---

## 17. Zhrnutie

Navrhovaný systém stojí na týchto pilieroch:

* osobné modely sú primárne,
* synchronizácia je selektívna a modulárna,
* dôvera je organizovaná cez trusted circles,
* koordinácia je epizodická a mobile-friendly,
* federácia prebieha po moduloch, nie cez jeden monolit,
* fusion layer spája výsledky až na vyššej úrovni,
* rollout má ísť od najjednoduchších a najpraktickejších modulov.

Najsilnejšie jadro tejto architektúry je:

**osobné modely, selektívne učenie, dôveryhodné kruhy, konzervatívny merge**

To je realistický kompromis medzi decentralizovanou víziou a produktovou realizovateľnosťou.
