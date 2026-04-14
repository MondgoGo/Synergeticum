# Synergetikum Core Pilot

## Konkrétny zoznam súborov

---

# 1. Root

## `README.md`

**Úloha:** stručný technický úvod do projektu.

Má obsahovať:

* čo je Synergetikum Core Pilot
* čo pilot robí
* čo pilot nerobí
* ako spustiť appku
* stručný popis architektúry
* stručný popis learning loopu
* kde sú seed otázky
* kde je debug obrazovka

---

## `Synergetikum.sln`

**Úloha:** solution file.

Má obsahovať:

* hlavný MAUI projekt
* unit tests
* integration tests

---

# 2. App bootstrap

## `App.xaml`

**Trieda:** implicitná MAUI app resource definícia
**Úloha:** globálne resource dictionary.

Má obsahovať:

* app-level styles
* farby
* základné theme resources

---

## `App.xaml.cs`

**Trieda:** `App`
**Úloha:** štart aplikácie.

Má obsahovať:

* inicializáciu root page alebo shell
* minimum logiky

Metódy:

* constructor
* prípadne bootstrap navigácie

---

## `MauiProgram.cs`

**Trieda:** `MauiProgram`
**Úloha:** composition root.

Má obsahovať registráciu:

* repositories
* domain services
* use cases
* viewmodelov
* SQLite inicializácie
* debug services

Metódy:

* `CreateMauiApp()`

---

## `appsettings.json`

**Úloha:** konfiguračné hodnoty pilotu.

Má obsahovať napríklad:

* počet otázok v batche
* exploration rate
* thresholdy relevance
* debug flag
* seed source config

---

# 3. Common

## `Common/Constants/AppConstants.cs`

**Trieda:** `AppConstants`
**Úloha:** všeobecné konštanty.

Má obsahovať:

* default batch size
* min/max confidence
* default exploration ratio
* názov DB
* názov appky

---

## `Common/Constants/TopicKeys.cs`

**Trieda:** `TopicKeys`
**Úloha:** centralizované topic key stringy.

Má obsahovať:

* `FoodBusiness`
* `Community`
* `Entrepreneurship`
* `LocalNeeds`
* `Preferences`
* `Collaboration`
* `PracticalThinking`
* `SystemThinking`

---

## `Common/Constants/PerspectiveTypes.cs`

**Trieda:** `PerspectiveTypes`
**Úloha:** centralizované perspective typy.

Má obsahovať:

* `Practical`
* `Analytical`
* `Reflective`
* `Action`
* `Systemic`

---

## `Common/Constants/EventTypes.cs`

**Trieda:** `EventTypes`
**Úloha:** názvy typov eventov.

Má obsahovať:

* `QuestionAnswered`
* `QuestionSkipped`
* `ProfileSnapshotUpdated`
* `InterestShiftDetected`
* `PreferenceShiftDetected`
* `ReflectionGenerated`

---

## `Common/Enums/QuestionType.cs`

**Enum:** `QuestionType`

Hodnoty:

* `SingleChoice`
* `MultiChoice`
* `Scale`
* `ShortText`

---

## `Common/Enums/AnswerType.cs`

**Enum:** `AnswerType`

Hodnoty:

* `SingleChoice`
* `MultiChoice`
* `Scale`
* `Text`
* `Skipped`

---

## `Common/Enums/TrendDirection.cs`

**Enum:** `TrendDirection`

Hodnoty:

* `Unknown`
* `Rising`
* `Stable`
* `Falling`

---

## `Common/Enums/ParticipationType.cs`

**Enum:** `ParticipationType`

Hodnoty:

* `Passive`
* `Reflective`
* `ActiveReflective`
* `Committed`

---

## `Common/Results/Result.cs`

**Trieda:** `Result`
**Úloha:** ne-generický výsledok operácie.

Properties:

* `IsSuccess`
* `ErrorMessage`

Statické factory:

* `Success()`
* `Failure(string error)`

---

## `Common/Results/ResultT.cs`

