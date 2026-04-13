# Ako uložiť Personal AI tak, aby bola**

* pochopiteľná,
* rozšíriteľná,
* synchronizovateľná,
* auditovateľná,
* a zároveň nie príliš komplikovaná.

Poviem to priamo:

**Personal AI by som nereprezentoval ako jeden “magický model blob”.**
To je presne cesta do pekla.

Namiesto toho by som ju reprezentoval ako:

👉 **kompozitný profil z viacerých menších dátových objektov**

---

# 1. Základný princíp

Personal AI má mať 3 vrstvy reprezentácie:

## 1.1 Raw observations

To, čo sa reálne stalo:

* používateľ odpovedal,
* ignoroval otázku,
* podporil vetvu,
* zapojil sa do projektu.

## 1.2 Derived profile state

To, čo z toho systém odvodil:

* záujem o podnikanie je stredný,
* ochota commitnúť sa je nízka,
* preferuje praktické otázky pred systémovými.

## 1.3 Behavioral memory / history

Pamäť zmien v čase:

* ako sa profil menil,
* čo bolo krátkodobé vybočenie,
* čo je stabilný trend.

Teda nie:

❌ jeden objekt „user_model.bin“

Ale:

✅ sada menších objektov, ktoré sa skladajú do jedného Personal AI stavu.

---

# 2. Ako by som to rozdelil dátovo

Navrhol by som 6 hlavných DTO / storage objektov.

---

## 2.1 PersonalAIProfile

Toto je hlavný agregovaný objekt.

Je to „hlavička“ celej Personal AI.

Obsahuje:

* user_id
* profile_version
* created_at
* updated_at
* active_context
* confidence_level_overall

Jeho úloha:

* držať celkový stav profilu,
* referencovať ostatné časti.

### Príklad

```json
{
  "user_id": "user_miro_001",
  "profile_version": 17,
  "created_at": "2026-04-01T10:00:00Z",
  "updated_at": "2026-04-21T18:10:00Z",
  "active_context": {
    "location_scope": "Trnava",
    "language": "sk"
  },
  "confidence_level_overall": 0.68
}
```

---

## 2.2 InterestProfile

Mapa záujmov.

Obsahuje:

* topic_key
* interest_score
* trend
* confidence
* last_updated

### Príklad

```json
{
  "user_id": "user_miro_001",
  "interests": [
    {
      "topic_key": "food_business",
      "interest_score": 0.82,
      "trend": "rising",
      "confidence": 0.74,
      "last_updated": "2026-04-20T11:00:00Z"
    },
    {
      "topic_key": "community_projects",
      "interest_score": 0.77,
      "trend": "stable",
      "confidence": 0.69,
      "last_updated": "2026-04-20T11:00:00Z"
    },
    {
      "topic_key": "constitutional_topics",
      "interest_score": 0.41,
      "trend": "rising",
      "confidence": 0.38,
      "last_updated": "2026-04-19T13:40:00Z"
    }
  ]
}
```

---

## 2.3 CapabilityProfile

V čom vie človek prispieť.

Obsahuje:

* capability_key
* capability_score
* source
* confidence

### Príklad

```json
{
  "user_id": "user_miro_001",
  "capabilities": [
    {
      "capability_key": "community_initiation",
      "capability_score": 0.84,
      "source": "observed_behavior",
      "confidence": 0.77
    },
    {
      "capability_key": "project_direction",
      "capability_score": 0.72,
      "source": "question_responses",
      "confidence": 0.63
    },
    {
      "capability_key": "construction_execution",
      "capability_score": 0.18,
      "source": "unknown",
      "confidence": 0.22
    }
  ]
}
```

---

## 2.4 PreferenceProfile

Ako človek rozmýšľa.

Toto je veľmi dôležité, lebo práve to riadi poradie perspektív.

Obsahuje dimenzie typu:

* practical_vs_conceptual
* local_vs_systemic
* action_vs_analysis
* risk_tolerance
* individual_vs_collective

### Príklad

```json
{
  "user_id": "user_miro_001",
  "preferences": {
    "practical_vs_conceptual": 0.58,
    "local_vs_systemic": 0.51,
    "action_vs_analysis": 0.66,
    "risk_tolerance": 0.61,
    "individual_vs_collective": 0.57
  },
  "confidence": {
    "practical_vs_conceptual": 0.71,
    "local_vs_systemic": 0.49,
    "action_vs_analysis": 0.74,
    "risk_tolerance": 0.64,
    "individual_vs_collective": 0.56
  }
}
```

---

## 2.5 ParticipationProfile

Ako sa človek reálne zapája.

Toto je rozdiel medzi „myslí si“ a „robí“.

Obsahuje:

* average_response_rate
* average_commitment_rate
* preferred_participation_type
* topic_to_commitment ratios
* dropout tendency

### Príklad

