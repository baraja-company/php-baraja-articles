Algoritmus internetového vyhľadávača - Sotva crawler
====================================================

> id: '5ad042bf-7fda-4b2e-a38c-762169d0b594'
> slug:
> 	cs: vyhledavac-barely-a-crawler
> 	sk: algoritmus-internetoveho-vyhladavaca---sotva-crawler
> 
> publicationDate: '2016-09-11 12:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '5daf3854688c39f9e0071a93ab11dd4e'

V dnešnej lekcii sa budeme venovať dátovým sudom, ich štruktúre, StopSlovu a nakoniec si popíšeme crawlery.

Dátové sudy
-------------

Ide o špeciálny typ údajov, ktorý sa nachádza na viacerých serveroch súčasne vo viacerých kópiách. Zvyčajne ide o dátovo náročné súbory s veľkosťou stoviek GB, ktoré sa pomaly čítajú (preto sú rozdelené na časti) a prakticky sa nedajú upravovať. Ak chceme vykonať čo i len minimálnu zmenu, musíme prepočítať celý sud. Napríklad vyhľadávač Seznam môže prepočítavať sudy s údajmi maximálne raz za niekoľko dní alebo týždňov, zatiaľ čo Google prepočítava raz za niekoľko hodín (a len niektoré časti, nikdy nie celé naraz).

Sudy obsahujú slová a ich výskyty na internete. Dá sa povedať, že ide o surové údaje o výsledkoch vyhľadávania v ešte nezotriedenej podobe (niekedy sú čiastočne zoradené podľa relatívnej dôležitosti) a vopred pripravené. Samotné vyhľadávanie prebieha dlho predtým, ako používateľ zadá vyhľadávací dotaz, a práve tu sa pripravujú všetky výsledky. Samotné vyhľadávanie by nebolo možné v reálnom čase, takže vyhľadávač v podstate číta už pripravené výsledky (vyhľadávanie vykonáva komponent indexer, o ktorom bude reč v samostatnej kapitole).

Sudy obsahujú všetky základné informácie potrebné na vytvorenie výsledkov vyhľadávania pre každé slovo, ktoré sa vyskytuje na internete, takže pri postupnom čítaní sa musia riešiť problémy s výkonom. V praxi sa sudy zvyčajne ukladajú v dávkach na viacerých serveroch a tiež v pamäti RAM na rýchlejší prístup. Napríklad vyhľadávač Google vôbec nečíta sudy z disku, ale uchováva ich kompletne v pamäti RAM a na disku uchováva iba ich kópiu pre prípad straty údajov z pamäte RAM.

Veľké vyhľadávače s dostatočnými zdrojmi majú aj takzvané "zlaté sudy", ktoré obsahujú najdôležitejšie stránky na internete a vyhľadávajú sa v prípade veľkého zaťaženia vyhľadávania. Ak napríklad zlyhá niekoľko vyhľadávacích serverov, zlaté sudy dočasne zabezpečia vyhľadávanie. Vyhľadávač síce nenájde všetky výsledky, ale vyhľadávanie sa aspoň úplne nepreruší, ale dočasne sa obmedzí len počet hľadaných dokumentov. Je tiež potrebné pravidelne aktualizovať hlavne, pretože počet stránok na internete sa neustále zvyšuje. Keďže sudy majú zvyčajne veľkosť rádovo gigabajtov, nie je možné vykonávať inkrementálnu údržbu, ale je potrebné ich raz za čas celé obnoviť. Preto vyhľadávač uchováva pomocný sud, v ktorom sú všetky údaje vo forme veľkej tabuľky, z ktorej sa sudy vytvárajú - napríklad Google vytvára sudy približne každých 12 hodín. Veľkou výzvou je práve to, že sudy musia byť aspoň čiastočne roztriedené a odrážať skutočný stav internetu. Takže kým vyhľadávač neaktualizuje sudy, používateľ nenájde nové webové stránky.

Štruktúra suda
----------------

V praxi sa hlaveň ukladá ako veľký textový súbor, ktorý obsahuje ID každého dokumentu, v ktorom sa vyskytuje hľadané slovo, a pozíciu výskytu aktuálne hľadaného slova.

Príklad veľmi malého súdka s tromi stranami:

[1:5,8,12,27,30;5:1,3,6;12:4,8,10]

Táto vzorka suda obsahuje stránky s identifikátormi `1`, `5` a `12`. Priradené čísla označujú pozíciu výskytu slova v dokumente. Takýto sud zachytáva len výskyty jedného slova, takže každé slovo na internete má svoj vlastný sud. Pri vyhľadávaní viacerých kľúčových slov je potrebné načítať viacero sudov (pre každé slovo iný) a následne vykonať operácie podľa binárneho stromu (viac v predchádzajúcich kapitolách).