**Trieda:** `Result<T>`
**Úloha:** generický výsledok operácie.

Properties:

* `IsSuccess`
* `Value`
* `ErrorMessage`

Statické factory:

* `Success(T value)`
* `Failure(string error)`

---

# 4. Domain / Entities

## `Domain/Entities/Question.cs`

**Trieda:** `Question`
**Úloha:** doménová entita otázky.

Properties:

* `QuestionId`
* `Text`
* `TopicKey`
* `QuestionType`
* `PerspectiveType`
* `ComplexityLevel`
* `IsExplorationQuestion`
* `IsActive`
* `Tags`
* `CreatedAt`

Metódy:

* `Deactivate()`
* `Activate()`

---

## `Domain/Entities/Answer.cs`

**Trieda:** `Answer`
**Úloha:** doménová entita odpovede.

Properties:

* `AnswerId`
* `QuestionId`
* `UserId`
* `AnswerType`
* `RawValue`
* `IsSkipped`
* `AnsweredAt`
* `DurationMs`

Metódy:

* `CreateSkipped(...)`
* `CreateAnswered(...)`

---

## `Domain/Entities/PersonalAiProfile.cs`

**Trieda:** `PersonalAiProfile`
**Úloha:** koreňový profil používateľa.

Properties:

* `UserId`
* `ProfileVersion`
* `CreatedAt`
* `UpdatedAt`
* `OverallConfidence`
* `ActiveContext`
* `InterestProfile`
* `CapabilityProfile`
* `PreferenceProfile`
* `ParticipationProfile`
* `CognitiveModesDistribution`

Metódy:

* `UpdateTimestamp()`
* `IncreaseVersion()`

---

## `Domain/Entities/PersonalAiEvent.cs`

**Trieda:** `PersonalAiEvent`
**Úloha:** event pre audit a evolúciu profilu.

Properties:

* `EventId`
* `UserId`
* `EventType`
* `Timestamp`
* `PayloadJson`
* `CorrelationId`

Metódy:

* `Create(...)`

---

## `Domain/Entities/PersonalAiSnapshot.cs`

**Trieda:** `PersonalAiSnapshot`
**Úloha:** agregovaný runtime stav profilu.

Properties:

* `UserId`
* `SnapshotVersion`
* `GeneratedAt`
* `TopTopics`
* `TopCapabilities`
* `PreferenceSummary`
* `CognitiveSummary`
* `ParticipationSummary`
* `ExplanationSummary`

Metódy:

* `Create(...)`

---

# 5. Domain / Value Objects

## `Domain/ValueObjects/InterestSignal.cs`

**Trieda:** `InterestSignal`
**Úloha:** signál záujmu o tému.

Properties:

* `TopicKey`
* `InterestScore`
* `Trend`
* `Confidence`
* `LastUpdated`

Metódy:

* `Increase(double delta)`
* `Decrease(double delta)`
* `SetTrend(TrendDirection trend)`

---

## `Domain/ValueObjects/CapabilitySignal.cs`

**Trieda:** `CapabilitySignal`
**Úloha:** odhad schopnosti prispieť.

Properties:

* `CapabilityKey`
* `CapabilityScore`
* `Confidence`
* `Source`
* `LastUpdated`

Metódy:

* `Adjust(double delta)`

---

## `Domain/ValueObjects/PreferenceDimensionSet.cs`

**Trieda:** `PreferenceDimensionSet`
**Úloha:** rozloženie rozhodovacích preferencií.

Properties:

* `PracticalVsConceptual`
* `LocalVsSystemic`
* `ActionVsAnalysis`
* `RiskTolerance`
* `IndividualVsCollective`

Metódy:

* `Normalize()`
* `ApplyDelta(...)`

---

## `Domain/ValueObjects/CognitiveModesDistribution.cs`

**Trieda:** `CognitiveModesDistribution`
**Úloha:** interné rozloženie typov uvažovania.

Properties:

* `Level1Operational`
* `Level2Tactical`
* `Level3Structural`
* `Level4Systemic`
* `Level5Conceptual`
* `Level6Abstract`

