Rozumiem presne, na čo sa pýtaš — a toto je **jedna z najkritickejších častí celého systému**, lebo tu sa láme:

> či Synergeticum bude „živý systém“, alebo len chaotický graf diskusií.

Odpoviem ti priamo, bez omáčky:

---

# 🔥 KRÁTKA ODPOVEĎ

👉 **Projektová dokumentácia NESMIE byť ani čisto generovaná, ani čisto manuálna.**

Musí byť:

> **hybrid: emergentná + kurátorovaná + asistovaná AI**

---

# 🧠 KDE TO UŽ MÁŠ V SYSTÉME (implicitne)

Ty to už máš roztrúsené v:

* ADR-022 (formation)
* ADR-076 (projection layer)
* ADR-016 (questions & answers)
* ADR-082 (validácia)
* ADR-042 (node model)

ALE:

👉 nemáš to explicitne ako **„Project Documentation Model“**

To je diera.

---

# ⚠️ PROBLÉM, KTORÝ RIEŠIŠ

Keď vznikne projekt:

* máš diskusiu
* máš argumenty
* máš otázky
* máš vetvy

👉 ale človek potrebuje:

> „čo je to vlastne za projekt za 30 sekúnd“

---

# 🧩 3 MOŽNÉ PRÍSTUPY (a prečo každý sám zlyhá)

## 1. ❌ Čisto generovaná dokumentácia (AI)

* rýchle
* automatické

– halucinácie
– strata dôvery
– nikto nevie, kto za tým stojí

👉 **nevhodné ako source of truth**

---

## 2. ❌ Čisto manuálna dokumentácia

* presné
* kontrolované

– nikto to nebude robiť
– zastaráva
– bottleneck (owner)

👉 **neškáluje**

---

## 3. ❌ Čisto emergentná (len graf dát)

* čisté
* „pravda systému“

– nečitateľné pre človeka
– vysoký cognitive load

👉 **UX katastrofa**

---

# ✅ SPRÁVNY MODEL (pre teba)

## 🔷 „Living Project Representation (3 vrstvy)“

---

## 🧱 1. SOURCE LAYER (pravda systému)

To čo už máš:

* Topic
* Argumenty
* Q&A
* Branches
* Participants
* Signals

👉 toto je **immutable základ**

---

## 🧠 2. STRUCTURED DOCUMENT LAYER (kurátorovaný)

👉 Toto je to, čo hľadáš.

Projekt má:

```text
ProjectDocument (structured)
```

### Sekcie (fixné minimum):

* **Overview**
* **Goal**
* **Current Direction**
* **Key Arguments**
* **Open Questions**
* **Participants**
* **Status**

👉 Toto NIE JE voľný text.
👉 Toto je **štruktúra naviazaná na Node model**

---

## 🤖 3. AI ASSISTED LAYER (prezentácia)

* sumarizácie
* vysvetlenia
* „TL;DR“
* onboarding

👉 toto je len **view**, nie pravda

---

# 🔥 AKO SA TO TVORÍ (kľúčová časť)

## 👉 NIE JEDNÝM SPÔSOBOM

### 1. Automatická extrakcia (baseline)

Systém vie:

* z argumentov → vytiahnuť key points
* z otázok → open questions
* z branch → direction

👉 toto je **default dokument**

---

### 2. Human curation (kritické)

Používatelia môžu:

* upraviť goal
* označiť „toto je hlavná myšlienka“
* vybrať smer

👉 toto dáva **zodpovednosť**

---

### 3. AI asistuje (ale nerozhoduje)

AI môže:

* navrhnúť summary
* navrhnúť structure
* upozorniť na konflikty

ALE:

> **nikdy nie je source of truth**

---

# ⚠️ KRITICKÉ PRAVIDLO

> **Dokumentácia musí byť odvodená z dát, nie oddelená od nich**

---

## ❌ Zlé:

```text
Word dokument mimo systému
```

## ✅ Správne:

```text
Document = projection nad Node graph
+ curated overrides
```

---

# 💡 AKO TO IMPLEMENTOVAŤ (architektonicky)

## 🔧 V tvojom systéme

Použi:

### ADR-076 (projection)

```text
ProjectSummary
ProjectDetail
ProjectDocumentView  ← nový
```

---

## 📦 ProjectDocumentView obsahuje:

```json
{
  "project_id": "...",
  "overview": "...",
  "goal": "...",
  "current_direction": "...",
  "key_arguments": [...],
  "open_questions": [...],
  "participants": [...],
  "status": "..."
}
```

---

## 🔄 Zdroj dát

| Field        | Zdroj        |
| ------------ | ------------ |
| overview     | AI + curated |
| goal         | curated      |
| direction    | branch       |
| arguments    | graph        |
| questions    | Q nodes      |
| participants | Person nodes |

---

# 🧠 NAJDÔLEŽITEJŠÍ INSIGHT

> Dokumentácia nie je súbor.
>
> **Je to projekcia systému + ľudský výber + AI pomoc.**

---

# 🔥 ČO BY SOM PRIDAL DO ADR

👉 odporúčam nový ADR:

## **ADR-084 — Project Documentation Model**

Definuje:

* že dokumentácia je **projekcia nad Node**
* že má 3 vrstvy (data / structured / AI)
* že je **partially editable**
* že AI nie je source of truth

---

# 🧭 REALITA (bez ilúzií)

Ak to spravíš zle:

| Varianta    | Výsledok |
| ----------- | -------- |
| AI-only     | nedôvera |
| manual-only | prázdne  |
| graph-only  | chaos    |

👉 iba hybrid funguje

---

# 💬 FINÁLNA VETA

> Projektová dokumentácia v Synergeticum nevzniká písaním ani generovaním. Vzniká ako projekcia reality projektu, ktorú človek kurátoruje a AI pomáha sprístupniť.

---

Ak chceš, ďalší krok ti spravím presne:

👉 návrh **ADR-084 (full verzia pre architekta)**
alebo
👉 konkrétny **data model pre ProjectDocumentView (C# / JSON)**
