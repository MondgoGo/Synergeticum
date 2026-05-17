# 🧾 ADR-076 — Example Flow

## Project Activation (end-to-end)

---

## 1. 📌 Status

**Accepted (Reference implementation flow)**

---

## 2. 🎯 Cieľ

Ukázať kompletný tok:

> **Topic → Project (forming) → Project (active)**

cez:

* Node (storage)
* Projection (read model)
* Command (write model)
* Validation
* Policy
* Sync-ready správanie

---

## 3. 🧠 Scenár (realita)

Používateľ:

1. sleduje Topic
2. zapojí sa (commitment)
3. pridá sa druhý participant
4. systém navrhne aktiváciu
5. používateľ potvrdí

👉 výsledok: Project prejde do `active`

---

## 4. 📦 Počiatočný stav (Node layer)

### 4.1 Topic Node

```json id="topic_node"
{
  "id": "topic-123",
  "type": "Topic",
  "payload": {
    "title": "Ako zlepšiť spánok",
    "interest_score": 0.7
  },
  "relations": [
    { "type": "PARTICIPANT", "targetId": "user-A" },
    { "type": "PARTICIPANT", "targetId": "user-B" }
  ]
}
```

---

### 4.2 Commitment Node (User A)

```json id="commitment_node"
{
  "id": "commitment-1",
  "type": "Commitment",
  "payload": {
    "type": "explicit",
    "expires_at": "2026-05-01"
  },
  "relations": [
    { "type": "PERSON", "targetId": "user-A" },
    { "type": "TOPIC", "targetId": "topic-123" }
  ]
}
```

---

## 5. 🔄 Projection layer (read model)

### 5.1 TopicSummary

```text id="topic_summary"
TopicSummary:
- Id: topic-123
- InterestScore: 0.7
- ParticipantCount: 2
- CommitmentCount: 1
```

---

### 5.2 TopicDomainView

```text id="topic_domain"
TopicDomainView:
- IsCandidateForProject = true
- HasEnoughParticipants = true
- HasCommitment = true
```

👉 **POZOR**

Toto NIE JE rozhodnutie.
Len **structural readiness**.

---

## 6. 🤖 Local AI suggestion (voliteľné)

```text id="ai_suggestion"
Personal AI:
"Táto téma vyzerá pripravená na projekt."
```

👉 lokálne, neovplyvňuje systém

---

## 7. 👤 User action

Používateľ klikne:

```text
[Navrhnúť projekt]
```

---

## 8. ⚙️ Command layer

### 8.1 Command

```text id="cmd_create_project"
CreateProjectFromTopicCommand:
- TopicId: topic-123
- InitiatedBy: user-A
```

---

### 8.2 Command Handler flow

```text id="cmd_flow"
1. Load Topic Node
2. Validate structural conditions
3. Evaluate policy
4. Create Project Node
5. Create initial branch
6. Persist
```

---

## 9. 🔍 Validation

### 9.1 Structural (Projection)

```text
- participant_count >= 2  ✅
- commitment_count >= 1  ✅
```

---

### 9.2 Policy (separate layer)

```text
- threshold profile: default
- time window satisfied: YES
```

👉 rozhodnutie tu, NIE v projekcii

---

## 10. 🧱 Vytvorenie Project Node

```json id="project_node"
{
  "id": "project-456",
  "type": "Project",
  "payload": {
    "stage": "forming",
    "origin_topic_id": "topic-123"
  },
  "relations": [
    { "type": "PARTICIPANT", "targetId": "user-A" },
    { "type": "PARTICIPANT", "targetId": "user-B" }
  ]
}
```

---

## 11. 🌿 Initial Branch

```json id="branch_node"
{
  "id": "branch-1",
  "type": "Branch",
  "payload": {
    "is_root": true
  },
  "relations": [
    { "type": "PROJECT", "targetId": "project-456" }
  ]
}
```

---

## 12. 🔄 Projection (Project)

### 12.1 ProjectSummary

```text id="proj_summary"
ProjectSummary:
- Id: project-456
- Stage: forming
- ParticipantCount: 2
```

---

### 12.2 ProjectDomainView

```text id="proj_domain"
ProjectDomainView:
- CanTransitionToActive = true
```

👉 stále len **structural readiness**

---

## 13. 👤 Aktivácia projektu

Používateľ klikne:

```text
[Spustiť projekt]
```

---

## 14. ⚙️ Command

```text id="cmd_activate"
ActivateProjectCommand:
- ProjectId: project-456
- TriggeredBy: user-A
```

---

## 15. ⚙️ Handler flow

```text id="activate_flow"
1. Load Project Node
2. Validate structural readiness
3. Evaluate policy readiness
4. Update stage → active
5. Save Node
```

---

## 16. 🧱 Updated Node

```json id="project_active"
{
  "id": "project-456",
  "type": "Project",
  "payload": {
    "stage": "active",
    "activated_at": "2026-04-16"
  }
}
```

---

## 17. 🧠 Projection (final state)

```text id="proj_final"
ProjectSummary:
- Stage: active

ProjectDomainView:
- IsActive = true
- IsHealthy = true
```

---

## 18. ⚠️ Edge cases (realita)

### 18.1 Partial data

```text
- participant missing
→ Summary OK
→ DomainView unavailable
```

---

### 18.2 Offline

```text
- user-A vidí active
- user-B ešte forming
→ eventual consistency
```

---

### 18.3 Conflict

```text
- dve aktivácie naraz
→ merge / idempotent command
```

---

## 19. 🧠 Čo tento flow dokazuje

### ✅ Oddelenie vrstiev

| Vrstva     | Úloha       |
| ---------- | ----------- |
| Node       | storage     |
| Projection | čítanie     |
| Command    | zápis       |
| Policy     | rozhodnutie |

---

### ✅ Žiadne porušenia

* ❌ Projection nerobí zápis
* ❌ Projection nerozhoduje
* ❌ Mapper nerobí logiku

---

### ✅ Reálna implementácia

Toto sa dá implementovať:

* C# (.NET MAUI)
* offline-first
* P2P sync

---

## 20. 🔥 Finálna veta

> **Tento flow ukazuje, že systém nie je postavený na magickej inteligencii, ale na striktne oddelených vrstvách: Node uchováva realitu, Projection ju interpretuje, Command ju mení a Policy rozhoduje. Vďaka tomu je systém zároveň flexibilný aj kontrolovateľný.**

---

# 🧭 Čo by som spravil ako ďalší krok

Ak chceš ísť ešte o level vyššie:

👉 spraviť ešte 2 flows:

1. **Branch creation & conflict**
2. **Project degradation (active → dormant)**

To sú 2 miesta, kde sa systém najčastejšie rozbije.

---