Metódy:

* `Normalize()`
* `ApplySignal(...)`

Poznámka pre Codex:

* musí byť validované, že hodnoty dávajú zmysel
* nesmie sa používať ako verejná nálepka používateľa

---

## `Domain/ValueObjects/ParticipationMetrics.cs`

**Trieda:** `ParticipationMetrics`
**Úloha:** vzorce zapojenia používateľa.

Properties:

* `AverageResponseRate`
* `AverageCommitmentRate`
* `PreferredParticipationType`
* `DropoutTendency`

Metódy:

* `UpdateFromAnswer(...)`
* `UpdateFromBehavior(...)`

---

## `Domain/ValueObjects/UserContext.cs`

**Trieda:** `UserContext`
**Úloha:** aktívny kontext používateľa.

Properties:

* `LocationScope`
* `Language`
* `TimeZone`
* `LastActiveAt`

---

# 6. Domain / Services

## `Domain/Services/ProfileLearningService.cs`

**Trieda:** `ProfileLearningService`
**Úloha:** hlavná doménová logika učenia profilu.

Má obsahovať logiku:

* aktualizácia interest profilu z odpovedí
* aktualizácia preference dimenzií
* aktualizácia cognitive modes
* smoothing a stabilizácia

Metódy:

* `ApplyAnswer(PersonalAiProfile profile, Question question, Answer answer)`
* `ApplySkip(...)`
* `RecalculateOverallConfidence(...)`

---

## `Domain/Services/QuestionSelectionService.cs`

**Trieda:** `QuestionSelectionService`
**Úloha:** výber ďalších otázok.

Má obsahovať logiku:

* relevance scoring
* exploration factor
* diversity balancing
* ordering podľa profilu

Metódy:

* `SelectNextBatch(...)`
* `ScoreQuestion(...)`
* `ApplyExploration(...)`

---

## `Domain/Services/ReflectionSummaryService.cs`

**Trieda:** `ReflectionSummaryService`
**Úloha:** vytvoriť čitateľné zhrnutie profilu.

Metódy:

* `BuildReflectionSummary(PersonalAiProfile profile)`
* `BuildTopTopicsSummary(...)`
* `BuildPreferenceSummary(...)`

---

## `Domain/Services/ParticipationInferenceService.cs`

**Trieda:** `ParticipationInferenceService`
**Úloha:** odvodiť participačné tendencie.

Metódy:

* `UpdateParticipationProfile(...)`
* `InferPreferredParticipationType(...)`
* `CalculateDropoutTendency(...)`

---

## `Domain/Services/ProfileSnapshotFactory.cs`

**Trieda:** `ProfileSnapshotFactory`
**Úloha:** zložiť snapshot z aktuálneho profilu.

Metódy:

* `CreateSnapshot(PersonalAiProfile profile, string explanationSummary)`

---

# 7. Domain / Rules

## `Domain/Rules/InterestUpdateRules.cs`

**Trieda:** `InterestUpdateRules`
**Úloha:** centralizované pravidlá zmien záujmov.

Má obsahovať:

* delta pri odpovedi
* delta pri ignorovaní
* min/max limity
* decay pravidlá

---

## `Domain/Rules/PreferenceUpdateRules.cs`

**Trieda:** `PreferenceUpdateRules`
**Úloha:** pravidlá aktualizácie preference dimenzií.

---

## `Domain/Rules/CognitiveModeRules.cs`

**Trieda:** `CognitiveModeRules`
**Úloha:** ako odpovede mapovať na kognitívne módy.

---

## `Domain/Rules/ExplorationRules.cs`

**Trieda:** `ExplorationRules`
**Úloha:** ako často a kedy zaradiť exploration otázky.

---

# 8. Domain / Interfaces

## `Domain/Interfaces/IQuestionRepository.cs`

**Interface:** `IQuestionRepository`

Metódy:

