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

## 15. Zhrnutie

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