Prepojenie sa zvyčajne vykonáva tak, že sa prechádza cez jeden z dokumentov a hľadá sa v druhom. Ak sú sudy veľké, nemusia byť úplne prečítané, takže vyhľadávače často zvládnu prečítať len ich časť a potom musia vrátiť výsledok, preto má zmysel sudy aspoň čiastočne zoradiť tak, aby najdôležitejšie stránky (ktoré používateľ pravdepodobne hľadá) boli na začiatku suda a všetko ostatné na konci suda.

StopLead
---------

Možno si myslíte, že pri niektorých slovách by sudy mohli byť naozaj veľké, a preto by sa s nimi ťažko pracovalo. Napríklad slovo `a` alebo slovo `1` sa nachádza vo viac ako polovici dokumentov, a napriek tomu nemajú pre vyhľadávača žiadny význam (pretože sa hľadajú zriedkavo a vyžadujú viac okolitých slov). Takéto slová sa nazývajú stopslova.

Problém so stopslovami spočíva v tom, že sa nedajú všetky vyhľadať, a preto vyhľadávače zobrazujú len skreslenú realitu toho, ako internet v skutočnosti vyzerá pre hľadané slovo.

Príkladom vyhľadávania pomocou StopSlovy je dotaz `Praha 1`.

Vyhľadávač poskytuje na tento dotaz iba `3 020 000 výsledkov`. V skutočnosti je však takýchto stránok oveľa viac. Nie všetky sa však vyhľadávajú z výkonnostných dôvodov.

Možnosti prístupu k vyhľadávaniu pomocou aplikácie StopSlovy:

- vôbec ich neindexovať a nevytvárať pre ne sudy (robia to malé vyhľadávače)
- ich indexovanie, ale ukladanie len relevantných stránok do sudov (to robí napríklad Google a Seznam).
- indexovať ich ako bežné slová a vytvárať kompletné sudy (toto riešenie má problém, že sudy môžu ľahko presiahnuť veľkosť bežného pevného disku, a preto sa nedajú uložiť a ich čítanie môže trvať niekoľko sekúnd - toto riešenie sa preto v praxi nepoužíva alebo len pre malé a špecializované vyhľadávače)

Vo všeobecnosti neexistuje univerzálne správny prístup a ani spoločnosť Google ho nepoužíva dokonale. Ide o taký veľký problém, že jeho riešenie by mohlo trvať celý život. Na účely tohto dokumentu však tento všeobecný opis postačuje.

Crawler (robot, tiež pavúk)
---------------------------

Crawler je program, ktorý systematicky prehľadáva internet a sleduje, aké slová sa objavujú na akých stránkach, a tiež zhromažďuje pomocné informácie (nazývané aj metainformácie). Crawler zvyčajne beží na mnohých serveroch, pretože prehľadávanie internetu je náročné na šírku pásma siete, a preto sa musí prehľadávať veľa stránok súčasne. Spoločnosť Google odhaduje, že v každom okamihu sa súčasne prehľadáva až 30 000 stránok.

Pred otvorením stránky sa prehliadač Crawler pozrie do databázy adries, na ktoré plánuje v budúcnosti prejsť. Ak už na danej adrese bol, iba aktualizuje svoje záznamy (na hlavné stránky chodí každých niekoľko hodín, na spravodajské stránky každých niekoľko minút, na bežné stránky maximálne raz za mesiac).

Ak robot príde na novú adresu, ktorú nepozná, pozrie sa na obsiahnuté odkazy na iné dokumenty (stránky). Pre každý odkaz vypočíta jeho dôležitosť, a ak ide o dôležitý odkaz, príde na stránku aj neskôr.

Napríklad Zoznam takto prehľadáva len niekoľko úrovní odkazov na každej lokalite, zatiaľ čo Google sa snaží prehľadávať celú lokalitu vrátane nedôležitých dokumentov, čo z neho robí najkomplexnejší vyhľadávač na internete.
Pri prehľadávaní sa robot snaží stránku aj ohodnotiť a vypočíta jej "rank", ktorý číselne vyjadruje, aká je stránka na internete dôležitá. V minulosti spoločnosť Google používala PageRank, ktorý má rozsah od 0 do 10. Spoločnosť Google počíta PageRank 10 len pre seba a stránky, ako sú microsoft.com alebo w3c.org. V súčasnosti sa hodnota počíta inak (umelá inteligencia a vektorová matematika).

Najvyšším PageRankom v Českej republike je Seznam.cz s hodnotou 7. Rank má veľký vplyv na to, ako často robot stránku navštívi a ako vysoko bude vo výsledkoch vyhľadávania. Ranky sa vypočítavajú len zo spätných odkazov smerujúcich na požadovanú stránku.

Crawler s údajmi nič iné nerobí, len ich stiahne z internetu a uloží do databázy, kde čakajú na proces indexovania.
