Algoritmus internetového vyhledávače - Třídění a popisovač
==========================================================

> id: ca4aaac5-0cd8-41db-b860-c32661c43d82
> slug:
> 	cs: vyhledavac-trideni-a-popisovac
>
> publicationDate: "2016-09-11 13:00:00"
> mainCategoryId: "367f936c-073f-44bd-b399-30738e93137a"

V této lekci popisu principu internetového vyhledávače pochopíme, jak vyhledávač získané výsledky třídí, popisuje a hodnotí.

Třídění výsledků
----------------

Představme si hotový barel, který je právě připravený na hledacím serveru. Od uživatele přijde náš první vyhledávací dotaz a nyní musíme provést první „hrubé“ třídění, které bude dále upřesněno.

Mějme následující vzorový vstupní dotaz:

```
[O (and) pejskovi (and) a (and) kočičce]
```

Ano, právě v takové podobě získá vyhledávací server zpracovaný dotaz od uživatele a nyní čeká, než se vrátí výsledek. Celkem na to máme méně než `300 ms`, tak rychle :)

V prvním kroku získáme barely plné ID budoucích výsledků (též zvané kandidáty) a operace, které máme provést. Tento načtený barel nemusí být v některých případech kompletní, ale obsahuje pouze hodnoty, které server stihl přečíst z disku za nějaký předem určený časový interval (též zvaný **timeout**). Získáme například následující číselné řady (které jsou částečně setříděny předem):

| Barel | Dokumenty |
|-------|---------|
| o | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| pejskovi | 5,19,23,42,46,58 |
| a | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| kočičce | 9,19,42,57,58,62,68,83 |

Nyní provedeme hrubý průnik všech množin a získáme seznam ID dokumentů, které jsou ve všech barelech stejné. V praxi se prochází nejkratší seznam a hledá se v ostatních.

Společná ID ve všech barelech jsou: „19, 42, 58“, takže budoucí stránka výsledků obsahuje 3 položky v nějakém, ještě neznámém, pořadí. Stále se na dokumenty díváme jako na ID, protože tím šetříme enormní množství dat. Ve výsledcích hledání ovšem uživateli zpravidla nechceme vypsat čísla dokumentů, ale spíše jeho název (nadpis), popis, URL a další informace. To zajistí komponenta „popisovač“.

Popisovač
---------

Z předchozí kapitoly nám zůstal seznam kandidátů na budoucí výsledky. Získali jsme jej v setříděné podobě z hlediska systému, tj. tak, jak je vyhledávač postupně zařadil do indexu. V této chvíli již nemůžeme provést sekvenční čtení rozsáhlých dat, ale musíme sekvenčně projít nějaký typ relační databáze, kde nalezneme další informace pro další typ řazení, které je srozumitelnější pro uživatele.

Výřez databáze může vypadat například takto:


| ID | Titulek | Popisek | URL | Rank |
|----|---------|---------|-----|------|
| 19 | Josef Čapek: Povídání o pejskovi a kočičce | To bylo tenkrát, když pejsek a kočička ještě spolu hospodařili; měli u lesa svůj malý domeček a tam spolu bydleli a chtěli všechno dělat tak, jak to dělají velcí lidé | www.troglodytarium.cz/webz/axf/.../Capek_J_Pejsek_a_kocicka.htm | 72 |
| 42 | Povídání o pejskovi a kočičce | "Dobrá," řekl pejsek a kočička vzala mýdlo a hrnec s vodou, klekla na zem, vzala pejska jako kartáč a vydrhla pejskem celou podlahu. Podlaha byla celá mokrá | www.capek.narod.ru/povidani.htm | 86 |
| 58 | Jak si pejsek s kočičkou dělali k svátku dort| Pejsek měl mít zítra svátek a kočička narozeniny. Děti to věděly a chtěly pejska a kočičku nějak k svátku a k narozeninám překvapiti. Přemýšlely, co by pejskovi a | a.da.mek.sweb.cz/capek.j/dort.htm | 34 |

Výpočet relevance
-----------------

Posledním krokem je výpočet relevance odpovědi. U většiny výsledků stačí relevanci reprezentovat jako jedno číslo s pevným rádiusem, podle kterého budou výsledky setříděny. Podle ranku jsou výsledky opět „hrubě“ setříděny na několik skupin (jejich počet je závislý na rozptylu ranků) a s těmito skupinami se dále pracuje.

Nejdříve se vezmou skupiny s největším rankem, často to stačí pro setřídění první stránky výsledků a nemusíme řešit nic dalšího. Pokud však má více různých dokumentů stejný rank, tak je potřeba provést podrobnější rozbor a vypočítat další pomocné ranky, které pomohou stránku zařadit. Dalším rankem může být například kvalita odkazů, stáří dokumentu, délka titulku, „vzhled“ URL adresy a mnoho dalších faktorů.

Návrat výsledků k uživateli
---------------------------

Hurá! Máme výsledky k dispozici a nyní zbývá jen vykreslit stránku pro uživatele. Balík s výsledky předáme zpět výdejovému serveru, ten výsledky dosadí do předem připravené šablony, kolem stránky vloží reklamní bannery a stránku odešle zpět k hledajícímu uživateli. Zároveň také může provést další pomocné operace, jako je například cachování výsledků. Pokud někdo další bude hledat ten samý dotaz v nějaké blízké době (například za poslední hodinu), tak se výsledky nebudou znovu hledat, ale dojde pouze k jejich přečtení z dočasné paměti – což často pomáhá zvýšit celkový výkon vyhledávače.

Závěr a pohled do sémantiky
---------------------------

Tato práce měla za úkol popsat základní principy toho, jak fungují vyhledávače uvnitř a jak zvládají vyhledat teoreticky neomezené množství dokumentů za stále rozumný čas. Jedná se o obrovské matematické optimalizace, které vznikaly za dlouhé roky v nejlepších programátorských týmech.

Celkový popis detailů je nad rámec této práce. Celé to také komplikuje fakt, že většinu dalších kroků žádný z vyhledávačů podrobněji nepopisuje, protože se jedná o jejich obchodní tajemství.

Důležité je si také uvědomit, že vyhledávače ještě stále nechápou obsah stránek a smysl výsledků a stále se jedná jen o statistické výpočty, založené na hrubém výpočetním výkonu a schopnosti lidí odkazovat na kvalitní texty a tento systém je také dosti zranitelný (příkladem nechť například „Google bomba“, kdy zákeřný webmaster vyhledávači vnutí obsah na dotaz, který je nerelevantní).

Tento problém by částečně měla řešit umělá inteligence, kterou se postupně snaží všechny vyhledávače integrovat. Je to ale stále vzdálená hudba budoucnosti, protože se nejedná o lehký úkol a většinou se jedná jen o zdokonalení statistických metod výpočtů. Příkladem nechť Google graph search, který se snaží rovnou odpovídat na dotaz (i když ve vnitřní struktuře stále jen hledá k předem připravené databázi znalostí).

Až tedy budete příště pokládat dotaz do vašeho oblíbeného vyhledávače, tak si vzpomeňte, kolik náročných činností musel váš vyhledávač provést a kolik TB dat musel přečíst z disku, aby mohl najít právě těch 10 nejlepších dokumentů na váš hledaný dotaz.
