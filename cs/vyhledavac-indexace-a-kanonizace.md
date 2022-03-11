Algoritmus internetového vyhledávače - Indexace a kanonizace
============================================================

> id: daa97e66-e77f-4741-ade2-905c1cfdbd56
> slug:
> 	cs: vyhledavac-indexace-a-kanonizace
>
> publicationDate: "2016-09-11 11:00:00"
> mainCategoryId: "367f936c-073f-44bd-b399-30738e93137a"

V dnešní lekci se budeme věnovat indexaci a kanonizaci dokumentů na internetu.

Indexace
--------

Proces indexace provádí komponenta zvaná indexér. Jedná se o speciálně navržený program, který ze stažených dat (ta data, která stáhl Crawler) udělá speciální datový typ určený k vyhledávání – barely.

Problém indexace je v tom, že nelze dokumenty „chytře“ procházet, ale je nevyhnutelné sekvenční čtení (čtení úplně celého textu od začátku do konce), proto se jedná o náročnou disciplínu a pro tuto činnost používají vyhledávače ty nejvýkonnější servery. Žádná jiná úloha v procesu vyhledávání není právě tak náročná, jako indexování, kdy se z prostých textů stávají indexy.

Mějme například stránku o kočkách, staženou z Wikipedie. Indexér dostane kompletní text stránky a musí odstranit zbytečnosti (například ovládací menu pro uživatele, reklamy, patičky, …) a vypreparovat stránku tak, aby získal prostý text. Textem může například být:

> Kočka domácí (Felis silvestris f. catus) je domestikovaná forma kočky divoké, která je již po tisíciletí průvodcem člověka. Stejně jako její divoká příbuzná patří do podčeledi malé kočky, a je typickým zástupcem skupiny. Má pružné a svalnaté tělo, dokonale přizpůsobené lovu, ostré drápy a zuby a vynikající zrak, sluch a čich.

