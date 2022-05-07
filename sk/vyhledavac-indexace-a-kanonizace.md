Algoritmus internetových vyhľadávačov - indexovanie a kanonizácia
=================================================================

> id: daa97e66-e77f-4741-ade2-905c1cfdbd56
> slug:
> 	cs: vyhledavac-indexace-a-kanonizace
> 	sk: algoritmus-internetovych-vyhladavacov---indexovanie-a-kanonizacia
> 
> publicationDate: '2016-09-11 11:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d73a30f413e4b8b77dbceda60b3cf173

V dnešnej lekcii sa budeme venovať indexovaniu a kanonizácii dokumentov na internete.

Indexovanie
--------

Proces indexovania vykonáva komponent nazývaný indexátor. Ide o špeciálne navrhnutý program, ktorý zo stiahnutých údajov (údajov, ktoré stiahol Crawler) vytvorí špeciálny typ údajov na vyhľadávanie - sudy.

Problémom indexovania je, že nie je možné "inteligentne" prechádzať dokumenty, ale sekvenčné čítanie (čítanie celého textu od začiatku do konca) je nevyhnutné, takže ide o náročnú disciplínu a vyhľadávače na túto činnosť používajú najvýkonnejšie servery. Žiadna iná úloha v procese vyhľadávania nie je taká náročná ako indexovanie, pri ktorom sa z obyčajného textu stáva index.

Vezmite si napríklad stránku o mačkách, stiahnutú z Wikipédie. Indexovač získa kompletný text stránky a musí odstrániť nepotrebné veci (napr. používateľské ovládacie menu, reklamy, päty, ...) a analyzovať stránku, aby získal čistý text. Text by mohol byť napríklad takýto:

> Mačka domáca (Felis silvestris f. catus) je domestikovaná forma mačky divej, ktorá je spoločníčkou ľudí už tisíce rokov. Rovnako ako jej voľne žijúca príbuzná patrí do podčeľade malých mačiek a je typickým zástupcom tejto skupiny. Má štíhle a svalnaté telo, dokonale prispôsobené na lov, ostré pazúry a zuby, výborný zrak, sluch a čuch.

