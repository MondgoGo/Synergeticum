# Návrh adresárovej štruktúry

```text
Synergetikum/
├── Synergetikum.sln
├── README.md
├── docs/
├── seed/
├── src/
│   └── Synergetikum.App/
│       ├── App.xaml
│       ├── App.xaml.cs
│       ├── MauiProgram.cs
│       ├── appsettings.json
│       ├── Resources/
│       ├── Platforms/
│       ├── Common/
│       ├── Domain/
│       ├── Application/
│       ├── Infrastructure/
│       ├── Presentation/
│       ├── Features/
│       ├── Diagnostics/
│       └── TestsSupport/
└── tests/
    ├── Synergetikum.UnitTests/
    └── Synergetikum.IntegrationTests/
```

---

# 1. Root priečinok

## `Synergetikum.sln`

Obsah:

* solution súbor pre celý projekt

Codex má:

* vytvoriť solution
* pridať hlavný MAUI projekt
* pridať unit test a integration test projekty

---

## `README.md`

Obsah:

* stručný popis projektu
* ako spustiť appku
* čo je pilot
* čo pilot robí a čo nerobí
* základná architektúra
* ako seedovať otázky
* ako zapnúť debug režim

Codex má:

* napísať README tak, aby nový developer za 5 minút pochopil projekt

---

# 2. Dokumentácia

## `docs/`

Obsah:

* interné technické dokumenty
* stručné ADR odkazy
* architektonické poznámky
* data model overview
* learning loop overview

Odporúčané súbory:

### `docs/architecture-overview.md`

* high-level architektúra pilotu
* vrstvy systému
* flow otázka → odpoveď → profil → ďalšia otázka

### `docs/domain-model.md`

* popis entít
* Question
* Answer
* PersonalAiProfile
* Snapshot
* Event

### `docs/learning-model.md`

* ako sa mení profil
* aké signály idú do interest/preference/cognitive modes

### `docs/out-of-scope.md`

* čo pilot zámerne nerieši

Codex má:

* vytvoriť krátke, vecné markdown dokumenty
* neopakovať UI texty, ale vysvetliť systém

---

# 3. Seed dáta

## `seed/`

Obsah:

* počiatočné otázky
* prípadne defaultné konfigurácie
* testovacie scenáre

Odporúčané súbory:

### `seed/questions.seed.json`

Obsah:

* zoznam štartovacích otázok
* každá otázka má:

  * id
  * text
  * topic
  * perspective
  * complexity
  * tags
  * active

### `seed/profile-defaults.json`

Obsah:

* defaultné hodnoty profilu
* neutrálny interest/profile state
* exploration weights

### `seed/sample-sessions.json`

Obsah:

* ukážkové odpovede používateľa
* vhodné pre testy a debug

Codex má:

* pripraviť realistický seed dataset
* nie generický lorem ipsum obsah

---

# 4. Hlavný app projekt

## `src/Synergetikum.App/`

Toto je hlavná MAUI aplikácia.

---

## `App.xaml`

Obsah:

* globálne resources
* theme bindings
* app-level štýly

Codex má:

* vytvoriť čistý základ
* bez zbytočne komplikovaného stylingu

---

## `App.xaml.cs`

Obsah:

* bootstrap appky
* štart root page / shell

Codex má:

* držať minimum logiky
* žiadna business logika

---

## `MauiProgram.cs`

Obsah:

* registrácia DI
* registrácia služieb
* SQLite
* repositories
* application services
* viewmodely
* debug services

Codex má:

* sem dať composition root
* žiadne doménové rozhodovanie

---

## `appsettings.json`

Obsah:

* konfiguračné hodnoty pilotu, napríklad:

  * počet otázok v jednej sade
  * exploration ratio
  * thresholdy pre relevance
  * debug flags

Codex má:

* pripraviť konfiguračný model
* nepchať magic numbers naprieč kódom

---

# 5. `Resources/`

## Účel

Štandardný MAUI priestor pre:

* fonty
* farby
* štýly
* obrázky
* raw JSON zdroje

Odporúčané podpriečinky:

### `Resources/Styles/`

* colors.xaml
* styles.xaml
* spacing / typography tokens

### `Resources/Raw/`

* seed otázky, ak ich nechceš mať v `seed/` mimo appky

Codex má:

* držať UI resource veci len tu
* seed JSON buď sem, alebo načítavať z embedded resource

---

# 6. `Platforms/`

Štandardný MAUI platform-specific kód.

Codex má:

* sem dávať len platformové veci
* nič z domény ani learning logiky

---

# 7. `Common/`

Toto je priestor pre zdieľané technické stavebné bloky.

Odporúčané podpriečinky:

## `Common/Constants/`

Obsah:

