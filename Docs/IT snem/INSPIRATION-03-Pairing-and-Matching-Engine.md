### `INSPIRATION-03-Pairing-and-Matching-Engine.md`

# Návrh: Párovací engine pre potreby a príležitosti

## Východisko z pôvodného systému
Pôvodný systém mal silný koncept "Párovania" (Parovanie) – prepojovanie dopytu a ponuky, potrieb ľudí, nápadov, služieb, spolujázd. Išlo o aktívne vyhľadávanie a spájanie komplementárnych entít.

## Návrh pre nový systém
Tento koncept by mal byť implementovaný ako samostatná vrstva alebo engine, ktorý pracuje nad univerzálnym modelom uzlov.

**Opportunity Layer** (System Architecture Overview, bod 6.11) je na to ideálne miesto, ale mal by byť výrazne posilnený:

1.  **Párovanie osôb a projektov**: Na základe Capability Modelu a záujmov používateľa mu Personal AI ponúkne projekty/vetvy, kde môže prispieť.
2.  **Párovanie otázok a expertov**: Ktorí používatelia majú podľa svojho profilu najväčšiu relevantciu k danej otázke?
3.  **Párovanie projektov a zdrojov**: Ak projekt potrebuje financiu, priestor, náradie – engine nájde uzly (iné projekty, miesta, osoby), ktoré to môžu poskytnúť.
4.  **Párovanie argumentov**: Nájdenie protiargumentov alebo podporných argumentov v rôznych projektoch/vetvách.

## Prepojenie na Micro-Federation
Párovací engine by mal fungovať P2P – namiesto centrálneho indexu by Personal AI model posielal anonymizované "dopytové signály" do siete a prijímal "ponukové signály" od iných modelov.

## Príklad
Personal AI používateľa, ktorý má v Capability Modeli vysoké skóre pre "finance" a "project_direction", dostane od Párovacieho enginu notifikáciu: "Projekt X (pizzéria) hľadá niekoho na finančné vedenie. Tvoja úroveň skúseností (odvodená z tvojich predchádzajúcich rozhodnutí) je 8/10."

---

