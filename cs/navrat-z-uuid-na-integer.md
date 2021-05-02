Návrat z UUID na integer
========================

> id: "d842517b-c4f6-4cef-a839-d08ff3804fca"
> slug:
> 	cs: navrat-z-uuid-na-integer
> 
> publicationDate: "2021-05-02 17:00:00"
> mainCategoryId: "95374429-e651-46bd-9149-15aa716f8207"

Při vývoji softwaru dojde programátor celkem často na scestí, kdy stojí před architektonickým rozhodnutím, které bude mít obrovská dopad na budoucnost jeho práce na příští desítky let. Zároveň jde o rozhodnutí, které nelze vzít zpět, a za každou chybu se draze platí. Typickým příkladem architektonického rozhodnutí, kde při každé drobné chybě padají hlavy, jsou databáze.

Jedno z velkých rozhodnutí poslední doby bylo, jakým způsobem ukládat primární klíče v databázových tabulkách. Ačkoli se to zdá jako triviální problém, tak je za tím mnohem víc, než si možná myslíte.

Možnosti primárních klíčů
-------------------------

Základní možnosti jsou v zásadě čtyři:

- integer
- integer unsigned
- big int
- <a href="/uuid-performance">UUID</a>

Integer je prostě celé číslo (v případě `unsigned` pak bez znaménka, tedy vždycky kladné, a v případě `big int` pak může dosahovat extrémně velkých hodnot). Velmi jednoduchý koncept. UUID je pak textový řetězec (například ve tvaru `c4a760a8-dbcf-5254-a0d9-6a4474bd1b62`), který se skládá z několika částí, přičemž každá z nich může mít určité vlastnosti a hodí se to pro budování obrovských multiserverových nebo decentralizovaných aplikací. kolem UUID je velký ekosystém užitečných technologií, který řeší problémy, které možná ani nevíte, že máte, nebo v budoucnu budete mít.

Používej správné kladívko
-------------------------

Před nedávnou dobou (zima 2020) mi kamarád Pavel vysvětloval koncept použití vhodného řešení na problém o rozměru daného typu. To je skvělá a důležitá myšlenka, na kterou řada vývojářů ráda zapomíná - vznikají pak obrovsky komplexní řešení, i když to není potřeba. V angličtině pro to máme krásné slovní spojení **over-engineering**.

Velikost a unikátnost UUID
--------------------------

Zásadní výhoda UUID tkví v tom, že když aplikace příliš vyroste a databázi rozdělíme na mnoho webových serverů, kdy bude jedna databázová tabulka tak obrovská, že se nevejde na disk jednoho stroje, tak ji můžeme rozdělit mezi mnoho fyzických strojů, přičemž každý z nich bude znát svůj kousek tabulky a na zbytek se doptávat svých kolegů. UUID také řeší zásadní problém s insertem nových řádků, kdy v případě mimořádně rozsáhlých aplikací potřebujeme zapisovat řádky paralelně na mnoham místech a nechceme čekat, až se uvolní zapisovací kapacita hlavního serveru.

Konceptu zapisování na mnoha místech zároveň využívají například chatovací aplikace. Když pošlete zprávu přes Messenger, tak putuje na nejbližší databázový server Facebooku, který zprávě přidělí UUID, časovou značku a zapíše ji do své lokální databáze. Váš kamarád z druhé strany světa zase zapisuje zprávy do jeho lokálního datového centra a v mezičase celá cloudová infrastruktura zajišťuje synchronizaci napříč celým světem. Zní to cool, že? :)

Aby takové paralelní zapisování mohlo fungovat, je potřeba vyřešit problém s kolizí záznamů. Pokud by jednotlivé lokální databáze používaly obyčejný integer, tak se velmi brzy stane, že dva nezávislé servery zapíší dva různé záznamy pod stejným identifikátorem. Při synchronizaci těchto záznamů pak vznikne kolize. Řešení obvykle neexistuje - ID nemůžete přečíslovat, protože na to mohou vést další relace.

UUID toto řeší například tak, že každý server má svůj domluvený prefix, který vkládá na začátek každého UUID, poté vloží časovou značku a následně samotný identifikátor.

> **Zajímavost:** Při zapisování tak obrovského objemu dat nás až tak moc nezajímá, kdy byl jaký záznam zapsán, ale v jakém to bylo pořadí (abychom například uživateli neprohodili pořadí zpráv).

Možná se ptáte, jak řešit situaci, kdy se servery nemohou navzájem domluvit, jaký prefix má jaký z nich použít. Tento problém totiž vzniká například u decentralizovaných nebo offline aplikací. V takovém případě lze UUID dokonce generovat náhodně.

Otázkou pak je, jaká je šance, že při <a href="https://stackoverflow.com/questions/1155008/how-unique-is-uuid">náhodném generování UUID nastane konflikt</a>. No, pravděpodbně se vám to nestane. Unikátních UUID existuje přibližně `2^122` (protože jde o `128-bitové číslo`). V praxi je šance na konflikt přibližně `0.00000000006 (6 × 10−11)`. V praxi to znamená to, že pokud bychom po dobu následujících 100 let každou sekundu vygenerovali **1 miliardu UUID**, tak bude šance na konflikt `50 %`. Konflikt tedy spíše nenastane a UUID je definitivní řešení vašich databázových problémů.

Je potřeba tak robustní řešení?
-------------------------------

Pokud nevíte, tak odpověď je **ne**.

Při nastavení primárního klíče jako `int` s příznakem `unsigned` existuje `4 294 967 295` možných hodnot (4 miliardy). Porovnání velikostí integerů najdete v <a href="https://dev.mysql.com/doc/refman/8.0/en/integer-types.html">dokumentaci MySql</a>.

Než do jedné tabulky uložíte 4 miliardy záznamů, tak vám pravděpodobně dříve dojde diskový prostor.

Výkon integeru a joinu
----------------------

Integery jsou opravdu velmi rychlé. V MySql pro ně existují nativní optimalizace. Správně fungují indexy (navíc jsou mnohem menší), zabírají jen 4 bajty, velmi rychle se joinují a na většinu případů vám budou stačit.

Pokud řešíte problém s replikací databáze, tak bude možná nejlepší řešení umístit celou databázi do cloudu typu MS Azure a doptávat se ji externě. I při uložení desítek milionů záznamů je rychlost přístupu přes integer ke konkrétnímu řádku v řádu milisekund (na dobře nakonfigurovaném serveru pod 3 ms) a při clusterovaném indexu může být čas dobře udržitelný i při velkém počtu požadavků.

Pokud potřebujete UUID opravdu používat, raději opusťte svět MySql a vyjdete se cestou databáze Postgres, která na rozdíl od MySql má pro <a href="https://www.postgresql.org/docs/9.1/datatype-uuid.html">UUID vlastní datových typ</a>. Práce s joiny je v případě UUID a MySql obrovský problém a při spojování už jenom 3 tabulek (přičemž každá má jen nižší desítky tisíc zánamů) může zpracování celého dotazu trvat i několik stovek milisekund až nizké sekundy. A toto je bohužel problém MySql, který pravděpodobně nevyřešíte.