* `GetActiveQuestionsAsync()`
* `GetByIdAsync(string questionId)`
* `SaveAsync(Question question)`

---

## `Domain/Interfaces/IAnswerRepository.cs`

**Interface:** `IAnswerRepository`

Metódy:

* `SaveAsync(Answer answer)`
* `GetByUserAsync(string userId)`
* `GetRecentByUserAsync(string userId, int count)`

---

## `Domain/Interfaces/IPersonalAiProfileRepository.cs`

**Interface:** `IPersonalAiProfileRepository`

Metódy:

* `GetAsync(string userId)`
* `SaveAsync(PersonalAiProfile profile)`

---

## `Domain/Interfaces/IPersonalAiEventRepository.cs`

**Interface:** `IPersonalAiEventRepository`

Metódy:

* `AppendAsync(PersonalAiEvent evt)`
* `GetByUserAsync(string userId)`
* `GetRecentByUserAsync(string userId, int count)`

---

## `Domain/Interfaces/IPersonalAiSnapshotRepository.cs`

**Interface:** `IPersonalAiSnapshotRepository`

Metódy:

* `GetLatestAsync(string userId)`
* `SaveAsync(PersonalAiSnapshot snapshot)`

---

## `Domain/Interfaces/IClock.cs`

**Interface:** `IClock`

Metódy:

* `UtcNow`

---

# 9. Application / UseCases

## `Application/UseCases/GetNextQuestionBatchUseCase.cs`

**Trieda:** `GetNextQuestionBatchUseCase`
**Úloha:** vráti ďalší batch otázok.

Metódy:

* `ExecuteAsync(string userId, int batchSize)`

Má robiť:

* načítať profil
* načítať aktívne otázky
* použiť QuestionSelectionService
* vrátiť DTO

---

## `Application/UseCases/SubmitAnswerUseCase.cs`

**Trieda:** `SubmitAnswerUseCase`
**Úloha:** uložiť odpoveď a aktualizovať profil.

Metódy:

* `ExecuteAsync(AnswerSubmissionDto dto)`

Má robiť:

* načítať question
* vytvoriť Answer
* uložiť Answer
* vytvoriť PersonalAiEvent
* aktualizovať profile cez ProfileLearningService
* uložiť snapshot

---

## `Application/UseCases/GetReflectionViewUseCase.cs`

**Trieda:** `GetReflectionViewUseCase`
**Úloha:** dodať dáta pre obrazovku „Moja AI“.

Metódy:

* `ExecuteAsync(string userId)`

---

## `Application/UseCases/GetHistoryTimelineUseCase.cs`

**Trieda:** `GetHistoryTimelineUseCase`
**Úloha:** zobrazenie histórie.

Metódy:

* `ExecuteAsync(string userId)`

---

## `Application/UseCases/ResetProfileUseCase.cs`

**Trieda:** `ResetProfileUseCase`
**Úloha:** reset pilotného profilu.

Metódy:

* `ExecuteAsync(string userId)`

---

## `Application/UseCases/SeedQuestionsUseCase.cs`

**Trieda:** `SeedQuestionsUseCase`
**Úloha:** natiahnuť počiatočné otázky do DB.

Metódy:

* `ExecuteAsync()`

---

# 10. Application / DTOs

## `Application/DTOs/QuestionDto.cs`

**Trieda:** `QuestionDto`

Properties:

* `QuestionId`
* `Text`
* `TopicKey`
* `PerspectiveType`
* `QuestionType`
* `ComplexityLevel`
* `Tags`
* `WhyShown`

---

## `Application/DTOs/AnswerSubmissionDto.cs`

**Trieda:** `AnswerSubmissionDto`

Properties:

* `QuestionId`
* `UserId`
* `AnswerType`
* `RawValue`
* `IsSkipped`
* `DurationMs`

---

## `Application/DTOs/ReflectionViewDto.cs`

**Trieda:** `ReflectionViewDto`

Properties:

* `TopTopics`
* `TopCapabilities`
* `PreferenceSummary`
* `CognitiveSummary`
* `ParticipationSummary`
* `ExplanationSummary`

