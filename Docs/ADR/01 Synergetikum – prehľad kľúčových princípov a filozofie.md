# 🧭 Synergetikum – prehľad kľúčových princípov a filozofie

---

## 1. Čo je Synergetikum

- Osobná digitálna inteligencia, ktorá pomáha myslieť, rozhodovať sa a spolupracovať.
- Súkromie, vlastníctvo a nezávislosť sú základné stavebné kamene. [1][4]
- Nie je to tradičný asistent zberajúci dáta – je to nástroj na rozšírené myslenie.

---

## 2. Čo systém nerobí (zakázané modely)

| Zakázané | Dôvod |
|----------|-------|
| ❌ Predaj používateľských dát | Porušuje vlastníctvo a dôveru [35] |
| ❌ Reklama založená na profilovaní | Manipulácia, nie pomoc [35] |
| ❌ Skryté tlačenie k jedinému záveru | Porušuje princíp non-manipulation [3] |
| ❌ AI rozhodujúca za človeka | Človek je vždy v rozhodovacej slučke [2] |
| ❌ Paywall na základné funkcie | Základ musí byť dostupný všetkým [35] |

> **Ak systém musí zarábať na dátach používateľa, prestáva byť systémom pre používateľa.** [35]

---

## 3. Kľúčové princípy

### 🔒 Vlastníctvo osobného modelu [1]
- Každý používateľ vlastní svoj AI model.
- Model žije primárne na jeho zariadení (mobil, príp. desktop).
- Žiadny centrálny model → žiadne centrálne profilovanie.

### 🧠 Človek v rozhodovacej slučke (Human-in-the-loop) [2]
- AI pomáha myslieť, nerozhoduje.
- AI sumarizuje, kladie reflexné otázky, ukazuje alternatívy.
- Finálne rozhodnutie je vždy ľudské.

### 🚫 Non-manipulation [3]
- Personalizácia je povolená, ale nesmie skrývať alternatívy.
- Každé odporúčanie musí byť vysvetliteľné.
- Žiadne dark patterns, žiadne tlačenie k akcii.

### 🔐 Privacy-first [4]
- Default: žiadne zdieľanie (Solo režim).
- Zdieľanie je vždy explicitné, voliteľné, pod kontrolou používateľa.
- Dáta ostávajú lokálne, prenášajú sa len agregáty alebo šifrované výstupy.

### 🌊 Graceful degradation (tolerancia voči chaosu) [40]
- Systém funguje aj pri nekvalitných vstupoch, unavených používateľoch, trolloch.
- Kvalita výstupov klesá plynulo, nie náhle.
- Vždy existuje fallback režim (základné odpovede, žiadna personalizácia).

---

## 4. Udržateľnosť (bez predaja dát) [35]

| Zdroj | Príklad |
|-------|---------|
| 🧠 Open-source jadro | Komunitný vývoj, znížená závislosť na jednej entite |
| 🖥️ Parent node služby | Platený hosting, zálohovanie, synchronizácia (bez prístupu k dátam) |
| 🤝 Granty / verejné zdroje | Financovanie z inštitúcií pre spoločenský prínos |
| 🧩 Modulárne platené služby | Pokročilé AI modely, enterprise nástroje (opt-in) |
| 🔁 Reciprocita | Interná ekonomika hodnoty (dobrovoľné zdieľanie, participácia) [36] |

> **Základ systému je vždy plne funkčný bez platenia.** [35]

---

## 5. Čo z toho vyplýva pre záujemcov o systém

### ✅ Systém umožňuje
- Dôveryhodné fungovanie – žiadne skryté profily, žiadna manipulácia.
- Decentralizovanú infraštruktúru – nezávislosť od jedného centrálneho poskytovateľa.
- Platené služby bez prístupu k dátam – hosting, zálohovanie, výkon.
- Modulárne rozšírenia – špecializované funkcie, enterprise nástroje.

### ❌ Systém neumožňuje (a je to súčasť jeho hodnoty)
- Predaj dát.
- Behaviorálne cielenú reklamu.
- Lock-in cez paywall na základné funkcie.
- AI, ktorá rozhoduje za ľudí.

---

## 6. Ďalšie súvisiace princípy (pre úplnosť)

- **Identity** nie sú pseudonymné profily, ale súkromné, voliteľne zdieľané [37].
- **Moc a vplyv** v sieti sa riadi pravidlami, nie náhodou alebo šumom [38].
- **Argumentačný model** toleruje slabé argumenty, ale neblokuje systém [25].
- **Low-friction participácia** znamená, že používateľ nemusí byť disciplinovaný [40].

---

## 7. Zhrnutie

> **Synergetikum je open-source, súkromie-first, osobne vlastnená AI sieť.**
> 
> - Nezarába na dátach, ale na službách.
> - Nerozhoduje za človeka, ale pomáha mu myslieť.
> - Funguje aj v chaose – nie len v ideálnych podmienkach.

---