```json
{
  "user_id": "user_miro_001",
  "participation": {
    "average_response_rate": 0.73,
    "average_commitment_rate": 0.31,
    "preferred_participation_type": "active_reflective",
    "dropout_tendency": 0.22
  },
  "topic_commitment_patterns": [
    {
      "topic_key": "community_projects",
      "response_score": 0.88,
      "commitment_score": 0.54
    },
    {
      "topic_key": "food_business",
      "response_score": 0.81,
      "commitment_score": 0.36
    }
  ]
}
```

---

## 2.6 ProfileEventLog

Toto je najdôležitejšia vrstva pre audit a evolúciu.

Každá významná vec, ktorá ovplyvní profil, je event.

Príklady eventov:

* question_answered
* question_ignored
* topic_followed
* branch_supported
* branch_committed
* role_accepted
* preference_shift_detected

### Príklad

```json
{
  "user_id": "user_miro_001",
  "events": [
    {
      "event_id": "paievt_001",
      "timestamp": "2026-04-18T10:10:00Z",
      "type": "question_answered",
      "payload": {
        "question_id": "q_1001",
        "topic_key": "food_business",
        "response_type": "supportive"
      }
    },
    {
      "event_id": "paievt_002",
      "timestamp": "2026-04-18T10:22:00Z",
      "type": "branch_committed",
      "payload": {
        "project_id": "proj_pizzeria_trnava_001",
        "branch_id": "branch_A",
        "commitment_type": "conceptual_support"
      }
    }
  ]
}
```

---

# 3. Ako by to malo byť uložené

Teraz k storage.

Ja by som to nedržal ako jeden JSON súbor.
Ja by som to držal ako:

👉 **viacero tabuliek / kolekcií / dokumentov**

To znamená:

* `personal_ai_profiles`
* `interest_profiles`
* `capability_profiles`
* `preference_profiles`
* `participation_profiles`
* `personal_ai_event_log`

Prečo?

Lebo:

* môžeš aktualizovať len časť profilu,
* nemusíš stále prepisovať celý objekt,
* vieš lepšie auditovať zmeny,
* lepšie sa to verzionuje.

---

# 4. Ako by to vyzeralo v DTO logike

Keby som to preložil do DTO vrstvy, videl by som aspoň tieto objekty:

* `PersonalAiProfileDto`
* `InterestProfileDto`
* `InterestSignalDto`
* `CapabilityProfileDto`
* `CapabilitySignalDto`
* `PreferenceProfileDto`
* `ParticipationProfileDto`
* `PersonalAiEventDto`

A dôležité:

* `PersonalAiSnapshotDto`

To je objekt, ktorý zloží všetko dokopy pre runtime.

---

# 5. Snapshot vs event log

Toto je rovnaký princíp ako pri projektoch.

## Event log

Je pravda o tom, čo sa stalo.

## Snapshot

Je pohodlný aktuálny stav.

Obe vrstvy chceš.

Prečo?

Lebo:

* snapshot je rýchly na použitie,
* event log je dobrý na rekonštrukciu a audit.

---

# 6. Čo by sa určite nemalo ukladať

Toto je veľmi dôležité.

Nemal by si ukladať:

## 6.1 Surové „psychologické nálepky“

Napríklad:

* „tento človek je slabý“
* „tento človek je manipulovateľný“
* „je to taký typ osobnosti“

To je nebezpečné a zbytočné.

## 6.2 Jedno absolútne skóre osobnosti

Niečo typu:

* `user_level = 84`

To je nezmysel a vedie to k zlým rozhodnutiam.

## 6.3 Príliš jemné citlivé odvodenia

Ak sa to nedá obhájiť a používateľ by sa pri tom cítil creeped out, nemá to v profile čo robiť.

---

# 7. Ako to zapadá do Synergetika

Tento model slúži na:

## 7.1 Question ordering

Ktoré otázky ukázať a v akom poradí.

## 7.2 Perspective ordering

Ktorý uhol pohľadu ukázať ako prvý.

## 7.3 Participation filtering

Kde má zmysel, aby sa človek zapojil.

## 7.4 Opportunity matching

Čo by mohol robiť v projekte.

## 7.5 Reflection

Aké reflexné otázky mu položiť.

---

# 8. Môj odporúčaný prístup

Keby som to mal zjednodušiť do jedného odporúčania:

👉 **Personal AI reprezentuj ako event-sourced profil zložený z menších doménových profilov a jedného agregovaného snapshotu.**

To je podľa mňa najzdravšie.

Nie je to:

* príliš tvrdé,
* nie je to magický black box,
* a dá sa to rozvíjať.

---

# 9. Najväčšie riziko

Najväčšie riziko nie je technické. Je produktové.

Ak budete zbierať príliš veľa profilových premenných:

* stratíte sa,
* systém bude creepy,
* a nikto mu nebude veriť.

Začni s málo dimenziami.
Naozaj málo.

---

# 10. Praktický záver

Keby som to mal uzavrieť veľmi stručne:

## Personal AI dátovo =

* identita profilu
* záujmy
* schopnosti
* preferencie myslenia
* participačné vzorce
* event log
* snapshot

To je celé jadro.