* konštanty
* názvy topicov
* názvy perspective typov
* event type strings
* defaultné limity

Súbory:

* `TopicKeys.cs`
* `PerspectiveTypes.cs`
* `EventTypes.cs`
* `AppConstants.cs`

---

## `Common/Enums/`

Obsah:

* malé enumy pre application/domain boundary

Súbory:

* `QuestionType.cs`
* `ParticipationType.cs`
* `TrendDirection.cs`
* `AnswerType.cs`

---

## `Common/Results/`

Obsah:

* Result pattern
* Operation result
* Validation result

Súbory:

* `Result.cs`
* `ResultT.cs`
* `ValidationResult.cs`

---

## `Common/Extensions/`

Obsah:

* malé utility extension methods
* bez business logiky

---

## `Common/Models/`

Obsah:

* drobné pomocné modely
* pagination-like requesty
* common metadata

Codex má:

* sem nedávať doménové entity
* len technické shared veci

---

# 8. `Domain/`

Najdôležitejšia vrstva.
Sem patrí **čo systém je**, nie ako sa zobrazuje alebo ukladá.

Odporúčané podpriečinky:

---

## `Domain/Entities/`

Obsah:

* hlavné doménové entity

Súbory:

### `Question.cs`

Má obsahovať:

* QuestionId
* Text
* TopicKey
* PerspectiveType
* Complexity
* Tags
* IsActive
* metadata o type otázky

### `Answer.cs`

Má obsahovať:

* AnswerId
* QuestionId
* UserId
* AnswerType
* RawValue
* IsSkipped
* AnsweredAt
* DurationMs

### `PersonalAiProfile.cs`

Má obsahovať:

* hlavnú identitu profilu
* verziu
* summary confidence
* referencie na subprofily

### `PersonalAiSnapshot.cs`

Má obsahovať:

* aktuálne agregované hodnoty
* state pripravený pre runtime/use cases

### `PersonalAiEvent.cs`

Má obsahovať:

* event id
* typ eventu
* timestamp
* payload metadata

---

## `Domain/ValueObjects/`

Obsah:

* menšie nemenné objekty

Súbory:

### `InterestSignal.cs`

* topic
* score
* confidence
* trend

### `CapabilitySignal.cs`

* capability
* score
* confidence
* source

### `PreferenceDimensionSet.cs`

* practical vs conceptual
* local vs systemic
* action vs analysis
* risk tolerance
* individual vs collective

### `CognitiveModesDistribution.cs`

* level1..level6
* validácia, že dáva zmysel rozdelenie

### `ParticipationMetrics.cs`

* response rate
* commitment tendency
* dropout tendency

Codex má:

* použiť value objects tam, kde to dáva doménový zmysel
* nespraviť z toho anemický chaos

---

## `Domain/Services/`

Obsah:

* čistá doménová logika bez závislosti na databáze/UI

Súbory:

### `ProfileLearningService.cs`

Má obsahovať:

* pravidlá aktualizácie profilu z odpovedí
* smoothing
* stabilizáciu signálov

### `QuestionSelectionService.cs`

Má obsahovať:

* výpočet relevancie otázok
* exploration vs relevance balans

### `ReflectionSummaryService.cs`

Má obsahovať:

* tvorbu stručného vysvetlenia, čo sa profil naučil

### `ParticipationInferenceService.cs`

Má obsahovať:

* odvodenie participation tendencies z histórie správania

Codex má:

* sem dať doménové rozhodovanie
* bez SQLite a bez UI závislostí

---

## `Domain/Rules/`

Obsah:

* explicitné pravidlá a thresholdy

Súbory:

* `InterestUpdateRules.cs`
* `PreferenceUpdateRules.cs`
* `ExplorationRules.cs`

---

## `Domain/Interfaces/`

Obsah:

* doménové kontrakty
* repository interfaces
* provider interfaces

Súbory:

* `IQuestionRepository.cs`
* `IAnswerRepository.cs`
* `IPersonalAiProfileRepository.cs`
* `IPersonalAiEventRepository.cs`
* `IClock.cs`

Codex má:

* interfaces definovať tu, implementácie až v Infrastructure

---

# 9. `Application/`

Táto vrstva rieši **use cases**.
Čiže čo systém robí krok po kroku.

Odporúčané podpriečinky:

---

## `Application/UseCases/`

Obsah:

* konkrétne scenáre aplikácie

Súbory:

### `GetNextQuestionBatchUseCase.cs`

* načítanie ďalšej dávky otázok
* použitie profile + selection service

### `SubmitAnswerUseCase.cs`

* uloženie odpovede
* vytvorenie eventu
* update profilu
* update snapshotu

### `GetReflectionViewUseCase.cs`

* zloženie dát pre obrazovku „Moja AI“

