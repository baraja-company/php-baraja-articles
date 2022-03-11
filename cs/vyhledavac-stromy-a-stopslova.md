Algoritmus internetového vyhledávače - Stromy a StopSlova
=========================================================

> id: 11ec73f2-e505-4338-89aa-8307a55b7ba0
> slug:
> 	cs: vyhledavac-stromy-a-stopslova
>
> publicationDate: "2016-09-11 10:00:00"
> mainCategoryId: "367f936c-073f-44bd-b399-30738e93137a"

Na internetu každou sekundu přibyde 5 milionů nových stránek a tato rychlost se neustále zvyšuje. Aby bylo možné tomuto obrovskému moři informací dát nějaký řád a něco v něm naleznout, tak existují vyhledávače. Následující práce má za cíl seznámit s problematikou vyhledávání a vysvětlení celého postupu od vzniku nové stránky až po její nalezení ve vyhledávači.

Úkol nalezení a setřídění množiny miliard dokumentů není vůbec snadný. Samotný Google na tuto disciplínu potřebuje 300 tisíc webových serverů, aby tento úkol zvládl již za několik hodin. Vyhledávání vašeho dotazu totiž probíhá daleko před tím, než jej položíte. Už nyní má Google v paměti uložené výsledky hledání, na které se budete ptát v následujících dnech.

Architektura vyhledávače
------------------------

<img src="{$baseUrl}/images/fulltext-schema.png" alt="Architektura běžného vyhledávače do 50 milionů dokumentů" class="w-100 mb-3">

Správně navržený vyhledávač obsahuje mnoho komponent, kterých je řádově několik stovek. Toto schéma popisuje nejzákladnější prvky, které musí minimálně vyhledávač obsahovat, aby mohl fungovat. Jedná se o starou a již nepoužívanou architekturu, která fungovala přibližně do roku 2005, dokud na webu bylo jen několik milionů dokumentů. Tato architektura nepočítá s „nekonečným“ množstvím obsahu a také s možnou odstávkou vyhledávacích serverů (base search). V případě odstávky byť jediné komponenty by přestal fungovat celý systém a vyhledávač by byl nedostupný.

Kompletní popsání aktuálních trendů v návrhu nových architektur není v silách této práce, protože se zpravidla jedná o obchodní tajemství velkých společností. Cílem tohoto textu bude vysvětlit, jak tato základní architektura pracuje. Jednotlivé komponenty jsou podrobně vysvětleny v dalších kapitolách.

Vstup dotazu
------------

Nejvíce viditelnou komponentou i pro běžné uživatele je právě vstup dotazu, který je v současné době nejčastěji reprezentován jako textové políčko. Vstupní políčko je umístěné na speciální stránce, která je obvykle určena pouze pro vstup.

V další fázi uživatel vloží hledaný řetězec a odešle formulář na servery vyhledávače a tím zahájí komunikaci s výdejovou komponentou, někdy zvanou také Main search. Výdejové servery jsou stavěné na obří zátěže od uživatelů a musí zvládat obsluhovat všechny vyhledávající uživatele naráz.

Architektura výdejového serveru je zpravidla obyčejný Apache server se základní instalací a výbornou datovou propustností do sítě. Při vstupu dotazu na server dojde k jeho zpracování přes regresní rozhodovací stromy a je vytvořena „mapa dotazu“, která vystihuje kompletní sémantiku kolem ostatních významů, aby bylo zřejmé, co uživatel skutečně hledá. Metody přepisu dotazu jsou až příliš nad rámec této práce, proto si jen ukážeme obecné popisy toho, jak může a naopak jak nemůže přepis vypadat.

Mějme následující dotaz: 

```
"O pejskovi a kočičce"
```

Tento dotaz bude převeden na binární strom, který zachycuje jeho sémantickou podstatu:

<img src="{$baseUrl}/images/fulltext-strom.png" alt="Symbolické schéma parsovacího stromu" class="w-100 mb-3">

Podstatou celého stromu je zachycení toho, jak se bude vyhledávač na výsledky hledání dívat. Tento strom je spolu s dotazem rozeslán na pomocné vyhledávací servery (v této práci jsou značené jako Base search), kde dojde k vyhledání jednotlivých klíčových slov (čtení indexů) a následně k setřídění dle pravidel (podle operací).

| Operátor | Třídící operace |
|----------|------------------
| AND	| Průnik	|
| OR	| Součet	|
| NOT	| Doplněk	|

K samotnému řazení výsledků vyhledávání zde nedochází, tato komponenta pouze provádí rychlé binární operace nad obrovským množstvím dat (často i miliony záznamů) a v podstatě se jen porovnávají ID jednotlivých dokumentů.

O tom, jak funguje samotné řazení výsledků vyhledávání pro stránku výsledků budou pojednávat následující kapitoly. Výdejový server slouží pouze ke spojení mnoha dat z různých vyhledávacích serverů, aby se zvyšoval celkový výkon a rozkládala zátěž a následně zjištěné hodnoty vkládá do stránky s výsledky (té se říká „webovka“) a obsahuje složené informace z mnoha desítek serverů.

