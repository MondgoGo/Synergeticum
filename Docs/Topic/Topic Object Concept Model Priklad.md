# Nižšie ti dám **realistický príklad projektu „Pizzéria na sídlisku“** tak, aby si videl:

* identitu projektu
* vetvy
* participantov
* support
* argumenty
* event stream
* current snapshot
* personal view pre jedného človeka

Najprv dám krátke vysvetlenie a potom celý príklad.

---

# Ako to čítať

Projekt si predstav ako objekt s viacerými vrstvami:

1. **project_identity**
   stabilné info o projekte

2. **branches**
   rôzne smery riešenia

3. **participants**
   kto je v projekte a ako sa zapája

4. **arguments**
   argumenty za a proti

5. **events**
   história zmien

6. **derived_state**
   aktuálny stav odvodený z eventov

7. **views**
   čo z toho vidí konkrétny používateľ cez svoju Personal AI

---

# Príklad v JSON

```json
{
  "project_identity": {
    "project_id": "proj_pizzeria_trnava_001",
    "title": "Možnosť vytvoriť pizzériu na sídlisku Prednádražie",
    "summary": "Komunitný projekt zameraný na preverenie a prípadnú realizáciu nového stravovacieho miesta v lokalite Prednádražie v Trnave.",
    "project_type": "execution",
    "governance_mode": "hybrid",
    "created_at": "2026-04-11T19:10:00Z",
    "initiator_user_id": "user_miro_001",
    "origin_question": {
      "question_id": "q_1001",
      "text": "Chýba mi na našom sídlisku miesto, kde by sa dalo dobre a rýchlo najesť. Myslíte si, že by tu mala zmysel pizzéria alebo podobný podnik?"
    },
    "status": "active"
  },

  "branches": [
    {
      "branch_id": "branch_A",
      "parent_branch_id": null,
      "title": "Malá klasická pizzéria s rozvozom",
      "description": "Jednopodlažná menšia pizzéria zameraná na lokálnych obyvateľov, osobný odber a rozvoz.",
      "state": "active",
      "created_at": "2026-04-12T08:00:00Z",
      "created_by": "user_miro_001",
      "support_profile": {
        "alignment_score": 0.74,
        "commitment_score": 0.43,
        "confidence_score": 0.68,
        "expertise_signal": 0.39,
        "health_indicator": "strong"
      },
      "key_metrics": {
        "estimated_initial_cost": 120000,
        "estimated_monthly_demand": 850,
        "estimated_risk_level": "medium"
      },
      "lifecycle": {
        "phase": "active",
        "last_meaningful_activity": "2026-04-20T10:15:00Z",
        "revived_from": null
      }
    },
    {
      "branch_id": "branch_B",
      "parent_branch_id": "branch_A",
      "title": "Dvojpodlažná pizzéria + cowork kút",
      "description": "Rozšírený koncept, kde by prízemie bolo gastro a horné podlažie by slúžilo ako menší cowork / komunitný priestor.",
      "state": "active",
      "created_at": "2026-04-14T12:30:00Z",
      "created_by": "user_lucia_013",
      "support_profile": {
        "alignment_score": 0.48,
        "commitment_score": 0.19,
        "confidence_score": 0.41,
        "expertise_signal": 0.52,
        "health_indicator": "uncertain"
      },
      "key_metrics": {
        "estimated_initial_cost": 260000,
        "estimated_monthly_demand": 920,
        "estimated_risk_level": "high"
      },
      "lifecycle": {
        "phase": "active",
        "last_meaningful_activity": "2026-04-18T14:05:00Z",
        "revived_from": null
      }
    },
    {
      "branch_id": "branch_C",
      "parent_branch_id": "branch_A",
      "title": "Len výdajné okienko + rozvoz bez sedenia",
      "description": "Nízkoriziková minimalistická verzia bez interiérového sedenia, zameraná na rýchlosť a nízke náklady.",
      "state": "dormant",
      "created_at": "2026-04-15T09:10:00Z",
      "created_by": "user_roman_021",
      "support_profile": {
        "alignment_score": 0.36,
        "commitment_score": 0.11,
        "confidence_score": 0.45,
        "expertise_signal": 0.34,
        "health_indicator": "weak"
      },
      "key_metrics": {
        "estimated_initial_cost": 60000,
        "estimated_monthly_demand": 500,
        "estimated_risk_level": "low"
      },
      "lifecycle": {
        "phase": "dormant",
        "last_meaningful_activity": "2026-04-16T17:40:00Z",
        "revived_from": null
      }
    }
  ],

  "participants": [
    {
      "participant_id": "user_miro_001",
      "display_name": "Miro",
      "roles": ["initiator", "concept_owner"],
      "participation_type": "active",
      "capabilities": ["vision", "community_trigger", "project_direction"],
      "branch_alignment": {
        "branch_A": 0.91,
        "branch_B": 0.42,
        "branch_C": 0.37
      },
      "branch_commitment": {
        "branch_A": 0.78,
        "branch_B": 0.10,
        "branch_C": 0.05
      }
    },
    {
      "participant_id": "user_marek_008",
      "display_name": "Marek",
      "roles": ["builder", "executor"],
      "participation_type": "active",
      "capabilities": ["construction", "supplier_contacts"],
      "branch_alignment": {
        "branch_A": 0.73,
        "branch_B": 0.33,
        "branch_C": 0.40
      },
      "branch_commitment": {
        "branch_A": 0.66,
        "branch_B": 0.12,
        "branch_C": 0.18
      }
    },
    {
      "participant_id": "user_jana_014",
      "display_name": "Jana",
      "roles": ["investor_candidate"],
      "participation_type": "reflective",
      "capabilities": ["capital", "financial_review"],
      "branch_alignment": {
        "branch_A": 0.61,
        "branch_B": 0.27,
        "branch_C": 0.22
      },
      "branch_commitment": {
        "branch_A": 0.31,
        "branch_B": 0.07,
        "branch_C": 0.03
      }
    },
    {
      "participant_id": "user_lucia_013",
      "display_name": "Lucia",
      "roles": ["community_designer", "co_innovator"],
      "participation_type": "active",
      "capabilities": ["space_design", "community_program"],
      "branch_alignment": {
        "branch_A": 0.44,
        "branch_B": 0.88,
        "branch_C": 0.20
      },
      "branch_commitment": {
        "branch_A": 0.16,
        "branch_B": 0.64,
        "branch_C": 0.03
      }
    }
  ],

  "arguments": [
    {
      "argument_id": "arg_001",
      "branch_id": "branch_A",
      "type": "support",
      "author_id": "user_miro_001",
      "claim": "Na sídlisku je nedostatok kvalitného rýchleho jedla vo večerných hodinách.",
      "reasoning": "Najbližšie možnosti sú buď slabé kvalitatívne, alebo príliš vzdialené na peší prístup.",
      "assumptions": [
        "obyvatelia preferujú dostupnosť pešo",
        "večerný dopyt je dostatočný"
      ],
      "implications": [
        "lokálny podnik by mohol mať stabilnú opakovanú klientelu"
      ],
      "confidence": 0.72
    },
    {
      "argument_id": "arg_002",
      "branch_id": "branch_B",
      "type": "support",
      "author_id": "user_lucia_013",
      "claim": "Pizzéria s komunitným priestorom by zvýšila hodnotu projektu a odlíšila ho od konkurencie.",
      "reasoning": "Samotná pizzéria je bežný koncept, ale spojenie s komunitnou vrstvou by mohlo vytvoriť miesto stretávania.",
      "assumptions": [
        "komunita by mala záujem o menšie podujatia alebo cowork",
        "náklady by vedeli byť pokryté vyššou pridanou hodnotou"
      ],
      "implications": [
        "vyššie počiatočné investície",
        "vyššie prevádzkové riziko"
      ],
      "confidence": 0.58
    },
    {
      "argument_id": "arg_003",
      "branch_id": "branch_B",
      "type": "counter",
      "author_id": "user_jana_014",
      "claim": "Dvojpodlažný koncept je príliš kapitálovo náročný vzhľadom na neistý dopyt.",
      "reasoning": "Pred overením základného gastronomického dopytu je rozširovanie o ďalšiu funkciu príliš riskantné.",
      "assumptions": [
        "dopyt po cowork/komunitnom priestore nie je overený"
      ],
      "implications": [
        "možné oneskorenie realizácie",
        "vyššie riziko návratnosti"
      ],
      "confidence": 0.79
    }
  ],

  "events": [
    {
      "event_id": "evt_0001",
      "timestamp": "2026-04-11T19:10:00Z",
      "type": "project_created",
      "actor_id": "user_miro_001",
      "payload": {
        "project_id": "proj_pizzeria_trnava_001",
        "title": "Možnosť vytvoriť pizzériu na sídlisku Prednádražie"
      }
    },
    {
      "event_id": "evt_0002",
      "timestamp": "2026-04-12T08:00:00Z",
      "type": "branch_created",
      "actor_id": "user_miro_001",
      "payload": {
        "branch_id": "branch_A",
        "title": "Malá klasická pizzéria s rozvozom"
      }
    },
    {
      "event_id": "evt_0003",
      "timestamp": "2026-04-12T09:40:00Z",
      "type": "participant_joined",
      "actor_id": "user_marek_008",
      "payload": {
        "participant_id": "user_marek_008",
        "roles": ["builder", "executor"]
      }
    },
    {
      "event_id": "evt_0004",
      "timestamp": "2026-04-13T17:25:00Z",
      "type": "support_changed",
      "actor_id": "user_jana_014",
      "payload": {
        "branch_id": "branch_A",
        "alignment": 0.61,
        "commitment": 0.31,
        "confidence": 0.74
      }
    },
    {
      "event_id": "evt_0005",
      "timestamp": "2026-04-14T12:30:00Z",
      "type": "branch_created",
      "actor_id": "user_lucia_013",
      "payload": {
        "branch_id": "branch_B",
        "parent_branch_id": "branch_A",
        "title": "Dvojpodlažná pizzéria + cowork kút"
      }
    },
    {
      "event_id": "evt_0006",
      "timestamp": "2026-04-15T09:10:00Z",
      "type": "branch_created",
      "actor_id": "user_roman_021",
      "payload": {
        "branch_id": "branch_C",
        "parent_branch_id": "branch_A",
        "title": "Len výdajné okienko + rozvoz bez sedenia"
      }
    },
    {
      "event_id": "evt_0007",
      "timestamp": "2026-04-16T17:40:00Z",
      "type": "branch_state_changed",
      "actor_id": "system",
      "payload": {
        "branch_id": "branch_C",
        "old_state": "active",
        "new_state": "dormant",
        "reason": "low_activity_low_commitment"
      }
    },
    {
      "event_id": "evt_0008",
      "timestamp": "2026-04-18T14:05:00Z",
      "type": "argument_added",
      "actor_id": "user_jana_014",
      "payload": {
        "argument_id": "arg_003",
        "branch_id": "branch_B",
        "type": "counter"
      }
    },
    {
      "event_id": "evt_0009",
      "timestamp": "2026-04-20T10:15:00Z",
      "type": "branch_updated",
      "actor_id": "user_miro_001",
      "payload": {
        "branch_id": "branch_A",
        "update_summary": "Doplnené lokality, predbežný odhad dopytu a rozvozová zóna"
      }
    }
  ],

  "derived_state": {
    "active_branch_ids": ["branch_A", "branch_B"],
    "dormant_branch_ids": ["branch_C"],
    "archived_branch_ids": [],
    "top_conflict": {
      "conflict_id": "conf_01",
      "description": "Jednoduchá lacnejšia pizzéria vs rozšírený komunitný dvojpodlažný koncept",
      "branch_ids": ["branch_A", "branch_B"]
    },
    "project_health": {
      "overall_activity": "medium_high",
      "overall_engagement": "medium",
      "overall_clarity": "medium",
      "execution_readiness": "early_stage"
    }
  },

  "views": {
    "for_user_marek_008": {
      "recommended_branches": ["branch_A", "branch_C"],
      "hidden_or_deprioritized_branches": ["branch_B"],
      "summary": "Projekt má pre teba význam hlavne ako stavebná a realizačná príležitosť. Najsilnejší a najrealistickejší variant je menšia pizzéria s rozvozom. Minimalistická verzia bez sedenia je slabšia, ale stále ti môže byť blízka ako technicky jednoduchší variant.",
      "key_question": "Ak by sa išlo do realizácie, bol by si ochotný pomôcť so stavebnou časťou vetvy A?",
      "suggested_role": "executor"
    },
    "for_user_jana_014": {
      "recommended_branches": ["branch_A", "branch_B"],
      "hidden_or_deprioritized_branches": ["branch_C"],
      "summary": "Pre teba je dôležité finančné riziko a návratnosť. Vetva A vyzerá ako realistickejší vstupný bod. Vetva B má potenciál vyššej hodnoty, ale aj vyššie riziko a slabšie potvrdený dopyt.",
      "key_question": "Za akých podmienok by si bola ochotná vstúpiť ako investor do vetvy A?",
      "suggested_role": "investor_candidate"
    }
  }
}
```