---

## `Application/DTOs/ProfileSnapshotDto.cs`

**Trieda:** `ProfileSnapshotDto`

Properties:

* `ProfileVersion`
* `UpdatedAt`
* `OverallConfidence`
* `InterestSignals`
* `PreferenceDimensions`
* `CognitiveModes`
* `ParticipationMetrics`

---

## `Application/DTOs/HistoryTimelineItemDto.cs`

**Trieda:** `HistoryTimelineItemDto`

Properties:

* `Timestamp`
* `Type`
* `Title`
* `Details`

---

# 11. Application / Mappers

## `Application/Mappers/QuestionMapper.cs`

**Trieda:** `QuestionMapper`
**Úloha:** mapovanie Question ↔ QuestionDto

---

## `Application/Mappers/ProfileMapper.cs`

**Trieda:** `ProfileMapper`
**Úloha:** mapovanie snapshot/profile ↔ DTO

---

## `Application/Mappers/HistoryMapper.cs`

**Trieda:** `HistoryMapper`
**Úloha:** mapovanie eventov na timeline itemy

---

# 12. Infrastructure / Persistence

## `Infrastructure/Persistence/DatabaseOptions.cs`

**Trieda:** `DatabaseOptions`
**Úloha:** konfigurácia DB.

Properties:

* `DatabasePath`

---

## `Infrastructure/Persistence/SqliteConnectionFactory.cs`

**Trieda:** `SqliteConnectionFactory`
**Úloha:** vytvára SQLite spojenia.

Metódy:

* `CreateConnection()`

---

## `Infrastructure/Persistence/DatabaseInitializer.cs`

**Trieda:** `DatabaseInitializer`
**Úloha:** vytvorenie tabuliek a inicializácia schémy.

Metódy:

* `InitializeAsync()`

Má vytvoriť tabuľky pre:

* questions
* answers
* personal_ai_profiles
* interest_signals
* capability_signals
* preference_profiles
* participation_profiles
* personal_ai_events
* personal_ai_snapshots

---

# 13. Infrastructure / Persistence / Records

## `Infrastructure/Persistence/Entities/QuestionRecord.cs`

**Trieda:** `QuestionRecord`
**Úloha:** DB record otázky.

---

## `Infrastructure/Persistence/Entities/AnswerRecord.cs`

**Trieda:** `AnswerRecord`

---

## `Infrastructure/Persistence/Entities/ProfileRecord.cs`

**Trieda:** `ProfileRecord`

---

## `Infrastructure/Persistence/Entities/InterestSignalRecord.cs`

**Trieda:** `InterestSignalRecord`

---

## `Infrastructure/Persistence/Entities/CapabilitySignalRecord.cs`

**Trieda:** `CapabilitySignalRecord`

---

## `Infrastructure/Persistence/Entities/PreferenceProfileRecord.cs`

**Trieda:** `PreferenceProfileRecord`

---

## `Infrastructure/Persistence/Entities/ParticipationProfileRecord.cs`

**Trieda:** `ParticipationProfileRecord`

---

## `Infrastructure/Persistence/Entities/EventRecord.cs`

**Trieda:** `EventRecord`

---

## `Infrastructure/Persistence/Entities/SnapshotRecord.cs`

**Trieda:** `SnapshotRecord`

---

# 14. Infrastructure / Repositories

## `Infrastructure/Persistence/Repositories/QuestionRepository.cs`

**Trieda:** `QuestionRepository`
**Implements:** `IQuestionRepository`

---

## `Infrastructure/Persistence/Repositories/AnswerRepository.cs`

**Trieda:** `AnswerRepository`
**Implements:** `IAnswerRepository`

---

## `Infrastructure/Persistence/Repositories/PersonalAiProfileRepository.cs`

**Trieda:** `PersonalAiProfileRepository`
**Implements:** `IPersonalAiProfileRepository`

---

## `Infrastructure/Persistence/Repositories/PersonalAiEventRepository.cs`