Přepis na binární strom
-----------------------

Jak již bylo řečeno v předchozí kapitole, tak celé vyhledávání je stavěné na binárních stromech, které říkají, jak budou výsledky vypadat a co se má hledat. Celá „logika“ a „inteligence“ vyhledávače je přímo závislá na tom, jak kvalitně je vytvořen program, co takový strom vytváří – totiž, popisovač.

Správně vytvořený strom by měl obsahovat veškeré potřebné meta informace o dotazu v takové podobě, aby jej bylo možné snadno zpracovat i za pomoci sekvenčního čtení z disku a měl by eliminovat náhodné přístupy do paměti, proto zpravidla obsahuje jednotlivá hledaná slova (od uživatele), jeho přepisy (a vyskloňované tvary) a v neposlední řadě vazby mezi slovy, tedy jejich relativní vzdálenosti v textu.

Metody přepisu dotazu
---------------------

Úplně nejzákladnější přepis dotazu je jeho parsování na jednotlivá slova (podle mezer), s kterými se bude dále pracovat. Druhým krokem je vyhledávání StopSlov a jejich nahrazení za proměnlivý ukazatel (protože jak si později ukážeme, tak nemá smysl StopSlova vůbec vyhledávat).

Mějme následující dotaz: „O pejskovi a kočičce“, jeho přepis v úplně nejzákladnějším stavu vypadá takto:

```
"O (and) pejskovi (and) a (and) kočičce"
```

Druhým krokem je eliminace slov, která nenesou žádný význam pro vyhledávání. Samozřejmě, například slovo „o“ nebo „a“ pro nás lidi smysl má, ale pro hledání v databázi nikoli, protože jej lze nahradit jiným slovem a vypsat výsledky. Cílem nemá být nalezení doslovné shody dotazu vůči indexu, protože by to vedlo na špatné výsledky, protože často bývají slova v textech na internetu v jiných pádech a tato metoda se snaží nalézt výsledky i v jiných tvarech, než jak jsme je získali od uživatele. Jedná se tedy o první náznak „inteligentního“ vyhledávače.

Těmto slovům bez významu se často říká „StopSlova“, každý vyhledávač by si měl vést rejstřík takových slov a tento rejstřík by měl být často aktualizován (ručně).

```
["pejskovi (and) * (and) kočičce"]
```

V tuto chvíli vyhledáváme 2 různá slova („pejskovi“ a „kočičce“), mezi kterými bylo nějaké další slovo (interně jej označíme hvězdičkou).

Smyslem této úpravy není zvýšení kvality hledání, ale zvýšení výkonu. Když totiž hledáme nějaké příliš frekventované slovo, tak dochází k rapidní zátěži a tato úprava tomuto procesu pomáhá – zejména tím, že se nevyhledává slovo, které ve většině dokumentů stejně na této pozici bude k dispozici.

Také je důležité poznamenat, že tuto úpravu nemůžeme udělat vždy (vynechání slova), proto by si každý vyhledávač měl vést samostatný rejstřík frází, které se na internetu vyskytují, aby mohl ověřit, která úprava vede na dobré výsledky a která ne. Občas tuto úpravu ale vyhledávač provede, i když tuto frázi v indexu nemá – a to zejména ve chvíli, kdy uživatel hledá nějaké významné slovo, na které se očekává, že na prvních pozicích budou významné stránky, co tuto „chybovost“ přebijí, protože je uživatelé stejně většinou požadují.

Posledním krokem je práce s češtinou a „čištění“ dotazu od zbytečných znaků. Například na dotazu: „pračka, „ uživatel obvykle očekává výsledky pouze na slovo „pračka“ a znak čárky můžeme ignorovat.

Přesné metody toho, jak tento systém funguje, nejsou známé a každý vyhledávač má své. Předpokládá se, že se jedná většinou pouze o statistický model, který tyto úpravy provádí na základě znalostí miliard textů v databázi.

Přepis češtiny je také věda sama o sobě, a proto zde bude také jen základní popis. Ve většině případů stačí ke každému slovu přidat jeho vyskloňované tvary (podle slovníku) a ty vyhledávat společně se slovem, čímž docílíme efektu, že vyhledávač zvládá nalézt i dokumenty, kde se slova nevyskytují pouze v základním tvaru (tak, jak je zadal uživatel), ale nalezne i skloněné verze – což je velice užitečná vlastnost. Problém tohoto přístupu je ve skloňování nejasných a problémových slov a také ve skloňování dotazů, které jsou krátké (a nelze proto z kontextu určit, jak se má slovo sklonit). Příkladem problémového sklonění nechť slovo „her“.

Mějme anglický dotaz: „I love her“, špatně navržený skloňovač slovo „her“ vyskloňuje například jako: „hry, herní, herna, …“, proto je také potřeba indexovat okolní slova jako fráze a provádět skloňování jen ve známých situacích, nebo použít jiný postup (zde často nastupují fonetické algoritmy).

Posledním krokem je odeslání hotového stromu na Base search servery, kde proběhne samotné vyhledávání v barelech – o tom bude následující kapitola.