---

# Čo si z toho máš odniesť

Toto je dôležité:

## 1. Projekt nie je jeden text

Projekt je v skutočnosti **balík viacerých logických častí**:

* identita
* vetvy
* argumenty
* participants
* eventy
* odvodený stav
* personalizované view

## 2. Eventy sú chrbtica

Keby si sa ma spýtal, čo je najdôležitejšie, tak:
👉 **events**

Lebo z nich vieš:

* zrekonštruovať históriu
* odvodiť current state
* synchronizovať len zmeny

## 3. `derived_state` je to, čo chce UI

Používateľ nebude čítať celý event stream.
Bude ho zaujímať:

* čo je aktívne
* aký je konflikt
* čo je silné
* čo sa ho týka

## 4. `views` sú kritické

Bez nich sa v tom človek stratí.
Personal AI má robiť presne toto:

* vybrať relevantné vetvy
* zhrnúť ich
* položiť otázku
* navrhnúť rolu

---

# Ako by to mohlo vyzerať v Python-like tvare

Ak chceš čitateľnejšiu verziu:

```python
project = {
    "project_identity": {
        "project_id": "proj_pizzeria_trnava_001",
        "title": "Možnosť vytvoriť pizzériu na sídlisku Prednádražie",
        "project_type": "execution",
        "governance_mode": "hybrid",
    },
    "branches": [
        {
            "branch_id": "branch_A",
            "title": "Malá klasická pizzéria s rozvozom",
            "state": "active",
            "support_profile": {
                "alignment": 0.74,
                "commitment": 0.43,
                "confidence": 0.68,
            }
        },
        {
            "branch_id": "branch_B",
            "title": "Dvojpodlažná pizzéria + cowork",
            "state": "active",
            "support_profile": {
                "alignment": 0.48,
                "commitment": 0.19,
                "confidence": 0.41,
            }
        }
    ],
    "participants": [
        {"user_id": "user_miro_001", "roles": ["initiator"]},
        {"user_id": "user_marek_008", "roles": ["builder"]},
        {"user_id": "user_jana_014", "roles": ["investor_candidate"]},
    ],
    "events": [
        {"type": "project_created", "actor": "user_miro_001"},
        {"type": "branch_created", "actor": "user_miro_001", "branch_id": "branch_A"},
        {"type": "branch_created", "actor": "user_lucia_013", "branch_id": "branch_B"},
        {"type": "argument_added", "actor": "user_jana_014", "branch_id": "branch_B"},
    ]
}
```