### `GetHistoryTimelineUseCase.cs`

* načítanie odpovedí a eventov pre timeline

### `ResetProfileUseCase.cs`

* reset pilotného profilu

Codex má:

* sem dať orchestration
* use case volá repository a doménové služby

---

## `Application/DTOs/`

Obsah:

* aplikačné DTO pre UI a use cases

Súbory:

### `QuestionDto.cs`

* data pre zobrazenie otázky

### `AnswerSubmissionDto.cs`

* payload pri odoslaní odpovede

### `ReflectionViewDto.cs`

* summary pre reflection screen

### `ProfileSnapshotDto.cs`

* zjednodušený export aktuálneho profilu

### `HistoryTimelineItemDto.cs`

* jeden riadok histórie

Codex má:

* držať DTO oddelené od doménových entít

---

## `Application/Mappers/`

Obsah:

* mapovanie Entity ↔ DTO

Súbory:

* `QuestionMapper.cs`
* `ProfileMapper.cs`
* `HistoryMapper.cs`

---

## `Application/Validators/`

Obsah:

* validácia vstupov use caseov

---

# 10. `Infrastructure/`

Sem patrí všetko, čo je technická implementácia.

Odporúčané podpriečinky:

---

## `Infrastructure/Persistence/`

Obsah:

* SQLite inicializácia
* DB connection factory
* migrations / schema setup

Súbory:

### `DatabaseInitializer.cs`

* vytvorenie DB/tabuliek pri štarte

### `SqliteConnectionFactory.cs`

* vytváranie spojení

### `DatabaseOptions.cs`

* cesta k DB a config

---

## `Infrastructure/Persistence/Entities/`

Obsah:

* persistence modely pre SQLite, ak ich chceš oddeliť od domain

Súbory:

* `QuestionRecord.cs`
* `AnswerRecord.cs`
* `ProfileRecord.cs`
* `EventRecord.cs`

Codex má:

* oddeliť persistence modely od domain, ak je to praktické

---

## `Infrastructure/Persistence/Repositories/`

Obsah:

* implementácie repository interfaces

Súbory:

* `QuestionRepository.cs`
* `AnswerRepository.cs`
* `PersonalAiProfileRepository.cs`
* `PersonalAiEventRepository.cs`

Každý súbor má obsahovať:

* CRUD / query logiku
* mapovanie z DB do domain modelu
* bez UI a bez doménového rozhodovania

---

## `Infrastructure/Seed/`

Obsah:

* seed loader pre otázky
* defaultné dáta

Súbory:

* `QuestionSeedLoader.cs`
* `SeedBootstrapper.cs`

---

## `Infrastructure/Serialization/`

Obsah:

* export/import profilu
* JSON serializácia debug snapshotu

Súbory:

* `ProfileExportService.cs`
* `JsonSerializationService.cs`

---

## `Infrastructure/Services/`

Obsah:

* technické služby

Súbory:

* `SystemClock.cs`
* `DeviceContextService.cs`
* `DebugLogService.cs`

---

# 11. `Presentation/`

Čisté UI shell a spoločné prezentačné veci.

Odporúčané podpriečinky:

---

## `Presentation/Navigation/`

Obsah:

* route names
* app navigation service

Súbory:

* `AppRoutes.cs`
* `NavigationService.cs`

---

## `Presentation/ViewModels/`

Obsah:

* base viewmodely
* shared state

Súbory:

* `BaseViewModel.cs`
* `ViewModelState.cs`

---

## `Presentation/Converters/`

Obsah:

* value converters pre UI

---

## `Presentation/Behaviors/`

Obsah:

* reusable MAUI správania

---

## `Presentation/Components/`

Obsah:

* malé opakovane použiteľné UI bloky

Napríklad:

* `QuestionCard.xaml`
* `TagChip.xaml`
* `ProfileMetricCard.xaml`

---

# 12. `Features/`

Sem by som dal vertikálne feature moduly UI, aby sa appka dala ľahko rozširovať.

Odporúčané podpriečinky:

---

## `Features/QuestionFeed/`

Súbory:

* `QuestionFeedPage.xaml`
* `QuestionFeedPage.xaml.cs`
* `QuestionFeedViewModel.cs`

Obsah:

* zobrazenie batchu otázok
* načítanie ďalších otázok

---

## `Features/QuestionAnswering/`

Súbory:

* `QuestionDetailPage.xaml`
* `QuestionDetailPage.xaml.cs`
* `QuestionDetailViewModel.cs`
* `AnswerInputFactory.cs`

Obsah:

* detail jednej otázky
* render rôznych answer typov
* submit

---

## `Features/Reflection/`

Súbory:

* `ReflectionPage.xaml`
* `ReflectionPage.xaml.cs`
* `ReflectionViewModel.cs`