Prevzaté z [Wikipedia](http://cs.wikipedia.org/wiki/Ko%C4%8Dka_dom%C3%A1c%C3%AD).

Vyhľadávač teraz analyzuje jednotlivé slová z textu a zapisuje ich do jednotlivých sudov. Nemôže však vykonať zápis suda priamo (najmä preto, že každý sud existuje vo viacerých kópiách a tiež preto, že by to prekážalo pri vyhľadávaní), preto vytvorí zoznam požiadaviek na to, ako by mal budúci sud vyzerať, a uloží ich do dočasnej pamäte. V našom prípade by takýto zoznam mohol vyzerať takto (niektoré nedôležité slová možno ignorovať alebo označiť ako stopslova):

| ID dokumentu | Slovo | Pozícia | Typ |
|--------------|-------|--------|-----------|
| 1 | Cat | 1 | Normal |
| 1 | Domáce| 2 | Normálne |
| 1 | Felis | 3 | Normálne |
| 1 | Is | 7 | StopWord |
| 1 | Domestikované| 8 | Normálne |

Takýto zoznam je obrovský (oveľa väčší ako pôvodné texty), ale napriek tomu je vyhľadávanie rýchlejšie, pretože nemusíme čítať všetky texty postupne, ale môžeme vyhľadávať podľa indexu v každom stĺpci a slová z jednej stránky sú zoradené v riadkoch za sebou.

Po uplynutí určitého času (alebo po dokončení určitého počtu dokumentov) indexátor prestane pracovať na vytváraní tohto zoznamu požiadaviek (pre budúci sud) a začne znovu čítať údaje a obnovovať jednotlivé sudy (tento zoznam obsahuje staré záznamy, ktoré sú v už pracujúcom sude). Ak sa pridajú nové záznamy zo známych adries, tento proces ich aktualizuje, zatiaľ čo v prípade nových dokumentov ich zahrnie.

Týmto spôsobom indexátor opäť prejde zoznam a pomaly vytvorí nové sudy, ktoré budú obsahovať všetky prvky (budú vyzerať rovnako ako v príklade v predchádzajúcich kapitolách), a keď budú všetky sudy dokončené, odošlú sa na jednotlivé vyhľadávacie servery. Aktualizácia sudov na serveroch je časovo náročná (najmä kvôli obrovskému množstvu presúvaných údajov), preto počas tejto operácie nie sú servery dostupné a údaje sa aktualizujú na rôznych serveroch v rôznom čase. Výsledkom je napríklad to, že niektorí používatelia môžu dostať rôzne výsledky, pretože každý používateľ vyhľadáva na inom výdajovom serveri (v dôsledku rozkladu záťaže). Po dokončení aktualizácie sa všetko vráti do normálu a všetci používatelia nájdu všetky dokumenty rovnako.

Proces indexovania je dôležitý pre každý vyhľadávač a ten, ktorý ho vykonáva najčastejšie a najstarostlivejšie, má najaktuálnejší prehľad o internete. Google túto operáciu vykonáva každých pár hodín, Seznam raz za týždeň (a má miliónkrát menej údajov).

Kanonizácia dokumentov
--------------------

Pri pôvodnom návrhu fulltextového vyhľadávača nebolo potrebné nič také ako kanonizácia, pretože internet bol médiom, ktoré neustále vytváralo nový obsah. Postupom času však dochádza k duplicite (t. j. k tomu, že sa rovnaký obsah objavuje na viacerých rôznych adresách URL) a vyhľadávače sa tomu musia prispôsobiť. Typickým príkladom je Wikipédia, ktorá obsahuje mnoho článkov. Niektorí autori iných stránok tieto texty preberajú (čiastočne alebo dokonca úplne) a vytvárajú tak duplicitu. Vo väčšine prípadov to však nevadí, pretože zdrojová stránka má oveľa vyššiu pozíciu (kvalitu odkazov) ako plagiát, ale niekedy sa môže stať, že to znehodnotí originál na úkor piráta.

Vyhľadávače sa museli tomuto zlozvyku prispôsobiť a vznikol pojem "kanonizácia", ktorý možno chápať ako "odstránenie" určitých stránok z indexu. Mimochodom, vďaka tomu sa zmenšujú indexy a vyhľadávač nemusí zbytočne prehľadávať stále ten istý obsah.

Duplikáty sú v každom vyhľadávači interne rozdelené do 2 veľkých kategórií:

Prírodné duplikáty
-------------------

Tie vznikajú na základe prirodzeného správania sa internetu a jeho vlastností.

Napríklad adresa URL `http://mathematicator.cz` bude mať pravdepodobne rovnaký obsah ako adresa URL `http://www.mathematicator.cz` alebo `http://mathematicator.cz/index.php`, pretože ide o štandardné správanie servera Apache (a internetu všeobecne).

Ak sa nájde prirodzená duplicita, vyhľadávač vytvorí "kanonickú sadu", čo je skupina stránok, z ktorej vyhľadávač vyberie jedného zástupcu, ktorý sa vo vyhľadávaní vyníma. Ak odkaz vedie na niektorú stránku z kanonickej sady, jej hodnotenie sa automaticky prenesie na hlavného zástupcu.

Často je dobré pomôcť vyhľadávaču vytvoriť túto sadu a správne nastaviť presmerovanie na stránke, čo spôsobí, že vyhľadávač sa na stránku lepšie pozrie a hlavný zástupca bude lepšie vybraný.

Duplikáty vedúce k plagiátorstvu
----------------------------

Plagiátorstvo je dnes na internete problémom. Existencia plagiátorstva ako takého by nemala až taký význam, používateľov však trápi najmä preto, že na rovnaký dotaz nachádzajú stále rovnaké výsledky. Tiež ste niekedy našli niekoľko stránok s identickým textom pre daný dotaz? Práve takémuto správaniu sa vyhľadávače snažia zabrániť.

Najväčším problémom je určiť, ktorá stránka je pôvodným zdrojom - a to strojovo. Vyhľadávače opäť zaradia všetky podobné stránky do kanonickej množiny a vyberú z nej hlavného zástupcu. Ak sú zdroje z rôznych domén, potom sa na situáciu nemožno pozerať ako na prirodzenú duplicitu (a vybrať akéhokoľvek kandidáta), ale je potrebné kvalitatívne zhodnotiť všetky stránky a objektívne vybrať tú najlepšiu - a najlepšie pôvodný zdroj obsahu.

Vyhľadávače sa často rozhodujú na základe hodnotenia celej domény a sily siete odkazov na daný dokument, ale aj to je pomerne nespoľahlivý prístup. Druhým faktorom je zvyčajne aj čas vytvorenia (indexácie) dokumentu. Preto každý vyhľadávač sleduje, ktoré stránky sa často používajú na vytváranie nového obsahu, a tieto stránky často navštevuje, aby si novú stránku v ideálnom prípade okamžite všimol, a preto ju nevybral ako zástupnú.

Podrobný opis metód fungovania tohto výberu presahuje rámec tohto článku a mohol by byť predmetom celej knihy.