Tu sú **konkrétne príklady** k jednotlivým princípom – ilustrujú, ako sa filozofia Synergetika premieta do reálneho správania systému.

---

# 📘 Príklady k princípom Synergetika

---

## 🔒 Vlastníctvo osobného modelu [1]

### Príklad 1: Učenie bez odovzdania
**Situácia:** Používateľ Peter často číta články o udržateľnej energetike a klimatických zmenách. Jeho Personal AI si tohto vzor všimne.

**Tradičný systém:** Správa „Peter má záujem o ekológiu“ putuje na server, kde sa pripojí k jeho profilu. Prevádzkovateľ systému má k dispozícii Peterove preferencie.

**Synergetikum:** Personal AI si uloží túto preferenciu **lokálne** do Peterovho modelu. Nikam neposiela raw dáta – ani „čítal článok X“, ani „záujem ekológia“. Keď Peter nabudúce otvorí systém, AI mu sama od seba zoradí relevantné témy vyššie – ale nikto iný o jeho záujme nevie. [4]

### Príklad 2: Synchronizácia s desktopom
**Situácia:** Peter si kúpi výkonný desktop a chce, aby jeho Personal AI bežala aj tam (pre rýchlejšie simulácie).

**Synergetikum:** Peter dá explicitný súhlas na synchronizáciu modelu medzi mobilom a desktopom. Prenáša sa **celý model**, nie raw dáta. Desktop nikdy neodošle kópiu modelu tretiemu uzlu. Keď Peter synchronizáciu vypne, desktop prestane mať prístup k aktualizáciám. [21, 30]

---

## 🧠 Človek v rozhodovacej slučke [2]

### Príklad: Výber dodávateľa
**Situácia:** Peter potrebuje vybrať dodávateľa solárnych panelov. Synergetikum mu pomáha, nerozhoduje za neho.

**AI ponúkne:**
- Sumarizáciu troch recenzií od ľudí, ktorým Peter dôveruje.
- Simuláciu: „Ak zvolíš X, za 5 rokov ušetríš 2000 €, ak Y, tak 1500 €, ale Y má lepšiu záruku.“
- Reflexnú otázku: „Čo je pre teba dôležitejšie – úspora, alebo pokoj pri poruche?“

**AI nesmie:**
- Vybrať dodávateľa a automaticky ho objednať.
- Skryť Y, lebo X je sponzorovaný.
- Povedať: „Podľa tvojho profilu je X najlepší.“ (to by bola manipulácia).

👉 **Finálne kliknutie na „Vybrať dodávateľa“ robí Peter, nie AI.**

---

## 🚫 Non-manipulation [3]

### Príklad 1: Zobrazenie alternatív
**Situácia:** Peter v projekte „Stavebné úpravy kancelárie“ preferuje vetvu „open office“. Personal AI to vie.

**Tradičný systém:** Zobrazí len argumenty pre open office. Peter si myslí, že ostatné možnosti sú slabé.

**Synergetikum:** Zobrazí primárne Peterovu preferovanú vetvu, ale vždy vedľa sekciu **„Čo ešte stojí za zváženie“**:
- „Menej hlučné riešenie – bunky“
- „Flexibilné zóny – hybrid“
- Tlačidlo: **„Zobraziť všetky perspektívy“**

👉 Peter nie je uväznený v bubline. [27]

### Príklad 2: Vysvetliteľnosť odporúčania
**Situácia:** AI odporučí Petrovi článok o tepelných čerpadlách.

**Synergetikum ukáže:** „Toto odporúčam, pretože:
- V posledných 2 týždňoch si čítal 3 články o elektrifikácii vykurovania. [1]
- Dvaja ľudia v tvojom Trusted Circle to označili za užitočné. [36]
- Téma súvisí s tvojím projektom „Dom 2026“.“

👉 Žiadna skrytá optimalizácia na „udržanie pozornosti“. [3]

---

## 🔐 Privacy-first [4]

### Príklad 1: Default Solo režim
**Situácia:** Mária si prvýkrát spustí Synergetikum.

**Synergetikum:** Všetky funkcie fungujú okamžite – offline, bez zdieľania. Nikam sa neodosiela žiadna telemetria, žiadne dáta. Mária musí **výslovne zapnúť** zdieľanie, ak chce pomôcť komunite alebo využiť Trusted Circle. [36]

**Tradičný systém:** „Súhlasom s podmienkami nám dovoľuješ analyzovať tvoje správanie.“

### Príklad 2: Trusted Circle – zdieľanie len agregátov
**Situácia:** Mária a jej 5 kolegov z oddelenia tvoria Trusted Circle. Chcú zlepšiť svoje spoločné odporúčania na projekty bez toho, aby si navzájom videli do citlivých dát.

**Synergetikum:** Každý model vypočíta **lokálne delty** („čo som sa naučil o téme X“), ale odosiela len:
- Anonymizované vektory (embeddings)
- Agregované štatistiky („3 z 6 modelov zvýšili váhu téme Y“)
- Žiadne raw texty, žiadne mená, žiadne pôvodné vety.

