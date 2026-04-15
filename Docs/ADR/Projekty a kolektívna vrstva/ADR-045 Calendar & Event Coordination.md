# 🧾 ADR-045: Calendar & Event Coordination

---

## 1. 📌 Status

**Proposed**

---

## 2. 🎯 Kontext

Synergetikum je systém pre:

* kolektívnu spoluprácu na projektoch
* koordináciu medzi ľuďmi s rôznymi záujmami a schopnosťami
* decentralizované rozhodovanie a argumentáciu

Projekty (ADR-022, ADR-023) a vetvy (ADR-024, ADR-029) vyžadujú:

* stretnutia (online alebo fyzické)
* deadliny pre rozhodnutia
* termíny pre odovzdanie výstupov
* synchronizáciu aktivít medzi participantmi

---

### Problém

V doterajších ADR chýba mechanizmus pre:

* **časovú koordináciu** – kedy sa čo deje
* **udalosti** – stretnutia, workshopy, deadliny
* **notifikácie** – ako sa používatelia dozvedia o blížiacich sa udalostiach

Bez tohto mechanizmu:

* projekty nemôžu reálne koordinovať tím
* mítingy a termíny sú mimo systém (email, chat)
* strata prehľadu o tom, kto sa zapojí
* chýba prepojenie medzi argumentáciou a reálnymi akciami

---

### Kľúčová otázka

👉 **Ako zabezpečiť časovú koordináciu v decentralizovanom P2P systéme bez centrálneho servera, pri zachovaní súkromia a lokálnej kontroly?**

---

## 3. ⚖️ Rozhodnutie

Systém zavádza:

👉 **Calendar & Event Coordination Layer**

Táto vrstva umožňuje:

* vytváranie a zdieľanie udalostí
* prepojenie udalostí s projektmi, vetvami, argumentmi a témami
* decentralizované notifikácie (opt-in, lokálne rozhodnutie)
* sledovanie účasti ako signálu zapojenia a commitmentu

---

## 4. 🧩 Event ako Node

### 4.1 Základná definícia

`Event` je špecifický typ Node (podľa ADR-042: Unified Entity Model).

```json
{
  "node_id": "event_uuid_001",
  "node_type": "Event",
  "name": "Kickoff stretnutie - Komunitná pizzéria",
  "description": "Prvé stretnutie tímu, predstavenie vetiev, rozdelenie rolí",
  
  "content": {
    "start_time": "2026-05-01T16:00:00Z",
    "end_time": "2026-05-01T18:00:00Z",
    "timezone": "Europe/Bratislava",
    "location": {
      "type": "hybrid",
      "physical": "Komunitné centrum, Trnava",
      "online": "https://meet.example.com/pizzeria-kickoff"
    },
    "recurrence": {
      "type": "weekly",
      "interval": 1,
      "day_of_week": "thursday",
      "until": "2026-07-01T00:00:00Z"
    },
    "status": "confirmed",
    "max_participants": 20
  },
  
  "relations": [
    {
      "target_id": "proj_pizzeria_001",
      "relation_type": "child_of",
      "strength": 1.0
    },
    {
      "target_id": "branch_crowdfunding_001",
      "relation_type": "related_to"
    },
    {
      "target_id": "arg_cf_001",
      "relation_type": "discusses"
    }
  ],
  
  "rights": {
    "owner": "user_miro_001",
    "edit": ["user_miro_001", "user_jana_001"],
    "view": ["circle"],
    "participate": ["circle"]
  },
  
  "statistics": {
    "participants_count": 8,
    "interested_count": 12,
    "maybe_count": 3,
    "declined_count": 2
  }
}
```

### 4.2 Atribúty Eventu

| Atribút | Typ | Povinný | Popis |
|---------|-----|---------|-------|
| `start_time` | ISO timestamp | Áno | Začiatok udalosti |
| `end_time` | ISO timestamp | Áno | Koniec udalosti |
| `timezone` | string (IANA) | Áno | Časová zóna (napr. `Europe/Bratislava`) |
| `location` | object | Nie | Miesto konania (fyzické, online, hybrid) |
| `recurrence` | object | Nie | Pravidlo opakovania (ak chýba, jednorazová udalosť) |
| `status` | enum | Áno | `proposed`, `confirmed`, `cancelled`, `rescheduled` |
| `max_participants` | int | Nie | Maximálny počet účastníkov (voliteľné) |

