Co bych změnil na Nette, kdybych byl David Grudl
================================================

> id: "bad8fe4f-92a4-4396-8698-7ec42bb2aef8"
> slug:
>   cs: co-bych-zmenil-na-nette
>
> publicationDate: "2022-11-18 17:45:00"
> mainCategoryId: "483db7b7-5699-41fb-ba0b-d2b653bacd1f"

Nette Framework aktivně používám už skoro 6 let pro vývoj komerčního software. Během prvních let jsem byl velmi nadšený a framework dokonale řešil všechny potřeby, které jsme jako tým měli, a v podstatě nebyl důvod hledat jiný nástroj.

Zhruba od roku 2019 začínám v rámci Nette pozorovat určitý opad původního nadšení, kdy mi začínají chybět některé pokročilé funkce. Často to jsou věci nad rámec samotného frameworku, takže ani nečekám, že budou někdy implementovány, na druhou stranu ale musí vývojář udělat rozhodnutí, jak pokračovat při vývoji dál, a třeba sáhnout po jiné technologii.

Databázová vrstva
-----------------

Nette Database považuju za jeden z největších omylů frameworku, bohužel.

Každý týden máme ve firmě pohovory, kam se hlásí junioři hned po škole, nebo i středně pokročilí lidé, kteří přechází z jiného frameworku, a v rámci prozkoumávání Nette objeví také Nette Database, kterou se snaží naučit. Jako databázová vrstva není zas tak špatná, problém pak ale je praktické použití v reálných projektech.

Nette Database se totiž vůbec nesnaží vývojáře už od začátku tlačit k použití striktních datových typů, k pojmenování sloupců a relací, k oddělení metod pro pokládání dotazů (repositáře) od modelů, a další drobné niance, které celkově udělají hodně.

Dlouhé roky jsem používal knihovnu Dibi, která ve své době zahrála obrovskou roli. Umožnila celkem hezky pokládat databázové dotazy a sjednocovala rozhraní. Na druhou stranu doba pokročila, a když databáze vrací netypové objekty, tak prostě už jenom z principu nebude mít PHPStan radost.

Osobně bych do Nette Database buď zapracoval nativní podporu pro entity a repositáře, nebo bych v rámci Nette připravil oficiální podporu pro Doctrine. Pokud programujete aplikace na úrovni velké korporace, musíte vývoj myslet vážně, a zrovna Doctrine dává obrovský smysl. Zároveň chcete nováčky rovnou edukovat v lepší technologii, a nechcete je učit starým návýkům.

Tracy a logování chyb
---------------------

Tracy stále považuji za nejlepší runtime debugger pro PHP.

Když jsem v roce 2017 přišel do nejmenované digitální agentury, a viděl technický stav jejich Nette projektů, zděsil jsem se. Kolikrát měli weby psané v čistém PHP bez jakéhokoli pokusu o loogvání chyb. Celkem snadné řešení bylo nainstalovat Tracy do projektu, zavolat `Debugger::enable()` a nic víc neřešit. Chyby se zalogovaly automaticky do adresáře, který stačilo jednou za pár dnů ručně vybírat.

Co se týče Tracy, přijde mi, že jde o hotový produkt, který David nemá moc důvod zlepšovat. Na druhou stranu tu stále vidím prostor pro zlepšení novým směrem.

Proč například Tracy neobsahuje placené funkce pro firmy nebo jednotlivce, kteří to myslí s bezpečností vážně?

Tracy by mohla implementovat vlastní webové rozhraní typu Sentry, kde si založíte účet, a produkční chyby se budou automaticky odesílat přes API, kde je můžete sledovat napříč projekty. Aplikace by mohla také requestovat jednotlivé weby, a automatizovaně zjistit, že není dostupný, a upozornit majitele jiným způsobem (protože pád serveru Tracy logicky neumí rozpoznat). Někdy se také setkávám s problémem, že je Tracy omylem povolená na produkčním webu, a přes DIC si můžete zjistit přístup k databázi nebo i někam jinam. I na tohle by měla Tracy upozorňovat.

Tracy by mohla také implementovat další drobnosti, které znám z Node.js. Třeba u zobrazení zdrojového kódu by mohlo jít zvolit interval znaků, ve kterém se bude řádek highlihtovat speciálním způsobem, proč možnost zvýraznění konkrétního tokenu v rámci řádku. Zároveň by bylo fajn přidat podporu pro druhý (třeba modrý) zvýrazňující řádek, který ukáže kontext v další části aplikace.

