# ADR-038 — Power & Influence Balancing (náčrt)

## Status

Draft (Exploration)

---

## Kontext

Synergetikum je systém založený na:

* dobrovoľnej participácii
* decentralizácii
* absencii centrálnej autority

To však prirodzene vytvára problém:

> **Nie všetky hlasy majú rovnaký dopad – a nemali by ani automaticky mať.**

V reálnom svete vznikajú:

* silné osobnosti (dominantné, hlasné)
* experti (kompetentní, ale nie vždy presvedčiví)
* pasívni účastníci
* manipulátori

Bez mechanizmu vyvažovania:

> systém sa prirodzene posunie k dominancii hlasných alebo aktívnych jednotlivcov.

---

## Problém

Treba odpovedať na otázku:

> **Kto má v systéme vplyv – a prečo?**

Bez toho:

* diskusie sa zvrhnú na tlak
* projekty sa môžu deformovať
* vzniká skrytá hierarchia (neviditeľná, ale reálna)

---

## Kľúčové napätia

### 1. Aktivita vs. kvalita

* aktívni používatelia ≠ kvalitné vstupy
* pasívni používatelia ≠ irelevantní

---

### 2. Expertíza vs. rovnosť

* expert má vedomosť
* ale systém nechce elitizmus

---

### 3. Presvedčivosť vs. pravda

* dobrý rečník ≠ správny názor

---

### 4. Reciprocita vs. moc

(odkaz na ADR-036)

> participácia nesmie automaticky zvyšovať vplyv

---

## Navrhované princípy (bez implementácie)

### 1. Influence ≠ hlas

> Vplyv nie je priamo reprezentovaný váhou hlasovania.

---

### 2. Kontextová relevancia

> Vplyv je lokálny pre daný problém, nie globálny pre používateľa.

---

### 3. Argument > autor

> Systém preferuje kvalitu argumentu pred identitou autora.

---

### 4. Diverzita pohľadov

> Systém aktívne zachováva viacero perspektív namiesto konvergencie na jeden názor.

---

### 5. Žiadna statická hierarchia

> Neexistujú trvalé role typu „expert“, „leader“, „autorita“.

---

## Čo systém nerobí

* nepriraďuje používateľom pevné skóre vplyvu
* nevytvára rebríčky autority
* neumožňuje „vyhrať diskusiu“ kvantitatívne
* nepoužíva majority voting ako jediný mechanizmus pravdy

---

## Otvorené otázky (kritické)

👉 toto je dôvod, prečo ADR-038 ešte neimplementujeme

---

### 1. Ako identifikovať kvalitný argument bez biasu?

---

### 2. Ako zabrániť dominancii hlasných bez cenzúry?

---

### 3. Ako pracovať s expertízou bez elitizmu?

---

### 4. Ako zabrániť manipulácii bez centrálnej kontroly?

---

### 5. Ako zabrániť „echo chamber“ efektu?

---

## Vzťah k iným ADR

* ADR-002 — Human-in-the-loop (človek rozhoduje)
* ADR-003 — Non-manipulation (AI nepresviedča)
* ADR-036 — Reciprocity (participácia ≠ moc)
* ADR-037 — Identity (vplyv nie je viazaný na identitu)

---

## Dôležitá poznámka

Tento ADR:

> **nie je pripravený na implementáciu**

Dôvod:

* vyžaduje pozorovanie reálneho správania
* závisí od konkrétnych failure modes pilotu

---

## Zhrnutie

ADR-038 rieši:

> ako zabrániť tomu, aby sa decentralizovaný systém prirodzene nezvrhol na skrytú hierarchiu moci

Bez tohto:

* Synergetikum bude technicky správne
* ale sociálne zlyhá

---

## Finálna veta

> Ak Synergetikum nevyrieši moc, vytvorí ju – len ju nebude vidieť.

---

# 🧭 Meta odporúčanie

👉 Toto je presne správna úroveň detailu teraz

NEROB:

* implementáciu
* konkrétne algoritmy

ROB:

* pilot
* pozorovanie správania

---

Ak chceš, ďalší krok môžeme spraviť veľmi silný:

👉 prepojiť **ADR-038 + ADR-030 (argument model)**
→ ako by mohol vyzerať „argument graph“ bez power biasu

To je ďalší level celej architektúry.