### 4.3 Typy lokácií

```json
// Fyzická lokácia
"location": {
  "type": "physical",
  "address": "Komunitné centrum, Trnava",
  "latitude": 48.3775,
  "longitude": 17.5867
}

// Online lokácia
"location": {
  "type": "online",
  "url": "https://meet.example.com/room123",
  "platform": "jitsi"
}

// Hybridná lokácia
"location": {
  "type": "hybrid",
  "physical": "Komunitné centrum, Trnava",
  "online": "https://meet.example.com/room123"
}
```

### 4.4 Recurrence (opakovanie)

```json
// Týždenné opakovanie
"recurrence": {
  "type": "weekly",
  "interval": 1,  // každý týždeň
  "day_of_week": "thursday",
  "until": "2026-07-01T00:00:00Z"
}

// Denné opakovanie (pracovné dni)
"recurrence": {
  "type": "weekly",
  "interval": 1,
  "days_of_week": ["monday", "tuesday", "wednesday", "thursday", "friday"],
  "until": "2026-12-31T00:00:00Z"
}

// Mesačné opakovanie
"recurrence": {
  "type": "monthly",
  "interval": 1,
  "day_of_month": 15,
  "until": "2026-12-31T00:00:00Z"
}

// Ročné opakovanie
"recurrence": {
  "type": "yearly",
  "interval": 1,
  "month": 5,
  "day_of_month": 15,
  "until": "2030-05-15T00:00:00Z"
}
```

---

## 5. 🔗 Prepojenie Eventov na iné Entity

Eventy môžu byť spojené s:

| Entity | Typ vzťahu | Význam |
|--------|-----------|--------|
| **Project** | `child_of` | Event patrí projektu (napr. kickoff projektu) |
| **Branch** | `related_to` | Event sa týka konkrétnej vetvy |
| **Argument** | `discusses` | Event bude diskutovať konkrétny argument |
| **Topic** | `classified_as` | Event je kategorizovaný pod tému |
| **Person** | `organized_by` | Kto event organizuje |

Príklad: Event, ktorý je stretnutím pre vetvu "Crowdfunding" a bude diskutovať argument "Buduje komunitu":

```json
"relations": [
  { "target_id": "proj_pizzeria_001", "relation_type": "child_of" },
  { "target_id": "branch_crowdfunding_001", "relation_type": "related_to" },
  { "target_id": "arg_cf_001", "relation_type": "discusses" },
  { "target_id": "user_miro_001", "relation_type": "organized_by" }
]
```

---

## 6. 👥 Účasť na Evente

### 6.1 Statusy účasti

Používateľ môže k eventu vyjadriť svoj záujem:

| Status | Význam |
|--------|--------|
| `going` | Zúčastním sa |
| `interested` | Zaujíma ma, ale ešte neviem |
| `maybe` | Možno, potvrdím neskôr |
| `declined` | Nemôžem sa zúčastniť |

### 6.2 Štruktúra účasti

```json
{
  "event_id": "event_uuid_001",
  "user_id": "user_miro_001",
  "status": "going",
  "responded_at": "2026-04-15T10:00:00Z",
  "notes": "Prinesiem občerstvenie",
  "reminder_enabled": true
}
```

### 6.3 Synchronizácia účasti

* Účasť je lokálna informácia používateľa.
* Synchronizuje sa len v rámci Trusted Circles (ADR-004).
* Organizátor eventu vidí agregované štatistiky (počty, nie kto konkrétne, pokiaľ nie je v tom istom Trusted Circle).

---

## 7. 🔔 Notifikačný mechanizmus

### 7.1 Princíp

> **Notifikácie sú opt-in, lokálne rozhodnutie, nie centrálny server.**

Systém neposiela notifikácie centrálne. Namiesto toho:

1. Každý používateľ má lokálny **Notification Scheduler**.
2. Scheduler pravidelne kontroluje blížiace sa eventy z relevantných projektov.
3. Používateľ rozhoduje, pre ktoré projekty/cirkly chce notifikácie.
4. Notifikácie sú čisto lokálne – nikto iný nevie, či používateľ dostal notifikáciu.

### 7.2 Typy notifikácií