**Trieda:** `PersonalAiEventRepository`
**Implements:** `IPersonalAiEventRepository`

---

## `Infrastructure/Persistence/Repositories/PersonalAiSnapshotRepository.cs`

**Trieda:** `PersonalAiSnapshotRepository`
**Implements:** `IPersonalAiSnapshotRepository`

---

# 15. Infrastructure / Seed

## `Infrastructure/Seed/QuestionSeedLoader.cs`

**Trieda:** `QuestionSeedLoader`
**Úloha:** načíta otázky zo seed JSON.

Metódy:

* `LoadQuestionsAsync()`

---

## `Infrastructure/Seed/SeedBootstrapper.cs`

**Trieda:** `SeedBootstrapper`
**Úloha:** pri prvom spustení natiahne seed otázky.

Metódy:

* `BootstrapAsync()`

---

# 16. Infrastructure / Services

## `Infrastructure/Services/SystemClock.cs`

**Trieda:** `SystemClock`
**Implements:** `IClock`

---

## `Infrastructure/Services/DebugLogService.cs`

**Trieda:** `DebugLogService`
**Úloha:** pomocné debug logovanie pre pilot.

Metódy:

* `LogProfileChange(...)`
* `LogQuestionSelection(...)`
* `LogAnswerSubmission(...)`

---

## `Infrastructure/Services/ProfileExportService.cs`

**Trieda:** `ProfileExportService`
**Úloha:** export profilu a eventov do JSON.

Metódy:

* `ExportAsync(string userId)`

---

# 17. Presentation / ViewModels

## `Presentation/ViewModels/BaseViewModel.cs`

**Trieda:** `BaseViewModel`
**Úloha:** base class pre viewmodely.

Properties:

* `IsBusy`
* `Title`

Metódy:

* `SetProperty(...)`

---

# 18. Features / QuestionFeed

## `Features/QuestionFeed/QuestionFeedPage.xaml`

**Úloha:** zoznam ďalších otázok.

Obsah:

* list kariet otázok
* refresh / load batch
* info prečo sú otázky zobrazené

---

## `Features/QuestionFeed/QuestionFeedViewModel.cs`

**Trieda:** `QuestionFeedViewModel`

Properties:

* `Questions`
* `IsLoading`
* `EmptyStateMessage`

Metódy:

* `LoadAsync()`
* `RefreshAsync()`
* `OpenQuestionAsync(QuestionDto dto)`

---

# 19. Features / QuestionAnswering

## `Features/QuestionAnswering/QuestionDetailPage.xaml`

**Úloha:** detail otázky a vstup pre odpoveď.

---

## `Features/QuestionAnswering/QuestionDetailViewModel.cs`

**Trieda:** `QuestionDetailViewModel`

Properties:

* `Question`
* `SelectedAnswer`
* `ScaleValue`
* `TextValue`
* `CanSubmit`

Metódy:

* `LoadAsync(string questionId)`
* `SubmitAsync()`
* `SkipAsync()`

---

## `Features/QuestionAnswering/AnswerInputFactory.cs`

**Trieda:** `AnswerInputFactory`
**Úloha:** podľa typu otázky rozhodnúť, aký input komponent sa použije.

---

# 20. Features / Reflection

## `Features/Reflection/ReflectionPage.xaml`

**Úloha:** zobrazenie „Moja AI“.

---

## `Features/Reflection/ReflectionViewModel.cs`

**Trieda:** `ReflectionViewModel`

Properties:

* `Summary`
* `TopTopics`
* `TopCapabilities`
* `PreferenceCards`
* `CognitiveModeCards`
* `ParticipationInsights`

Metódy:

* `LoadAsync()`

---

# 21. Features / History

## `Features/History/HistoryPage.xaml`

**Úloha:** časová os odpovedí a eventov.

---

## `Features/History/HistoryViewModel.cs`

**Trieda:** `HistoryViewModel`

Properties:

* `Items`

Metódy:

* `LoadAsync()`

---

# 22. Features / Settings

