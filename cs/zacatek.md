PHP online kurz pro začátečníky
===============================

> id: "10ecc1cc-8d49-4882-8ecf-114ad74902df"
> slugCS: zacatek
> perex: "Výuka PHP online, kurz pro začátečníky. Naučíte se programovat v PHP přímo od senior vývojáře."
> publicationDate: "2020-02-09 14:27:47"
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a

PHP je serverový scriptovací jazyk, určený pro moderní webové aplikace.

Jazyk PHP nabízí velmi rychlou křivku učení, tj. za velmi krátký čas (řádově jednotky týdnů) zvládnete pochopit většinu principů jazyka do té úrovně, kdy budete schopni tvořit téměř libovolnou jednoduchout webobou aplikaci využívající formuláře, práce s uživatelskými účty, databázi a mnohé další.

Na PHP je dále výhodou jeho masivní rozšířenost téměř na všech serverech (u hostingů) a neustálý vývoj, díky kterému máte jistotu, že aplikace/web poběží všude.

Jak začít?!?
------------

Do začátku zajistěte tyto věci:

- Mozek, je to hodně o přemýšlení,
- Počítač (případně server), kde můžete spouštět vaše scripty,
- Hodí se znalost matematiky nebo nějakého technického oboru,
- Vhodné studijní materiály (třeba tento web a <a href="https://php.net">oficiální manuál</a>),
- Základní znalost HTML a CSS,
- Hodí se aspoň základní znalost angličtiny (většina materiálů je jenom v angličtině, třeba oficiální manuál a webová fóra),
- Výhodou je znalost jiného programovacího jazyka (hodně podobné je C/C++, z kterého PHP vychází),
- Výrazně doporučuji základní znalost HTML a CSS, bez čehož je pochopení PHP velice obtížné.
- Základní softwarovou výbavu (liší se napříč systémy a nejlepší programy **nejsou zdarma**).

Základní software
-----------------

`Počítač s Windows:`
- Jakýkoli moderní **webový prohlížeč**, který nabízí režim ladění. Já osobně používám <a href="https://www.google.com/chrome">Google Chrome</a>.
- Pro začátek stačí lepší **textový editor** se zvýrazňováním syntaxe. Světově nejlepší je asi <a href="https://www.sublimetext.com">Sublime Text</a> (který nabízí pokročilou práci s jakýmkoli textem v mnoha formátech, práci s více kurzory, regulárními výrazy a obecně jde o víceúčelový nástroj nejen na programování). V minulosti jsem používal český editor <a href="https://www.pspad.com/cz/">PSpad</a> (který momentálně vnímám jako hodně zastaralý a pro moderní weby nedostačující), část lidí používá ještě <a href="https://www.slunecnice.cz/sw/notepad/">Notepad++</a>.
- Pokud to s vývojem myslíte vážně, tak doporučuji spíše použít celé vývojové prostředí. V práci používám <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, který vnímám jako nejlepší editor pro psaní kódu, který byl kdy naprogramován.
- **Webový server**, který umí PHP, MySql databázi a umožňuje konfigurovat své nastavení. Za momentálně nejlepší volbu pro Windows pokládám <a href="https://www.apachefriends.org/download.html">Xampp</a>, což je předem připravený balíček.

`Linux (zejména webový server):`
- Jakýkoli prohlížeč, třeba <a href="https://www.google.com/chrome">Google Chrome</a> nebo Firefox.
- V Ubuntu používám <a href="https://www.sublimetext.com">Sublime Text</a>, oba jsou pro začátek dostačující.
- Instalace **webového serveru** je proti Windows náročnější. V Ubuntu pro to je například program <a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">Tasksel</a>, který se ovládá Terminálem.
- Pokud instalujete linuxový server, tak stojí také za zvážení technologie <a href="https://www.nginx.com/resources/wiki/">Ngnix</a>.

`Mac:`
- Na Macu se programuje skvěle, vychází uživateli vstříc.
- Pro vývoj na MacBooku Pro používám <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, který vnímám jako nejlepší vývojové prostředí, pro úpravu běžných textových souborů zase <a href="https://www.sublimetext.com">Sublime Text</a>, který si výborně poradí s velkými soubory.
- Server jsem si nainstaloval sám přes **Terminál**, což může být pro začátečníky náročné, ale existuje nástroj <a href="https://www.mamp.info/en/">Mamp</a>, ve kterém zvládnete všechny věci naklikat myší.

Díly seriálu
------------

Pro úplné základy s jazykem PHP jsem sepsal několik článků pro překonání začátečnické bariéry a vklouznutí do základních principů PHP:

- <a href="/uvod">Úvod do studia PHP</a>
- <a href="/prvni-script">První script</a>
- <a href="/zasady-promennych">Zásady zápisu proměnných</a>
- <a href="/cykly">Druhy cyklů</a>
- <a href="/jak-se-vyznat">Jak se vyznat v kódu</a>
- <a href="/metody-odesilani-dat">Metody odesílání dat</a>
- <a href="/include-soubor">Include (skládání stránek z kousků)</a>
- <a href="/podminky">Podmínky a větvení</a>
- <a href="/bezpecna-aplikace">Bezpečná aplikace</a>

Později je však vývoj webu již značně komplikovaný a člověk potřebuje opravdu mnoho znalostí (nebo aspoň tušit, že něco takového existuje). Jelikož pojetí celého jazyka a tvorby webu je značně složité, tak jsem připravil aspoň <a href="/znalosti">základní přehled znalostí</a>, který postupně doplňuji a dopisuji články.

Pro vývoj složitých aplikací doporučuji začít používat <a href="/oop">Objektově orientované programování</a>.

Licence
-------

Tyto materiály poskytuji zdarma prostřednictvím webu `php.baraja.cz`, proto nesmí být použity v rámci jakéhokoli jiného placeného kurzu. Texty mohou obsahovat chyby a nepřesnosti. Nejedná se o oficiální překlad manuálu.

Na texty si vyhrazuji veškerá práva (opravdu) a proto je **zakázáno** jejich kopírování. URL tohoto webu (odkazy sem) a ukázkové zdrojové kódy můžete použít bez dalších omezení.

Kontakt
-------

Rád s vámi pokecám o tvorbě webu, rád vám v obecné rovině poradím, ale **na složitější práci je hleděno jako na placenou zakázku**.

- E-mail: jan@barasek.com
- Osobní <a href="https://www.facebook.com/janbarasek">Facebook</a>

<a href="https://baraja.cz/kontakt">Všechny kontakty</a>
