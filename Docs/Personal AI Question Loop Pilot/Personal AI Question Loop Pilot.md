# **Personal AI Question Loop Pilot**

Jednoúčelový pilot, kde:

* používateľ dostáva otázky
* odpovedá
* systém si vedie interný profil
* systém po každej sade odpovedí mení:

  * poradie ďalších otázok
  * poradie perspektív
  * návrhy tém
* používateľ vidí, že sa systém „učí jeho štýl“

Toto je podľa mňa najlepší prvý pilot.

---

# Prečo práve tento

Lebo testuje to najdôležitejšie:

## 1. Vie sa model učiť rýchlo?

Po 20–50 odpovediach už máš vedieť vidieť posun.

## 2. Vieš to spraviť celé lokálne?

Bez siete, bez syncu, bez infra bordelu.

## 3. Dá sa to potom znovu použiť?

Áno. Toto bude jadro Synergetika aj neskôr.

## 4. Je to v MAUI realistické?

Áno. Toto je veľmi realistické.

---

# Čo by pilot nerobil

Pilot by zámerne nerobil:

* P2P sync
* collective query
* topic layer
* project formation
* distributed project object
* argument graph
* parent node
* branch lifecycle

Nie preto, že sú zlé.
Ale preto, že teraz by ťa zabili.

---

# Čo by pilot robil

Pilot by mal len 5 funkčných blokov.

## 1. Question Feed

Používateľ dostane otázky.

Typy otázok:

* záujmové
* rozhodovacie
* hodnotové
* jednoduché praktické
* trochu systémové

## 2. Answer Capture

Používateľ odpovie:

* výber z možností
* škála
* krátky text
* možno „preskočiť“

## 3. Personal AI Profile

Systém si vedie lokálny profil:

* interests
* preferences
* cognitive modes
* participation tendency

## 4. Adaptation Engine

Po každom bloku odpovedí systém upraví:

* ktoré otázky dá ďalej
* ktoré perspektívy dá prvé
* ktoré témy zdvihne vyššie

## 5. Reflection View

Používateľ vidí:

* čo sa o ňom systém zatiaľ naučil
* aké otázky teraz preferuje
* čo sa zmenilo oproti včerajšku

Toto je kritické. Inak nebudeš vedieť, či sa model učí alebo len zbiera dáta.

---

# Konkrétny cieľ pilotu

Pilot by nemal cieľ „dokázať celý Synergetikum“.

Má mať oveľa menší cieľ:

> Overiť, či sa z opakovaných odpovedí dá v krátkom čase vytvoriť použiteľný lokálny model preferencií a spôsobu myslenia, ktorý zmysluplne mení ďalšiu interakciu.

To je veľmi silný a čistý experiment.

---

# Ako by som ho produktovo zarámoval

Napríklad takto:

## Názov pilotu

**Synergetikum Core Pilot**
alebo
**Personal AI Learning Pilot**

## Čo používateľ robí

* otvorí appku
* dostane 10 otázok denne
* odpovie
* systém mu ukáže:

  * čo sa naučil
  * čo mu odporúča skúmať ďalej
  * ktoré pohľady mu dáva častejšie

## Čo vy testujete

* ako rýchlo sa profil stabilizuje
* či personalizácia pôsobí zmysluplne
* či používateľ cíti, že „AI ma chápe“
* či sa otázky po pár dňoch začnú správať inteligentnejšie

---

# Aké dáta by pilot ukladal

Stačí minimum.

## Raw

* question_id
* answer
* timestamp
* skipped / answered

## Derived

* interest weights
* preference dimensions
* cognitive mode distribution
* confidence per dimension

## UX

* čo bolo ukázané
* čo bolo kliknuté
* čo bolo ignorované

To stačí na veľmi slušný prvý pilot.

---

# Ako by som to navrhol v C# / MAUI svete

Nie kód, len smer.

## Frontend