Ukázka převzata z [Wikipedie](http://cs.wikipedia.org/wiki/Ko%C4%8Dka_dom%C3%A1c%C3%AD).

Vyhledávač nyní z textu vyparsuje jednotlivá slova a zapíše je do jednotlivých barelů. Zapsání do barelů však nemůže udělat přímo (zejména kvůli tomu, že každý barel existuje v mnoha kopiích a také proto, že by se tím narušilo vyhledávání) a proto vytvoří seznam požadavků, jak by měl budoucí barel vypadat a ty si uloží do dočasné paměti. Takový seznam může v našem případě vypadat třeba takto (některá nedůležitá slova lze ignorovat nebo označit za StopSlova):

| ID dokumentu | Slovo | Pozice | Typ       |
|--------------|-------|--------|-----------|
| 1            | Kočka | 1      | Normal    |
| 1            | Domácí| 2      | Normal    |
| 1            | Felis | 3      | Normal    |
| 1            | Je    | 7      | StopSlovo |
| 1            | Domestikovaná| 8| Normal   |

Takový seznam je obrovský (mnohem větší než původní texty), ale i přesto je rychlejší na vyhledávání, protože nemusíme číst veškeré texty sekvenčně, ale můžeme hledat dle indexu v jednotlivých sloupcích a slova z jedné stránky jsou řazena v řádcích za sebou.

Po uplynutí nějakého času (nebo dovršení nějakého počtu dokumentů) indexér přeruší práci s vytvářením tohoto seznamu požadavků (na budoucí barel) a začne data znovu číst a jednotlivé barely znovu sestavovat (tento seznam obsahuje i staré záznamy, které jsou v již funkčním barelu). Pokud přibydou nové záznamy ze známých adres, tak tímto procesem dojde k jejich aktualizaci, u nových dokumentů naopak k jejich zařazení.

Indexér takto tento seznam opětovně projde a pomalu vytvoří nové barely, které obsahují veškeré náležitosti (budou vypadat stejně, jako bylo uvedeno v ukázce v předchozích kapitolách) a po dokončení všech barelů dojde k jejich rozeslání na jednotlivé vyhledávací servery. Aktualizace barelů na serverech je časově náročná (zejména kvůli přesunu enormního množství dat), proto během této operace nejsou servery dostupné a data se aktualizují na různých serverech v různou dobu. To má například za důsledek to, že někteří uživatelé mohou dostat jiné výsledky, protože každý uživatel hledá na jiném výdejovém serveru (kvůli rozkladu zátěže). Po dokončení aktualizace je vše zase v normálu a všichni uživatelé najdou všechny dokumenty stejně.

Proces indexace je pro každý vyhledávač důležitý a ten, který ho dělá nejčastěji a nejpečlivěji, tak ten má nejaktuálnější pohled na internet. Google tuto operaci provádí každých několik hodin, Seznam jednou za týden (a to má milionkrát méně dat).

Kanonizace dokumentů
--------------------

V původním návrhu fulltextového vyhledávače nebylo nic jako kanonizace potřeba, protože internet byl médium, které tvořilo neustále nový obsah. Postupem času však vznikaly duplicity (tj. že na více různých URL se vyskytuje stejný obsah) a vyhledávače se tomu musí přizpůsobit. Typickým příkladem nechť Wikipedie, na které je mnoho článků. Někteří autoři dalších stránek tyto texty převezmou (z části, nebo i kompletně) a tím vytvoří duplicitu. Ve většině případů to však nevadí, protože zdrojová stránka má mnohem vyšší rank (kvalitu odkazů) než plagiát, nicméně se občas může stát to, že bude originál degradovat na úkor piráta.

Vyhledávače se této neřesti musely přizpůsobit a vznikl pojem „kanonizace“, který lze chápat jako „vyřazení“ některých stránek z indexu. Mimochodem díky tomu jsou indexy menší a vyhledávač nemusí zbytečně prohledávat neustále stejný obsah.

Duplicity si každý vyhledávač interně rozděluje na 2 velké kategorie:

Přirozené duplicity
-------------------

Vznikají na základě přirozeného chování internetu a na základě jeho vlastností.

Například na URL `http://mathematicator.cz` bude pravděpodobně stejný obsah jako na URL `http://www.mathematicator.cz` nebo na `http://mathematicator.cz/index.php`, protože se jedná o standardní chování Apache serveru (a internetu obecně).

Pokud je nalezena přirozená duplicita, tak vyhledávač vytvoří tzv. „kanonickou množinu“, což je skupina stránek, z kterých vyhledávač vybere jednoho zástupce, který vystupuje ve vyhledávání. Pokud nějaký odkaz vede na libovolnou stránku z kanonické množiny, tak jeho rank bude automaticky předán hlavnímu zástupci.

Často je dobré vyhledávači pomoci tuto množinu vytvořit a na webu správně nastavit přesměrování, což způsobí lepší pohled vyhledávače na web a hlavní zástupce bude vybrán lépe.

Duplicity vedoucí na plagiát
----------------------------

Problém současného internetu jsou plagiáty. Existence plagiátu jako takových by až tak nevadilo, nicméně uživatelům to vadí zejména z toho důvodu, že na jeden dotaz nachází stále stejné výsledky. Už se vám také někdy stalo, že jste na nějaký dotaz nalezli několik stránek s totožným textem? Právě tomuto chování se snaží vyhledávače zabránit.

Největší problém je v určení toho, která stránka je původním zdrojem – a to strojově. Vyhledávače všechny podobné stránky opět zařadí do kanonické množiny a z té vyberou hlavního zástupce. Pokud se jedná o zdroje z různých domén, tak se na situaci nelze dívat jako na přirozenou duplicitu (a zvolit libovolného kandidáta), ale je nutné všechny stránky kvalitně ohodnotit a vybrat objektivně toho nejlepšího – a ideálně původní zdroj obsahu.

Vyhledávače se často rozhodují podle ranku celé domény a podle síly odkazové sítě na daný dokument, nicméně i to je dosti nespolehlivý přístup. Druhým faktorem také bývá čas vzniku (indexace) dokumentu. Každý vyhledávač si proto eviduje, na kterých stránkách často vzniká nový obsah a tyto stránky často navštěvuje, aby se nové stránky všiml ideálně ihned a proto za zástupce nezvolil nějakou jinou.

Podrobný popis metod toho, jak tento výběr funguje je nad rámec této práce a šla by o tom napsat celá kniha.