Při zobrazení úryvku zdrojového kódu bych se nebál doplnit tlačítko, které by umělo vykreslit celý zbytek kódu ajaxem, abych ho mohl přímo v prohlížeči z callstacku prozkoumávat. Tohle zrovna Symfony umí a je to skvělá vlastnost. Kdyby se to doplnilo i o základní statickou analýzu s možností se proklikávat po metodách, třídách a dědičnosti, dost by nám to v celém týmu ušetřilo práci při debugování.

Pokud callstack prochází balíkem, bylo by fajn u konkrétního souboru zobrazit verzi balíku, s kterou pracuji. Často se mi vyhodí chyba v balíku, který vyvíjím, a mohl bych rovnou vizuálně poznat, že jde o starší verzi.

Statický routing
----------------

V rámci Nette Routeru bych umožnil definovat nový typ tzv. statické routy. Šlo by vyloženě o potomka základní routy, akorát by uměl přijímat jen string-literal, který zároveň nebude regulární výraz, ale rovnou path. Spousta stránek totiž funguje staticky (kontakty, různé články, překvapivě často i homepage), a router musí kvůli tomu pokaždé vyhodnocovat regexová pravidla pro match a složení URL.

Pokud by se statická routa použila například v Latte šabloně, mohla by se v době kompilace šablony bezpečně přeložit na statický řetězec `{$basePath}/path`, takže nebude potřeba při každém requestu sestavovat třeba 100 URL, které jsou v layoutu.

Rozumím tomu, že stejné funkce mohu dosáhnout cachováním na straně šablony, ale systémové řešení bych ocenil o dost víc.

Ve frameworku Next.js jsem úplně propadl konceptu statického routingu. Neexistuje tam nic jako `n:href`, zkrátka musíte URL vědět dopředu, což vás nutí nedělat tolik redirectů, formáty URL si předem promyslet, a zachovat je trvalé.

PSR rozhraní
------------

Přijde mi škoda, že Nette neimplementuje nativně PSR rozhraní. Jako vývojáři balíčku vás to vede na scestí, jestli chcete na Nette vůbec definovat Composer závislost. Na jednu stranu vám Nette řeší řadu problémů, na druhou stranu víte, že ho pak jednoduše nevyměníte. Nejednou se mi už stalo, že jsem kvůli chybějícímu obecnému rozhraní raději použil Symfony, Laravel, nebo úplně jiné obecné rozhraní.

Chybějící podpora pro obecné standardy pro budoucí přenositelnost je hlavní argument, proč jsem v roce 2022 od Nette úplně odešel a nové aplikace v týmech vyvíjíme výhradně obecně.

API vrstva
----------

Co když chcete Nette použít jako backend pro odbavování REST API požadavků? Postavíte si presenter a budete response odesílat jako `$this->sendJson()`? Nainstalujete si knihovnu třetí strany? Nebo jste David Grudl, který potřebu chybějící knihovny řeší tak, že si rovnou napíše novou?

Proč Nette nemůže už přímo v základu implementovat tak častou věc, jako je REST API? Dnes téměř každá aplikace potřebuje komunikovat přes API - například pro poskytování dat frontendu.

U nováčků to opět vede k tomu, že raději použijí vestavěné Latte a snippety, než aby zjistili, že existuje lepší možnost, a to postavit celý frontend odděleně vedle, a jenom mu poskytovat data.

Důvěra, co bude za 10 let
-------------------------

Poslední bod je hodně pragmatický, a vychází z debaty, kterou v týmu čas od času slyšíme od oddělení byznysu.

Co by se stalo v případě, že by David Grudl přestal úplně Nette vyvíjet? Co kdyby ho přestal vyvíjet kdokoli? Na co bychom přešli? A jak by to bylo náročné?

Této obavě se řada vývojářů (často bez korporátní zkušenosti) rádi vysmívají, že to je přeci v pohodě. Jako jo, ale taky ne.

Když ve firmě řešíte otázku, jestli investovat čas pro vzdělávání juniorů do Nette, nebo do Reactu a Node.js, tak bohužel vyhraje druhá možnost.

Nette ještě vlak neujel, ale na těch desítkách pohovorů, které jsem za poslední půlrok zažil, to celkem silně cítím.