## `Features/Settings/SettingsPage.xaml`

**Úloha:** nastavenia pilotu.

---

## `Features/Settings/SettingsViewModel.cs`

**Trieda:** `SettingsViewModel`

Properties:

* `IsDebugEnabled`
* `CurrentUserId`

Metódy:

* `ResetProfileAsync()`
* `ExportProfileAsync()`

---

# 23. Diagnostics

## `Diagnostics/ProfileInspectorPage.xaml`

**Úloha:** surový pohľad na profil.

---

## `Diagnostics/ProfileInspectorViewModel.cs`

**Trieda:** `ProfileInspectorViewModel`

Properties:

* `Snapshot`
* `InterestSignals`
* `Capabilities`
* `Preferences`
* `CognitiveModes`

Metódy:

* `LoadAsync()`

---

## `Diagnostics/EventLogPage.xaml`

**Úloha:** zoznam posledných eventov.

---

## `Diagnostics/EventLogViewModel.cs`

**Trieda:** `EventLogViewModel`

Properties:

* `Events`

Metódy:

* `LoadAsync()`

---

## `Diagnostics/QuestionSelectionDebugPage.xaml`

**Úloha:** vysvetlenie, prečo boli vybrané konkrétne otázky.

---

## `Diagnostics/QuestionSelectionDebugViewModel.cs`

**Trieda:** `QuestionSelectionDebugViewModel`

Properties:

* `CandidateQuestions`
* `SelectionScores`
* `ExplorationReasoning`

Metódy:

* `LoadAsync()`

---

# 24. TestsSupport

## `TestsSupport/FakeQuestionFactory.cs`

**Trieda:** `FakeQuestionFactory`
**Úloha:** generovanie testovacích otázok.

---

## `TestsSupport/FakeAnswerFactory.cs`

**Trieda:** `FakeAnswerFactory`
**Úloha:** generovanie testovacích odpovedí.

---

## `TestsSupport/TestProfileBuilder.cs`

**Trieda:** `TestProfileBuilder`
**Úloha:** tvorba profilov pre testy.

---

# 25. Unit Tests

## `tests/Synergetikum.UnitTests/Domain/ProfileLearningServiceTests.cs`

Má testovať:

* update interest signalov
* smoothing
* update preference dimenzií
* update cognitive modes

---

## `tests/Synergetikum.UnitTests/Domain/QuestionSelectionServiceTests.cs`

Má testovať:

* relevance scoring
* exploration insert
* diversity selection

---

## `tests/Synergetikum.UnitTests/Application/SubmitAnswerUseCaseTests.cs`

Má testovať:

* uloženie odpovede
* vytvorenie eventu
* update profilu
* uloženie snapshotu

---

## `tests/Synergetikum.UnitTests/Application/GetNextQuestionBatchUseCaseTests.cs`

Má testovať:

* batch selection
* ordering
* ignore inactive questions

---

# 26. Integration Tests

## `tests/Synergetikum.IntegrationTests/QuestionSeedIntegrationTests.cs`

Má testovať:

* seed loading
* validitu otázok

---

## `tests/Synergetikum.IntegrationTests/ProfilePersistenceIntegrationTests.cs`

Má testovať:

* uloženie a načítanie profilu
* konzistenciu snapshotu

---

## `tests/Synergetikum.IntegrationTests/AnswerSubmissionFlowTests.cs`

Má testovať:

* end-to-end flow:
  question → answer → profile update → snapshot

---

# 27. Najdôležitejšie pravidlá pre Codex

Použi ich doslova.

* Nedávaj business logiku do page code-behind.
* Nedávaj doménové rozhodovanie do repository.
* Nedávaj SQLite priamo do viewmodelov.
* Personal AI model nech je vysvetliteľný.
* Každá adaptácia profilu musí byť auditovateľná cez eventy.
* Reflection view musí byť čitateľná pre človeka, nielen debug výpis.
* Kognitívne módy reprezentuj ako distribúciu, nie ako jedinú nálepku.

---