| Typ | Kedy | Lokálne nastavenie |
|-----|------|-------------------|
| **Reminder** | X minút/hodín/dní pred eventom | Čas vopred (default 1 hodina) |
| **Change notification** | Zmena času, miesta, statusu eventu | Áno/nie |
| **New event** | Nový event v projekte/circle | Áno/nie |
| **Cancellation** | Event bol zrušený | Vždy (ak má používateľ status `going`) |

### 7.3 Lokálne nastavenia

```json
{
  "user_id": "user_miro_001",
  "notification_preferences": {
    "global_enabled": true,
    "default_reminder_minutes": 60,
    "per_project": {
      "proj_pizzeria_001": {
        "enabled": true,
        "reminder_minutes": 30,
        "notify_on_change": true,
        "notify_new_events": true
      }
    },
    "per_circle": {
      "circle_family_001": {
        "enabled": true,
        "reminder_minutes": 120
      }
    },
    "quiet_hours": {
      "enabled": true,
      "start": "22:00",
      "end": "08:00",
      "timezone": "Europe/Bratislava"
    }
  }
}
```

### 7.4 Ako notifikácie fungujú bez centrálneho servera

1. Keď používateľ vytvorí alebo zmení event, zmena sa synchronizuje s participantmi v Trusted Circle.
2. Každý participant má lokálnu kópiu eventov z projektov, v ktorých participuje.
3. Lokálny Notification Scheduler pravidelne (napr. každú hodinu) kontroluje:
   - `event.start_time - now() <= reminder_time`
   - `event.status == "confirmed"`
   - Používateľ má `going` alebo `interested` status
4. Ak podmienky splnené, zobrazí lokálnu notifikáciu.
5. **Žiadny centrálny server nevie o notifikáciách.**

---

## 8. 🔄 Prepojenie na Engagement Model (ADR-012)

ADR-012 definuje úrovne zapojenia používateľa:

| Úroveň | Popis |
|--------|-------|
| Pasívne pozretie | Len číta |
| Odpoveď | Odpovedá na otázky |
| Diskusia | Aktívne diskutuje |
| Záväzok | Explicitne sa zaväzuje |
| Reálna participácia | Aktívne koná |

**Prepojenie:**

* **Účasť na evente** je minimálne na úrovni "Diskusia".
* **Organizovanie eventu** je na úrovni "Reálna participácia".
* **Pravidelná účasť** (napr. na týždenných stretnutiach) je signálom pre posun smerom k "Záväzku".

```json
// Engagement signal z účasti na evente
{
  "user_id": "user_miro_001",
  "project_id": "proj_pizzeria_001",
  "engagement_signal": {
    "type": "event_participation",
    "event_id": "event_uuid_001",
    "level": "discussion",  // minimálne
    "timestamp": "2026-05-01T18:00:00Z",
    "attendance_confirmed": true
  }
}
```

---

## 9. 📊 Prepojenie na Support & Alignment Model (ADR-028)

ADR-028 definuje, ako sa meria podpora vetiev projektu:

* **Alignment** – súhlas so smerom
* **Commitment** – ochota investovať čas/energiu
* **Confidence** – istota v rozhodnutí

**Prepojenie:**

* **Účasť na evente** je signálom **commitmentu**.
* **Pravidelná účasť** zvyšuje váhu commitment signálu.
* **Organizovanie eventu** je vysoký commitment signál.

```json
// Commitment signal z účasti na evente
{
  "user_id": "user_miro_001",
  "branch_id": "branch_crowdfunding_001",
  "commitment_signal": {
    "type": "event_participation",
    "event_id": "event_uuid_001",
    "weight": 0.6,  // účasť na jednom evente
    "recurring_weight": 0.8,  // ak je pravidelná účasť
    "organizer_weight": 1.0   // ak je organizátor
  }
}
```

---

## 10. 🧠 Dôsledky rozhodnutia

### ✅ Pozitívne

| Oblasť | Dôsledok |
|--------|----------|
| **Koordinácia** | Projekty môžu reálne plánovať stretnutia a termíny |
| **Prehľadnosť** | Všetky udalosti sú v systéme, nie v externých nástrojoch |
| **Prepojenie** | Eventy sú prepojené s argumentmi a vetvami |
| **Decentralizácia** | Notifikácie sú lokálne, žiadny centrálny server |
| **Súkromie** | Používateľ rozhoduje, o čom chce vedieť |
| **Engagement** | Účasť na evente je merateľný signál zapojenia |
| **Commitment** | Pravidelná účasť posilňuje commitment signál |

