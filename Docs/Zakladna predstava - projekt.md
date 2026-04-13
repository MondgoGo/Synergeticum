Jasné. Toto si treba ujasniť, lebo inak celý „distribuovaný projekt“ ostáva len magická veta.

Poviem ti to **hrubo, architektonicky, bez implementačných detailov**, ale tak, aby si si to vedel predstaviť fyzicky.

---

# 1. Základná predstava

Projekt v Synergetiku **nie je jeden súbor na jednom mieste**.

Je to skôr:

👉 **distribuovaný živý objekt**,
ktorý má:

* identitu
* stav
* históriu
* vetvy
* podporu
* účastníkov
* argumenty
* zhrnutia

A tento objekt je **uložený vo viacerých uzloch naraz**.

---

# 2. Ako si to predstaviť najjednoduchšie

Predstav si, že projekt je niečo medzi:

* Git repozitárom
* Google Docs dokumentom
* stromom rozhodnutí
* a profilom aktivity

Ale nie na serveri.
👉 **v P2P priestore medzi relevantnými účastníkmi**

---

# 3. Z čoho sa projekt skladá

Hrubo by projekt pozostával z viacerých vrstiev.

## 3.1 Projektové jadro

To je najstabilnejšia časť.

Obsahuje napríklad:

* ID projektu
* názov
* stručný popis
* kto ho inicioval
* typ projektu
* governance režim
* čas vzniku

Toto je niečo ako „hlavička“.

---

## 3.2 Stav projektu

To je aktuálna živá vrstva.

Obsahuje:

* aktuálne vetvy
* ich stavy
* hlavné otázky
* hlavné konflikty
* support signály
* aktívnych participantov

Toto sa mení najčastejšie.

---

## 3.3 História projektu

Sem patrí:

* čo sa pridalo
* čo sa zmenilo
* kedy vznikla vetva
* kto navrhol nový smer
* kedy vetva zoslabla alebo ožila

Toto je chronologická pamäť projektu.

---

## 3.4 Argumentačná vrstva

Pri konceptuálnych projektoch:

* argumenty
* protiargumenty
* dôsledky
* väzby medzi nimi

Nie všetky projekty ju potrebujú rovnako silno.

---

## 3.5 View vrstva

Toto nie je samotný projekt, ale:
👉 **spôsob, ako ho vidí konkrétny človek**

Čiže:

* plný projekt existuje v sieti
* ale používateľ vidí personalizovaný výrez

---

# 4. V akom „formáte“ to existuje

Nie v zmysle technického súboru, ale konceptuálne:

Projekt bude najlepšie chápať ako:

👉 **strom stavov + tok udalostí**

To znamená, že existujú 2 základné možnosti, ako sa na to pozerať.

## 4.1 Snapshot pohľad

Projekt má aktuálny stav:

* toto sú vetvy
* toto je support
* toto je aktivita

To je „fotka teraz“.

## 4.2 Event pohľad

Projekt sa skladá zo série udalostí:

* vznikol projekt
* vznikla vetva A
* user X podporil vetvu A
* user Y pridal protiargument
* vetva B prešla do dormant stavu

To je „príbeh projektu“.

---

Najzdravší model je:

👉 **kombinácia oboch**

Teda:

* projekt má aktuálny stav
* ale ten vzniká z histórie udalostí

---

# 5. Kde to fyzicky „žije“

Toto je najdôležitejšie.

Projekt nežije:

* na jednom serveri
* ani na jednom mobile

Žije:

👉 **na viacerých uzloch, ktoré sú s ním relevantne spojené**

Napríklad:

* iniciátor
* aktívni participant
* parent nody niektorých participantov
* možno niektoré dedikované stále online uzly v trusted priestore

To znamená:

* každý z týchto uzlov má **lokálnu kópiu projektu**
* nie nutne celú do posledného detailu, ale jeho relevantný stav

---

# 6. Ako sa projekt synchronizuje

Nie tak, že by si stále posielal celý projekt sem a tam.
To by bolo neudržateľné.

Synchronizujú sa skôr:

👉 **zmeny**

Napríklad:

* pribudla nová vetva
* niekto pridal argument
* niekto zvýšil commitment
* vetva prešla do dormant

Teda:

* projekt je objekt
* ale sieťou tečú **udalosti / zmeny / delty**

To je dôležité.

---

# 7. Ako môže projekt „prežiť“

Projekt prežije, ak:

* aspoň niektoré uzly držia jeho stav
* a vedia si ho medzi sebou zosúladiť

To znamená:

* iniciátor môže zmiznúť
* projekt nemusí zomrieť

Ak však:

* nikto ho už nedrží
* nikto oň nejaví záujem
* všetky uzly ho pustia

tak projekt prakticky zanikne alebo sa stane čisto archivovaným objektom.

---

# 8. Čo sa stane, keď sú 2 rôzne verzie

Toto je kľúčový problém distribuovaných systémov.

Predstav si:

* ty máš jednu verziu projektu
* ja mám inú
* obaja sme boli chvíľu offline

Systém to nesmie chápať ako „chybu“.

Pri bežných projektoch sa to rieši:
👉 zlúčením zmien

Pri konfliktných zmenách:
👉 branchingom

Teda ak:

* ide o kompatibilné zmeny → spoja sa
* ide o zásadne odlišný smer → vznikne nová vetva

Toto je veľmi elegantné, lebo:

* konflikt neničí projekt
* len ho rozdeľuje na možnosti

---

# 9. Ako si to predstaviť úplne jednoducho

Projekt je ako:

👉 **živý strom, ktorého kópie majú viacerí ľudia naraz**

Každý z nich:

* môže vidieť inú časť
* môže pridať novú vetvu
* môže podporiť niektorú vetvu
* môže pridať argument

A keď sa tieto uzly stretnú:

* porovnajú zmeny
* zosúladia sa
* alebo vytvoria nové vetvy

---

# 10. Akú úlohu má v tomto Personal AI

Tvoja Personal AI nerobí len to, že „uchováva projekt“.

Ona robí hlavne:

* interpretuje projekt pre teba
* vyberá relevantné vetvy
* sumarizuje zmeny
* rozhoduje, čo ti ukáže
* rozhoduje, či sa tvoj uzol zapojí do synchronizácie alebo diskusie

Čiže:

* projekt ako objekt žije v sieti
* ale **projekt ako skúsenosť** žije v tvojej Personal AI

To je veľký rozdiel.

---

# 11. Najlepší hrubý mentálny model

Ak to mám zredukovať na jednu predstavu:

👉 Projekt v Synergetiku je **distribuovaný strom udalostí a stavov**,
ktorý:

* vzniká lokálne,
* šíri sa medzi relevantné uzly,
* synchronizuje sa cez zmeny,
* konflikty rieši vetvením,
* a používateľ ho nikdy nevidí celý, ale cez filter svojej Personal AI.

---

# 12. Čomu sa treba vyhnúť

Určite by som sa vyhol týmto zlým predstavám:

## 12.1 „Projekt je len dokument“

Nie. Dokument je len jedna projekcia projektu.

## 12.2 „Projekt je len chat“

Nie. Chat je len zdroj niektorých udalostí a argumentov.

## 12.3 „Projekt je len súbor“

Nie. Je to živý distribuovaný objekt.

## 12.4 „Projekt má jednu pravú verziu“

Nie. Má históriu, vetvy a synchronizovaný stav.

---

