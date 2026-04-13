# Zadanie pre Codex: Synergetikum Core Pilot

## Rola

Správaj sa ako senior solution architect a senior .NET MAUI developer.
Tvoj cieľ je navrhnúť a implementovať **pilotnú verziu systému Synergetikum**, ale iba jeho najmenšie jadro.

Nechcem plnú verziu celého Synergetika.
Nechcem P2P sieť, distribuované projekty, branching projektov ani collective query.

Chcem iba **pilot**, ktorý overí, či sa vie lokálna Personal AI učiť z otázok a odpovedí používateľa dostatočne rýchlo a zmysluplne.

---

# 1. Cieľ pilotu

Pilot musí overiť túto hypotézu:

> Z lokálnych otázok, odpovedí a správania používateľa sa dá v relatívne krátkom čase vytvoriť použiteľný osobný profil, ktorý začne meniť poradie ďalších otázok, perspektív a odporúčaní.

Tento pilot má byť:

* lokálny,
* jednoduchý,
* spustiteľný v .NET MAUI,
* písaný v C#,
* navrhnutý tak, aby sa neskôr dal rozšíriť na väčší systém.

---

# 2. Čo je Synergetikum v tomto pilote

V tomto pilote je Synergetikum:

> lokálna aplikácia s Personal AI profilom, ktorá kladie používateľovi otázky, zaznamenáva odpovede, učí sa z nich a následne prispôsobuje ďalšie otázky a interpretáciu používateľovi.

Toto je pilot na otestovanie:

* rýchlosti učenia,
* zmysluplnosti profilácie,
* kvality personalizácie,
* použiteľnosti reflexného loopu.

---

# 3. Čo pilot NEROBÍ

Do pilotu vedome nezahrňuj:

* P2P komunikáciu
* sync medzi zariadeniami
* parent node
* distributed project objects
* topic layer
* project formation
* branch lifecycle
* argument graph
* podporu viacerých používateľov
* cloud backend
* externé AI API ako povinnú súčasť
* veľké LLM modely ako jadro správania

Tento pilot musí byť navrhnutý tak, aby bol použiteľný **aj bez internetu**.

---

# 4. Hlavná funkcionalita pilotu

Pilot musí obsahovať týchto 5 základných funkčných blokov.

## 4.1 Question Feed

Používateľ dostáva otázky.

Otázky sú rozdelené do oblastí, napríklad:

* jedlo a služby
* komunita
* podnikanie
* lokálne problémy
* osobné preferencie
* spolupráca
* praktické vs systémové myslenie

Otázky môžu mať rôzne typy:

* single choice
* multi choice
* scale 1–5
* short text
* skip

## 4.2 Answer Capture

Aplikácia musí vedieť uložiť:

* odpoveď používateľa
* čas odpovede
* typ odpovede
* či otázku preskočil
* ako dlho mu trvalo odpovedať
* v akom kontexte odpovedal

## 4.3 Personal AI Profile

Aplikácia si lokálne vedie osobný profil používateľa.

Tento profil sa skladá aspoň z:

* Interest Profile
* Capability Profile
* Preference Profile
* Participation Profile
* Cognitive Modes Distribution
* Event Log
* Derived Snapshot

## 4.4 Adaptation Engine

Po každej sade odpovedí sa systém musí mierne upraviť.

Musí vedieť:

* meniť poradie ďalších otázok
* meniť poradie perspektív otázky
* zvyšovať váhu tém, ktoré používateľa reálne zaujímajú
* znižovať váhu tém, ktoré dlhodobo ignoruje
* občas zaradiť exploration otázku mimo komfortnej zóny

## 4.5 Reflection View

Používateľ musí vidieť, že sa systém učí.

Aplikácia musí zobrazovať:

* aké témy sa aktuálne javia ako silné
* aké typy otázok používateľ preferuje
* ktoré oblasti stúpajú alebo klesajú
* aké perspektívy systém zobrazuje častejšie
* stručné vysvetlenie: „prečo teraz vidíš tieto otázky“

---

# 5. Produktové správanie

## 5.1 Základný flow

Používateľ otvorí aplikáciu.
Dostane sadu otázok.
Odpovie.
Aplikácia aktualizuje profil.
Ďalšia sada otázok je jemne odlišná.

## 5.2 Reflexný flow

Po určitom počte odpovedí aplikácia ukáže:

* čo sa naučila
* čo zatiaľ nevie
* na čo by sa chcela používateľa spýtať ďalej

## 5.3 Explanation flow

Používateľ musí mať pocit, že systém:

* nie je black box
* nehodnotí ho
* len sa z jeho správania postupne učí

---

# 6. Architektonické princípy

