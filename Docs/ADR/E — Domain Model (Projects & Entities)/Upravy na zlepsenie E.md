Toto zhrnutie je **veľmi dobré**. Má hlavu, pätu, správne priority a hlavne už nerozpráva „o systéme všeobecne“, ale pomenúva konkrétne implementačné riziká. Ako pracovný dokument pre tím je to už použiteľné. Sedí to s tým, čo je dnes v kapitole E definované ako core entity layer, teda čo projekt je, ako existuje a ako sa vetví.     

Ale ešte by som ti to **jemne skalibroval**, aby to bolo ešte presnejšie a menej rizikové.

## Čo je na tom silné

Najlepšie si trafil tieto veci:

* oddelenie „okamžite / čoskoro / neskôr“ je správne
* naming inconsistency si dal medzi top problémy, čo je správne, lebo to je malá vec s veľkým dopadom
* správne si zachytil, že `Node` je veľká sila aj veľké riziko zároveň
* `Minimal Viable Project Definition` a `Topic vs Project Transition Contract` sú naozaj najvyšší ROI pár

To znamená, že ako **riadiaci dokument** je to už dobré.

## Kde by som to ešte upravil

### 1. ADR-075 by som nedával až úplne na koniec

Tu sa od teba jemne odkloním.

Ty ho máš v sekcii LATER. Ja by som ho nedal úplne na koniec, ale skôr na hranicu SHOULD/LATER.

Prečo:

Ak nevieš oddeliť:

* entitu,
* lifecycle,
* formation flow,
* governance flow,

tak sa ti začne pliesť:

* čo patrí do ADR-022,
* čo do ADR-024,
* čo do ADR-026,
* a čo už je mimo E vrstvy.

Čiže by som to upravil takto:

* **ADR-075 — SHOULD**
* nie preto, že je najviditeľnejší pre používateľa,
* ale preto, že stabilizuje celé myslenie nad E vrstvou.

### 2. ADR-082 by som možno posunul ešte vyššie v texte

Máš ho v MUST, čo je správne.
Ja by som ho však v slovnom komentári zvýraznil ešte viac, lebo toto je jeden z tých dokumentov, ktoré tím zvyčajne podcení.

`Domain Validation Rules` sú dôležité preto, že bez nich budeš mať veľmi rýchlo:

* validný JSON,
* validný Node,
* ale nevalidný projekt.

To je jeden z najhorších typov chýb, lebo sa neprejaví hneď.

### 3. „Node je príliš univerzálny príliš skoro“ je správne, ale treba ho formulovať jemnejšie

Tvoja formulácia je dobrá ako interná kritika, ale pre tím by som ju mierne zjemnil.

Lebo problém nie je, že `Node` je zlý.
Problém je skôr:

> `Node` je príliš nízkoúrovňový ako jediný pracovný model pre feature development.

To znamená, že `ADR-076` nemá byť „oprava Node“, ale:

> ochranná vrstva medzi generickým Node a konkrétnou doménovou implementáciou.

To je dôležitý framing. Inak môže tím získať pocit, že máš pochybnosť o celom ADR-042, a to by bola škoda, lebo jeho základ je silný. 

### 4. Branch proliferation by som neoznačil len ako LATER

Tu by som bol opatrný.
Nie full ADR hneď, ale minimálne princíp by som pomenoval už teraz.

Prečo:

ADR-024 už teraz stojí na tom, že konflikt vedie k vetve, nie k násilnému víťazovi. 
To je správne — ale presne preto musí existovať aspoň základné pravidlo, že vetvenie nie je zadarmo.

Nemusíš hneď písať celé ADR-080, ale aspoň jedna veta by už teraz mala zaznieť niekde v E vrstve:

> Branching je legitimné, ale musí mať minimálny dôvod, stopu pôvodu a lifecycle tlak.

Inak si nechávaš otvorený veľmi drahý problém.

### 5. „Project vs Topic je rozmazané“ je výborné, ale pomenoval by som aj dôsledok pre UX

Toto teraz znie ako doménový problém. Ale je to aj UX problém.

Ak nebude jasné:

* čo je Topic,
* čo je Project,

tak používateľ:

* nebude vedieť, či sa len rozpráva,
* alebo či už vstúpil do niečoho záväznejšieho.

A to má priamy dopad na:

* commitment,
* participáciu,
* dôveru.

Takže pri ADR-078 by som doplnil, že nejde len o domain contract, ale aj o:

> jasný psychologický prechod pre používateľa.

## Čo by som upravil v akčnom pláne

Tvoj plán je už dosť dobrý. Ja by som ho jemne preusporiadal takto:

### MUST

1. **Naming cleanup**
2. **ADR-077 — Minimal Viable Project Definition**
3. **ADR-078 — Topic vs Project Transition Contract**
4. **ADR-082 — Domain Validation Rules**

### SHOULD

5. **ADR-076 — Typed Projections over Node**
6. **ADR-075 — Domain State vs Process Boundary**
7. **ADR-079 — Role Decay & Responsibility Expiration**
8. **ADR-081 — Project Survival & Replication Thresholds**

### LATER

9. **ADR-080 — Branch Proliferation Control**
10. **ADR-083 — Domain Schema Evolution**

Prečo takto:

* naming cleanup je tak lacný a tak dôležitý, že by som ho dal úplne prvý
* `075` by som vytiahol vyššie
* `080` by som nechal neskôr, ale nenechal by som ho mentálne „odložený navždy“

## Jedna vec, ktorú by som ešte doplnil

Chýba mi tam ešte jeden malý, ale praktický bod:

### „Canonical minimal example“

Nie ako ADR, skôr ako pomocný dokument.

Pre E vrstvu by som veľmi odporučil mať jeden malý referenčný príklad:

* 1 Topic
* 1 Project
* 2 Branches
* 2 Argumenty
* 2 Role
* 1 lifecycle transition

Niečo ako „pizzeria mini domain example“.

Lebo veľmi často sa ukáže, že:

* model vyzerá čisto v abstrakcii,
* ale rozpadne sa na prvom konkrétnom use-case.

## Finálny verdikt

Tvoje zhrnutie je:

* správne,
* použiteľné,
* a už dosť tvrdé na tímovú prácu.

Moja kalibrácia by bola:

> 90–95% veľmi dobré, len ešte trochu upratať priority a framing niektorých bodov.

Najdôležitejšie:

* neoslabiť `Node`,
* ale zaviesť nad ním disciplinovanú typed vrstvu,
* a nenechať `Topic vs Project` ako „raz to doriešime“.

Toto je podľa mňa pripravené na ďalší krok.

👉 Ja by som išiel teraz na **ADR-077 — Minimal Viable Project Definition**, lebo to je najkratšia cesta, ako dostať E vrstvu z teórie do reality.