### ❗ Negatíva / Trade-offs

| Oblasť | Dôsledok |
|--------|----------|
| **Komplexita** | Ďalšia vrstva do systému |
| **Sync oneskorenie** | Zmeny eventov sa nemusia dostať ku všetkým okamžite (eventual consistency) |
| **Lokálne úložisko** | Každý používateľ ukladá kópie eventov z viacerých projektov |
| **Žiadne centrálne notifikácie** | Používateľ musí byť online, aby dostal aktualizácie eventov |
| **Riziko rozptylu** | Príliš veľa notifikácií môže viesť k ignorovaniu |

---

## 11. 🚫 Alternatívy (zamietnuté)

### ❌ Externý kalendár (Google Calendar, Outlook)

* jednoduchšia implementácia
* ale: porušuje súkromie, vyžaduje centrálny účet, mimo systém

### ❌ Centrálny notifikačný server

* jednoduchšie na implementáciu
* ale: centralizácia, sledovanie používateľov, porušuje ADR-004

### ❌ Iba textové notifikácie bez štruktúry

* jednoduchšie
* ale: žiadne prepojenie na projektový model, žiadna účastnícka logika

### ❌ Žiadny kalendár (spoliehať sa na externé nástroje)

* najjednoduchšie
* ale: systém nie je sebestačný, fragmentácia nástrojov

---

## 12. 🔗 Väzby na ďalšie ADR

| ADR | Vzťah |
|-----|-------|
| ADR-001 | Ownership – používateľ vlastní svoje notifikačné nastavenia |
| ADR-004 | Privacy-first – notifikácie sú lokálne, synchronizujú sa len v Trusted Circles |
| ADR-012 | Engagement Model – účasť na evente je úroveň zapojenia |
| ADR-014 | Network Communication – sync eventov medzi uzlami |
| ADR-015 | Participation Filter – používateľ vidí len eventy z relevantných projektov |
| ADR-022 | Project Formation – eventy patria projektom |
| ADR-024 | Branching – eventy môžu byť viazané na vetvy |
| ADR-025 | Argumentation – eventy môžu diskutovať argumenty |
| ADR-028 | Support & Alignment – účasť na evente je signál commitmentu |
| ADR-030 | Sync & Persistence – eventy sa synchronizujú ako iné Node-y |
| ADR-034 | Personal AI – notifikačné preferencie sú súčasťou Preference Modelu |
| **ADR-042** | **Unified Entity Model – Event je typ Node** |

---

## 13. 📏 Pravidlá implementácie (high-level)

Systém musí:

* umožniť vytváranie Event Node-ov s povinnými atribútmi (`start_time`, `end_time`, `timezone`)
* podporovať jednorazové aj opakujúce sa eventy
* umožniť prepojenie eventov s projektmi, vetvami, argumentmi a témami
* poskytnúť používateľom možnosť vyjadriť účasť (`going`, `interested`, `maybe`, `declined`)
* implementovať lokálny notifikačný scheduler (žiadny centrálny server)
* umožniť používateľom nastaviť notifikačné preferencie per projekt / per circle
* synchronizovať eventy v rámci Trusted Circles (ADR-004)
* prepojiť účasť na evente s Engagement Modelom (ADR-012) a Support Modelom (ADR-028)

---

## 14. 🔥 Príklady

### Príklad 1: Jednorazový event

```json
{
  "node_id": "event_kickoff_001",
  "node_type": "Event",
  "name": "Kickoff - Komunitná pizzéria",
  "content": {
    "start_time": "2026-05-01T16:00:00Z",
    "end_time": "2026-05-01T18:00:00Z",
    "timezone": "Europe/Bratislava",
    "location": {
      "type": "hybrid",
      "physical": "Komunitné centrum, Trnava",
      "online": "https://meet.example.com/pizzeria"
    },
    "status": "confirmed"
  },
  "relations": [
    { "target_id": "proj_pizzeria_001", "relation_type": "child_of" }
  ]
}
```

### Príklad 2: Opakujúci sa event (týždenné stretnutie)