## 6.1 Local-first

Všetko musí fungovať lokálne.

## 6.2 Offline-first

Aplikácia musí byť použiteľná bez internetu.

## 6.3 Human-in-the-loop

Systém nerobí rozhodnutia za používateľa.

## 6.4 Structured profile, not magic AI blob

Personal AI nesmie byť jeden nejasný blob.
Musí byť reprezentovaná ako kompozitný profil z menších objektov.

## 6.5 Evolučný model

Profil sa mení postupne.
Jedna odpoveď nesmie radikálne prepísať identitu používateľa.

## 6.6 Explainability

Každá adaptácia má byť aspoň stručne vysvetliteľná.

---

# 7. Odporúčaná architektúra aplikácie

Použi čistú, rozšíriteľnú architektúru vhodnú pre .NET MAUI.

Odporúčam:

* .NET MAUI
* C#
* MVVM
* SQLite ako lokálne úložisko
* oddelené vrstvy:

  * UI
  * Application
  * Domain
  * Infrastructure

Ak budeš navrhovať priečinkovú štruktúru, navrhni ju tak, aby sa dala neskôr rozšíriť o topic layer, project layer a P2P sync.

---

# 8. Požadované doménové modely

Navrhni a implementuj doménové modely aspoň pre tieto entity:

## 8.1 Question

Obsahuje napríklad:

* QuestionId
* Text
* TopicKey
* QuestionType
* PerspectiveType
* ComplexityLevel
* IsExplorationQuestion
* Tags
* Active

## 8.2 Answer

Obsahuje:

* AnswerId
* QuestionId
* UserId
* AnswerType
* Value
* IsSkipped
* AnsweredAt
* DurationMs

## 8.3 PersonalAiProfile

Obsahuje:

* UserId
* ProfileVersion
* CreatedAt
* UpdatedAt
* OverallConfidence

## 8.4 InterestSignal / InterestProfile

Obsahuje:

* TopicKey
* InterestScore
* Trend
* Confidence
* LastUpdated

## 8.5 CapabilitySignal / CapabilityProfile

Obsahuje:

* CapabilityKey
* CapabilityScore
* Confidence
* Source

## 8.6 PreferenceProfile

Obsahuje minimálne kontinuálne dimenzie:

* PracticalVsConceptual
* LocalVsSystemic
* ActionVsAnalysis
* RiskTolerance
* IndividualVsCollective

## 8.7 CognitiveModesDistribution

Obsahuje rozloženie typov uvažovania.

Dôležité:

* neukladaj to ako jednu úroveň
* ukladaj to ako distribúciu

Príklad dimenzií:

* Level1Operational
* Level2Tactical
* Level3Structural
* Level4Systemic
* Level5Conceptual
* Level6Abstract

Toto je interný model.
Nepoužívaj ho ako nálepku používateľa.

## 8.8 ParticipationProfile

Obsahuje:

* AverageResponseRate
* AverageCommitmentRate
* PreferredParticipationType
* DropoutTendency

## 8.9 PersonalAiEvent

Každá významná akcia musí vytvárať event, napríklad:

* QuestionAnswered
* QuestionSkipped
* InterestShiftDetected
* PreferenceShiftDetected
* ReflectionGenerated

## 8.10 PersonalAiSnapshot

Agregovaný runtime stav, z ktorého UI číta aktuálny profil.

---

# 9. Ukladanie dát

Použi SQLite.

Navrhni lokálne úložisko tak, aby boli oddelené:

* raw observations
* derived profile state
* event log
* snapshots

Nerob jeden veľký JSON blob ako jediný zdroj pravdy.

Chcem:

* auditovateľnosť
* možnosť rekonštrukcie
* možnosť evolúcie schémy

---

# 10. Learning model

Toto je jadro pilotu.

Nechcem veľké AI inferencie ani komplexný LLM orchestration.
Na pilot stačí:

* heuristický learning engine
* scoring model
* pravidlá
* jednoduché odvodenie trendov
* voliteľne neskôr rozšíriteľné o ONNX model

Learning engine musí vedieť:

## 10.1 Aktualizovať záujmy

Ak používateľ pravidelne odpovedá na určitú tému, jej váha rastie.
Ak ju ignoruje, váha klesá.

## 10.2 Aktualizovať preferencie

Z odpovedí a výberov odvodiť, či viac inklinuje k:

* praktickým otázkam
* systémovým otázkam
* akcii
* analýze

## 10.3 Aktualizovať kognitívne rozloženie

Na základe vzoru odpovedí odhadovať distribúciu typov myslenia.

## 10.4 Aktualizovať participation tendencies

Ak človek často len odpovedá, ale nezaväzuje sa, profil to musí odrážať.