Obsah:

* čo sa AI naučila
* top topics
* top preferences
* reasoning summary

---

## `Features/History/`

Súbory:

* `HistoryPage.xaml`
* `HistoryPage.xaml.cs`
* `HistoryViewModel.cs`

Obsah:

* odpovede
* event timeline
* zmeny profilu

---

## `Features/Settings/`

Súbory:

* `SettingsPage.xaml`
* `SettingsPage.xaml.cs`
* `SettingsViewModel.cs`

Obsah:

* reset
* export
* debug toggle

---

# 13. `Diagnostics/`

Toto je veľmi dôležité pre pilot.

Odporúčané súbory:

## `Diagnostics/ProfileInspectorPage.xaml`

Obsah:

* raw profile values
* interest signals
* preference dimensions
* cognitive distribution

## `Diagnostics/ProfileInspectorViewModel.cs`

Obsah:

* načítanie raw snapshotu a eventov

## `Diagnostics/EventLogPage.xaml`

Obsah:

* posledné eventy
* typy eventov
* timestampy

## `Diagnostics/QuestionSelectionDebugPage.xaml`

Obsah:

* prečo bola vybraná konkrétna otázka
* relevance score
* exploration factor

Codex má:

* toto spraviť poctivo
* bez diagnostiky nebudeš vedieť, čo sa deje

---

# 14. `TestsSupport/`

Pomocné veci pre testy a debug seedovanie.

Súbory:

* `FakeQuestionFactory.cs`
* `FakeAnswerFactory.cs`
* `TestProfileBuilder.cs`

Obsah:

* generovanie testovacích dát
* helpery na seed scenáre

---

# 15. `tests/Synergetikum.UnitTests/`

Testy domény a application vrstvy.

Odporúčané podpriečinky:

## `Domain/`

* `ProfileLearningServiceTests.cs`
* `QuestionSelectionServiceTests.cs`
* `ParticipationInferenceServiceTests.cs`

## `Application/`

* `SubmitAnswerUseCaseTests.cs`
* `GetNextQuestionBatchUseCaseTests.cs`
* `GetReflectionViewUseCaseTests.cs`

Codex má:

* prioritne testovať learning loop
* nie len CRUD

---

# 16. `tests/Synergetikum.IntegrationTests/`

Tu budú:

* SQLite integrácie
* seed bootstrap testy
* flow testy

Súbory:

* `QuestionSeedIntegrationTests.cs`
* `ProfilePersistenceIntegrationTests.cs`
* `AnswerSubmissionFlowTests.cs`

---

# 17. Pravidlá pre Codex, ako tvoriť súbory

Toto je dôležité. Môžeš to vložiť Codexu doslova.

## Každý súbor má mať:

* jasnú jednu zodpovednosť
* stručný file header comment, čo robí
* žiadne miešanie UI, domény a persistence v jednom súbore

## Každý ViewModel má:

* len UI orchestration
* volať use case, nie repository priamo

## Každý UseCase má:

* riešiť jeden scenár
* volať doménové služby
* ukladať cez repository interface

## Každá Domain Service má:

* čistú biznis logiku
* bez SQLite
* bez MAUI závislostí

## Každý Repository má:

* len persistence logiku
* bez doménových rozhodnutí

---

# 18. Najdôležitejšie súbory, ktoré musia vzniknúť ako prvé

Ak by mal Codex začať v správnom poradí, tak takto:

1. `Domain/Entities/Question.cs`
2. `Domain/Entities/Answer.cs`
3. `Domain/Entities/PersonalAiProfile.cs`
4. `Domain/ValueObjects/InterestSignal.cs`
5. `Domain/ValueObjects/PreferenceDimensionSet.cs`
6. `Domain/ValueObjects/CognitiveModesDistribution.cs`
7. `Domain/Services/ProfileLearningService.cs`
8. `Domain/Services/QuestionSelectionService.cs`
9. `Application/UseCases/SubmitAnswerUseCase.cs`
10. `Application/UseCases/GetNextQuestionBatchUseCase.cs`
11. `Infrastructure/Persistence/DatabaseInitializer.cs`
12. `Infrastructure/Persistence/Repositories/*`
13. `Features/QuestionFeed/*`
14. `Features/QuestionAnswering/*`
15. `Features/Reflection/*`
16. `Diagnostics/ProfileInspector*`

---

# 19. Môj tvrdý názor

Ak to dáš Codexu bez tejto štruktúry, veľmi pravdepodobne spraví:

* jeden obrovský projekt
* veľa logiky v page alebo viewmodeloch
* weak domain
* slabú diagnostiku

A potom sa v tom utopíš.

Táto štruktúra je dosť prísna, ale presne to potrebuješ.

---