```json
{
  "node_id": "event_weekly_001",
  "node_type": "Event",
  "name": "Týždenné stretnutie - Crowdfunding vetva",
  "content": {
    "start_time": "2026-05-07T17:00:00Z",
    "end_time": "2026-05-07T18:30:00Z",
    "timezone": "Europe/Bratislava",
    "location": {
      "type": "online",
      "url": "https://meet.example.com/crowdfunding"
    },
    "recurrence": {
      "type": "weekly",
      "interval": 1,
      "day_of_week": "thursday",
      "until": "2026-07-01T00:00:00Z"
    },
    "status": "confirmed"
  },
  "relations": [
    { "target_id": "proj_pizzeria_001", "relation_type": "child_of" },
    { "target_id": "branch_crowdfunding_001", "relation_type": "related_to" }
  ]
}
```

### Príklad 3: Účasť používateľa

```json
{
  "event_id": "event_kickoff_001",
  "user_id": "user_miro_001",
  "status": "going",
  "responded_at": "2026-04-15T10:00:00Z",
  "notes": "Môžem pomôcť s organizáciou",
  "reminder_enabled": true
}
```

### Príklad 4: Notifikačné nastavenia

```json
{
  "user_id": "user_miro_001",
  "notification_preferences": {
    "global_enabled": true,
    "default_reminder_minutes": 60,
    "quiet_hours": {
      "enabled": true,
      "start": "22:00",
      "end": "08:00"
    },
    "per_project": {
      "proj_pizzeria_001": {
        "enabled": true,
        "reminder_minutes": 30,
        "notify_on_change": true,
        "notify_new_events": true
      }
    }
  }
}
```

### Príklad 5: Engagement signal z účasti

```json
{
  "timestamp": "2026-05-01T18:00:00Z",
  "user_id": "user_miro_001",
  "project_id": "proj_pizzeria_001",
  "engagement_signal": {
    "type": "event_participation",
    "event_id": "event_kickoff_001",
    "level": "discussion",
    "attendance_confirmed": true,
    "duration_minutes": 120,
    "spoke": true
  }
}
```

### Príklad 6: Commitment signal z pravidelnej účasti

```json
{
  "user_id": "user_miro_001",
  "branch_id": "branch_crowdfunding_001",
  "commitment_signal": {
    "type": "recurring_event_participation",
    "event_series_id": "event_weekly_001",
    "attended_count": 4,
    "total_count": 4,
    "attendance_rate": 1.0,
    "weight": 0.8
  }
}
```

---

## 15. 🔥 Zhrnutie

> **Eventy sú časová koordinačná vrstva systému. Umožňujú projektom reálne stretávať sa, plánovať termíny a synchronizovať aktivity – všetko v decentralizovanom prostredí bez centrálneho servera.**

**ADR-045 prináša:**

* **Event ako Node** – jednotná štruktúra pre všetky udalosti
* **Prepojenie** – eventy môžu byť spojené s projektmi, vetvami, argumentmi
* **Lokálne notifikácie** – opt-in, žiadny centrálny server
* **Účasť** – používatelia môžu vyjadriť záujem (going, interested, maybe, declined)
* **Prepojenie na Engagement** – účasť na evente je signálom zapojenia (ADR-012)
* **Prepojenie na Commitment** – pravidelná účasť posilňuje commitment (ADR-028)

---

## 16. 🧭 Finálna veta

> **Bez časovej koordinácie nie je možná reálna spolupráca. ADR-045 dáva projektom nástroj na synchronizáciu ľudí v čase – bez toho, aby ohrozil súkromie alebo centralizoval kontrolu.**

Projekty v Synergetiku nie sú len zoznam argumentov a vetiev – sú to živé entity, ktoré potrebujú stretnutia, termíny a spoločné akcie. ADR-045 im to umožňuje spôsobom, ktorý je v súlade s princípmi decentralizácie, súkromia a ľudskej autonómie.

---

**Dátum:** 2026-04-15  
**Stav:** Proposed  
**Typ:** Feature ADR (Calendar & Coordination)  
**Verzia:** 1.0

---

## 📝 Zoznam zmien oproti prázdnemu stavu

| Sekcia | Zmena |
|--------|-------|
| 1-16 | **Celý ADR je nový** |