## 10.5 Stabilizovať profil

Model nesmie skákať príliš prudko.
Použi postupné zmeny a smoothing.

---

# 11. Adaptation engine

Implementuj mechanizmus, ktorý po každom kole odpovedí vyberie ďalšie otázky.

Výber sa má riadiť kombináciou:

* relevance
* diversity
* exploration
* profile confidence

Systém musí:

* ukázať relevantné otázky
* neuzavrieť používateľa do bubliny
* občas ponúknuť nové uhly pohľadu

---

# 12. UI obrazovky

Navrhni a implementuj minimálne tieto obrazovky:

## 12.1 Home / Feed

* zoznam otázok
* jasné CTA na odpoveď

## 12.2 Question Detail

* samotná otázka
* odpoveď
* voliteľné preskočenie

## 12.3 Reflection / My AI

* čo sa o mne systém učí
* moje silné témy
* moje vzory odpovedania
* moje preferované perspektívy

## 12.4 History / Timeline

* história odpovedí
* história zmien profilu

## 12.5 Settings / Debug

* reset profilu
* export lokálnych dát
* prepnutie debug režimu
* zobrazenie confidence

UI má byť jednoduché, čisté a nie pretechnizované.

---

# 13. Debug & observability

Pre pilot je kritické vedieť, čo systém robí.

Implementuj debug vrstvu, kde bude vidieť:

* aktuálny profile snapshot
* posledné eventy
* zmeny interest score
* zmeny preference dimensions
* prečo bola vybraná konkrétna otázka

Toto je povinné.
Inak nebudeme vedieť, či sa systém naozaj učí.

---

# 14. In / Out scope

## IN

* lokálna .NET MAUI app
* otázky
* odpovede
* profile learning
* question reordering
* reflection view
* SQLite
* event log
* snapshot model

## OUT

* P2P
* sync
* multi-user
* cloud backend
* projects
* topics
* branches
* argument graphs
* parent node
* external LLM dependency ako jadro
* production security hardening beyond reasonable local app defaults

---

# 15. Dôležité obmedzenia

* Projekt musí byť písaný čisto a rozšíriteľne.
* Nepreťažuj prvú verziu overengineeringom.
* Nepoužívaj „AI“ ako zámienku na magické nejasné správanie.
* Každé správanie systému musí byť vysvetliteľné.
* Personal AI je v tomto pilote **structured evolving profile**, nie veľký black-box model.

---

# 16. Očakávaný výsledok od Codexu

Chcem, aby si:

1. navrhol architektúru riešenia,
2. navrhol priečinkovú štruktúru,
3. navrhol doménové modely,
4. navrhol storage model,
5. implementoval základnú funkčnú verziu aplikácie,
6. pridal seed otázky,
7. pridal debug / inspection obrazovku,
8. pripravil projekt tak, aby sa dal neskôr rozšíriť.

---

# 17. Seed otázky

Priprav aspoň počiatočný seed dataset otázok v niekoľkých oblastiach:

* jedlo a služby
* komunita
* podnikanie
* lokálne potreby
* osobné preferencie
* spolupráca
* praktické vs systémové otázky

Každá otázka má mať:

* topic
* perspective
* complexity
* tags

---

# 18. Ako budeme pilot hodnotiť

Pilot je úspešný, ak po krátkom čase používania vidíme:

* že sa otázky začnú personalizovať
* že sa profil používateľa mení zmysluplne
* že Reflection view dáva zmysel
* že používateľ cíti rozdiel medzi „náhodnou appkou“ a „profilujúcim sa systémom“

---

# 19. Čo od teba nechcem

* nechcem akademický experiment bez použiteľnej appky
* nechcem enterprise overengineering
* nechcem generický survey app builder
* nechcem len CRUD nad otázkami
* nechcem prázdny skeleton bez skutočného learning loopu

---

# 20. Finálna inštrukcia

Implementuj tento pilot ako čistý základ pre budúce Synergetikum.

Priorita je:

1. lokálna Personal AI profile learning slučka
2. čitateľnosť architektúry
3. vysvetliteľnosť systému
4. reálna použiteľnosť

Ak si budeš musieť vybrať medzi:

* „viac features“
* a „čistejší learning loop“

vyber vždy:

## 👉 **čistejší learning loop**

Ak chceš, viem ti teraz spraviť ešte aj druhú verziu:

* kratšiu a tvrdšiu, priamo ako **prompt pre Codex**
* alebo dlhšiu, ako **internú technickú špecifikáciu** pre tím.

[1]: https://openai.com/codex/?utm_source=chatgpt.com "Codex | AI Coding Partner from OpenAI"