---

# Kde je v tom sync

Ak to prepojíme s tým, čo sme riešili predtým:

* **lokálne uložené** môže byť všetko
* medzi uzlami sa neposiela celý JSON stále dokola
* posielajú sa hlavne:

  * nové `events`
  * prípadne občas snapshot `derived_state`
  * a možno vybrané `views` sa vôbec neposielajú, ale generujú lokálne

Teda v realite by si mal niečo ako:

```text
Local Project Object
    + Event Log
    + Branch Objects
    + Support Signals
    + Argument Graph
    + Locally Generated View
```

---

# Čo je na tomto najťažšie

Najťažšie na tom nie je vymyslieť JSON.
Najťažšie je:

* synchronizácia eventov
* riešenie konfliktov
* branching
* efektívne generovanie `derived_state`
* a hlavne aby sa to celé nerozsypalo na mobile

Preto som ťa predtým brzdil pri tom, že sync engine vás môže zabiť.

---

# Môj tvrdý názor

Ak by som to mal zjednodušiť do MVP, tak by som v prvej verzii vôbec nemal:

* plný argument graph
* zložité support profily pre všetko
* veľa lifecycle stavov

Začal by som len s:

* project_identity
* branches
* participants
* events
* derived_state
* local view

To je jadro, na ktorom vieš stavať.

---