* .NET MAUI app
* jednoduché obrazovky:

  * feed otázok
  * detail otázky
  * moje AI zhrnutie
  * história posunu

## Local storage

* SQLite
* oddelené tabuľky pre:

  * questions
  * answers
  * profile state
  * profile events

## AI / model layer

Na pilot by som nešiel do veľkého LLM modelu.

Skôr by som použil:

* pravidlá
* scoring
* lightweight heuristiky
* voliteľne malý ONNX model neskôr

Na test rýchlosti učenia Personal AI nepotrebuješ veľký model.
Potrebuješ dobrý profilovací engine.

To je veľmi dôležité.

---

# Čo presne by ste tým overili

Toto sú metriky, ktoré by ma zaujímali.

## 1. Profil stabilization speed

Po koľkých odpovediach sa začnú meniť preferencie zmysluplne.

## 2. Perceived relevance

Má používateľ pocit, že ďalšie otázky sú viac relevantné?

## 3. Behavioral consistency

Sú odpovede dostatočne stabilné, aby z nich vznikol profil?

## 4. Exploration tolerance

Znesie používateľ aj mierne „provokačné“ otázky mimo svojej zóny?

## 5. Reflection usefulness

Dáva používateľovi niečo spätné zrkadlo od AI?

---

# Čo by som do pilotu pridal, aby bol užitočný aj neskôr

Tu je dôležité ROI myslenie.

Vyberal by som len veci, ktoré prežijú aj do veľkého systému.

## Určite áno

* question object model
* answer object model
* personal profile model
* profile event log
* preference dimensions
* cognitive modes distribution
* question ordering engine

## Skôr nie

* network
* branches
* projects
* sync
* collective query

Lebo tie teraz nepotrebuješ na odpoveď na tvoju hlavnú otázku.

---

# 3 možné piloty a ktorý by som zvolil

## Pilot A: Personal AI Question Loop

To, čo som opísal vyššie.

### Výhody

* najčistejšie testuje jadro
* najnižší scope
* veľmi dobré ROI

### Nevýhody

* netestuje kolektívnu vrstvu

### Môj výber

**Áno, toto by som zobral.**

---

## Pilot B: Mini Topic Pilot

Používateľ dá otázku a malá skupina ľudí odpovedá, bez projektov.

### Výhody

* testuje začiatok kolektívnej vrstvy

### Nevýhody

* bez kvalitného Personal AI jadra to bude plytké

### Môj názor

Až ako druhý pilot.

---

## Pilot C: Mini Project Pilot

Otázka → diskusia → malý projekt

### Výhody

* blízko vízii

### Nevýhody

* príliš skoro
* príliš veľa moving parts

### Môj názor

Zatiaľ nie.

---

# Moje finálne odporúčanie

Ak chceš pilot, ktorý:

* je realistický
* nezabije vás
* dá sa robiť v C# + MAUI
* otestuje jadro
* a bude znovupoužiteľný

tak zober:

# **Pilot 1: Local Personal AI Learning Loop**

## Scope

* 1 používateľ
* lokálne dáta
* otázky a odpovede
* profilovanie
* adaptívne poradie ďalších otázok
* jednoduché AI zhrnutie profilu

## Goal

* zistiť, či sa Personal AI začne „správať inteligentne“ už po malej dávke interakcií

---

# Jedna veľmi dôležitá vec

Na pilot by som úplne zahodil slovo „umelá inteligencia“, ak to budeš komunikovať tímu príliš skoro.

Prečo?

Lebo ľudia si predstavia LLM, modely, inferenciu, GPU.

Ale tvoj pilot je v skutočnosti test:

👉 **adaptívneho osobného modelu preferencií**

To je presnejšie a zdravšie.

---

# Finálna veta

👉 Ak chcete otestovať Synergetikum bez toho, aby vás zabilo, netestujte sieť. Testujte, či sa vie zrodiť Personal AI z otázok a odpovedí.