**Merge pravidlo:** Lokálny model Márie zoberie externý update, ale len ak je **dostatočne konzistentný** s jej existujúcim modelom. [15, 36]

---

## 🌊 Graceful degradation (tolerancia voči chaosu) [40]

### Príklad 1: Unavený používateľ
**Situácia:** Peter po 12-hodinovej práci otvorí Synergetikum, aby sa rozhodol o výbere softvéru. Je unavený, odpovedá „neviem“ alebo náhodne klika.

**Synergetikum:** Systém nevyžaduje presné odpovede. Ak Peter kliká náhodne, AI postupne **zníži mieru personalizácie**. Výstup prejde z:
- „Presné odporúčanie: Softvér A“ (pri kvalitných vstupoch)
- na „Všeobecný prehľad: Tu sú 3 najčastejšie vyberané možnosti“ (pri nízkych vstupoch)
- na „Základný fallback: Zobrazujem jednoduchý zoznam bez priorít“ (pri úplnom chaose)

👉 **Systém nikdy nepadne, nikdy neodmietne používateľa.** [40]

### Príklad 2: Troll alebo manipulátor
**Situácia:** Do diskusie o projekte vstúpi aktér, ktorý začne pridávať náhodné argumenty, nadávky, alebo opakovane kliká na jednu možnosť, aby skreslil výsledky.

**Synergetikum:** 
- Šum **zníži kvalitu** výstupov pre všetkých, ale **nezrúti systém**.
- Náhodné vstupy majú nízku váhu pri agregácii. [15]
- Ak niekto opakovane posiela nezmysly, jeho vplyv sa postupne **redukuje** (bez nutnosti banu, bez moderátora). [38]
- Systém sa nezlomí – jednoducho prejde do fallback režimu: „Momentálne je veľa šumu, zobrazujem len základné informácie.“

---

## 💰 Udržateľnosť bez predaja dát [35]

### Príklad 1: Parent node ako platená služba
**Situácia:** Mária chce, aby jej Personal AI bežala nepretržite a zálohovala modely. Nemá vlastný desktop.

**Synergetikum:** Mária si predplatí **parent node službu** u poskytovateľa X. Platí mesačný poplatok za:
- 24/7 dostupnosť jej modelu
- Šifrované zálohy
- Synchronizáciu medzi jej zariadeniami

**Poskytovateľ X nemá prístup k jej dátam** – všetko je end-to-end šifrované. [21, 35] Nevidí, čo Mária rieši, nevie jej profil predať ani použiť na reklamu.

### Príklad 2: Platené rozšírenie bez lock-in
**Situácia:** Peter potrebuje pokročilú finančnú simuláciu pre svoj projekt.

**Synergetikum:** Peter si kúpi modul „Finančný analytik“ ako **voliteľné rozšírenie**. Základný systém (otázky, argumenty, projekty) ostáva plne funkčný bez platenia. Peter modul môže kedykoľvek odpojiť – systém neprestane fungovať, stratí len špecifickú funkciu.

👉 **Žiadny paywall na základné funkcie.** [35]

### Príklad 3: Open-source jadro
**Situácia:** Firma, ktorá vyvíja Synergetikum, skrachuje.

**Synergetikum:** Core systém je open-source. Komunita vývojárov môže pokračovať vo vývoji, opravovať chyby, prevádzkovať parent node služby. Systém **prežije** bez pôvodnej organizácie. [35]

---

## 🔁 Reciprocita [36] – interná ekonomika hodnoty

### Príklad: Dobrovoľné zdieľanie znalostí
**Situácia:** Peter odpovedal na 20 otázok v komunite o fotovoltike. Jeho model sa tým mierne zlepšil (naučil sa, čo je kvalitná odpoveď).

**Synergetikum:** Keď Peter nabudúce potrebuje pomoc s tepelným čerpadlom, jeho Personal AI dostane **vyššiu prioritu** pri zdieľaní odpovedí od komunity – nie preto, že by systém skóroval „dobrý občan“, ale preto, že jeho model je teraz lepšie vyladený vďaka predchádzajúcej participácii. [36]

👉 **Nejde o karmu, tokeny ani reputačné skóre** – ide o emergentnú výhodu z aktívneho používania systému.

---

## 🧠 Zhrnutie príkladov

| Princíp | Jedna veta |
|---------|-------------|
| Vlastníctvo | Dáta a model ostávajú na zariadení používateľa. |
| Human-in-the-loop | AI ukazuje cesty, človek vyberá. |
| Non-manipulation | Alternatívy sú vždy viditeľné, odporúčania vysvetliteľné. |
| Privacy-first | Default je Solo režim, zdieľanie výslovné. |
| Graceful degradation | Systém funguje aj pri unavených a trolloch – horšie, ale stále. |
| Udržateľnosť | Platia sa služby (hosting, rozšírenia), nie dáta. |
| Reciprocita | Pomáhaním komunite si nepriamo pomáhaš – bez bodov. |

---
