Online kurz PHP pre začiatočníkov
=================================

> id: '10ecc1cc-8d49-4882-8ecf-114ad74902df'
> slug:
> 	cs: zacatek
> 	sk: online-kurz-php-pre-zaciatocnikov
> 
> perex: 'Výuka PHP online, kurz pro začátečníky. Naučíte se programovat v PHP přímo od senior vývojáře.'
> publicationDate: '2020-02-09 14:27:47'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: f4f8a8d74e09eb298963f7ab8ff00959

PHP je skriptovací jazyk na strane servera určený pre moderné webové aplikácie.

Jazyk PHP ponúka veľmi rýchlu krivku učenia, t. j. za veľmi krátky čas (rádovo týždne) budete schopní pochopiť väčšinu princípov jazyka do takej miery, že budete schopní vytvoriť takmer akúkoľvek jednoduchú webovú aplikáciu s použitím formulárov, používateľských účtov, databázy a mnohých ďalších.

Ďalšou výhodou PHP je jeho masové rozšírenie na takmer všetkých serveroch (na hosting) a neustály vývoj, vďaka ktorému máte istotu, že vaša aplikácia/web bude fungovať všade.

Ako začať?!?
------------

Pred začatím sa uistite, že ste si pripravili nasledujúce veci:

- Mozog, to je veľa o myslení,
- Počítač (alebo server), na ktorom môžete spúšťať svoje skripty,
- Užitočné sú znalosti matematiky alebo nejakej technickej oblasti,
- Vhodné študijné materiály (napríklad táto webová stránka a <a href="https://www.php.net">oficiálna príručka</a>),
- Základné znalosti HTML a CSS,
- Užitočná je aspoň základná znalosť angličtiny (väčšina materiálov je len v angličtine, napríklad oficiálna príručka a webové fóra),
- Znalosť iného programovacieho jazyka je výhodou (veľmi podobného jazyku C/C++, na ktorom je PHP založené),
- Dôrazne odporúčam základné znalosti HTML a CSS, bez ktorých je pochopenie PHP veľmi ťažké.
- Základné softvérové zázemie (líši sa v rôznych systémoch a najlepšie programy **nie sú zadarmo**).

Základný softvér
-----------------

`Počítač so systémom Windows:`
- Akýkoľvek moderný **webový prehliadač**, ktorý ponúka režim ladenia. Osobne používam <a href="https://www.google.com/chrome">Google Chrome</a>.
- Na začiatok stačí lepší **textový editor** so zvýrazňovaním syntaxe. Najlepší na svete je pravdepodobne <a href="https://www.sublimetext.com">Sublime Text</a> (ktorý ponúka pokročilú prácu s ľubovoľným textom v mnohých formátoch, prácu s viacerými kurzormi, regulárnymi výrazmi a vo všeobecnosti je viacúčelovým nástrojom nielen na programovanie). V minulosti som používal český editor <a href="https://www.pspad.com/cz/">PSpad</a> (ktorý v súčasnosti považujem za veľmi zastaraný a nedostatočný pre moderné webové stránky), niektorí ľudia používajú aj <a href="https://www.slunecnice.cz/sw/notepad/">Notepad++</a>.
- Ak to s vývojom myslíte vážne, radšej by som použil plné vývojové prostredie. V práci používam <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, ktorý považujem za najlepší editor na písanie kódu, aký bol kedy napísaný.
- **Webový server**, ktorý dokáže pracovať s PHP, databázou MySql a umožňuje konfigurovať nastavenia. V súčasnosti považujem <a href="https://www.apachefriends.org/download.html">Xampp</a>, čo je predpripravený balík, za najlepšiu voľbu pre Windows.

`Linux (najmä webový server):`
- Akýkoľvek prehliadač, napríklad <a href="https://www.google.com/chrome">Google Chrome</a> alebo Firefox.
- V Ubuntu používam <a href="https://www.sublimetext.com">Sublime Text</a>, obidve aplikácie sú pre začiatok postačujúce.
- Inštalácia **webového servera** je v porovnaní so systémom Windows náročnejšia. Napríklad v Ubuntu je na to určený program <a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">Tasksel</a>, ktorý sa ovláda pomocou Terminálu.
- Ak inštalujete server Linux, stojí za to zvážiť aj <a href="https://www.nginx.com/resources/wiki/">Ngnix</a>.

`Mac:`
- Na Macu sa výborne programuje, je prispôsobený používateľovi.
- Na vývoj na MacBooku Pro používam <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, ktorý považujem za najlepšie vývojové prostredie, a na úpravu bežných textových súborov používam <a href="https://www.sublimetext.com">Sublime Text</a>, ktorý veľmi dobre zvláda veľké súbory.
- Server som si nainštaloval sám cez **Terminál**, čo môže byť pre začiatočníkov náročné, ale existuje nástroj s názvom **Mamp**, ktorý vám umožní kliknúť na všetky veci myšou.

`Staršie odporúčania:`

Od roku 2020 začína byť zrejmé, že všetky problémy so spúšťaním PHP a celých aplikácií možno ľahko vyriešiť prostredníctvom kontajnerov Docker. Ak sa naučíte pracovať s Dockerom, ušetríte stovky hodín v budúcnosti a ľahko začleníte nováčikov do existujúceho projektu.

Časti série
------------

Pre úplný úvod do jazyka PHP som napísal niekoľko článkov, v ktorých prekonáte bariéru začiatočníkov a vklouznete do základov jazyka PHP:

- <a href="/úvod">Úvod do štúdia PHP</a>
- <a href="/prvý-skript">Prvý-skript</a>
- <a href="/principy-premennej-zápisu</a>
- <a href="/cykly">Cykly</a>
- <a href="/how-to-find-out">Ako sa orientovať v kóde</a>
- <a href="/metódy-posielania-dát">Metódy posielania dát</a>
- <a href="/include-file">Include (zostavenie stránok z častí)</a>
- <a href="/podmienky">Podmienky a vetvenie</a>
- <a href="/safe-application">safe-app</a>

Neskôr je však vývoj webu už pomerne zložitý a človek potrebuje naozaj veľa znalostí (alebo aspoň tušiť, že niečo také existuje). Keďže koncepcia celého jazyka a tvorby webu je pomerne zložitá, pripravil som aspoň <a href="/znalosti">základný prehľad znalostí</a>, ktorý postupne dopĺňam a píšem o ňom články.

Pre vývoj komplexných aplikácií odporúčam začať používať <a href="/oop">objektovo orientované programovanie</a>.

Licencia
-------

Tieto materiály poskytujem bezplatne prostredníctvom webovej stránky `php.baraja.cz`, takže sa nesmú používať v žiadnom inom platenom kurze. Texty môžu obsahovať chyby a nepresnosti. Toto nie je oficiálny preklad príručky.

Vyhradzujem si všetky práva na texty (naozaj), a preto je kopírovanie **zakázané**. URL tejto stránky (odkaz tu) a vzorový zdrojový kód môžete používať bez ďalších obmedzení.

Kontakt
-------

Rád sa s vami porozprávam o vývoji webových stránok, rád vám poskytnem všeobecné rady, ale **zložitejšiu prácu považujem za platenú prácu**.

- E-mail: jan@barasek.com
- Osobné <a href="https://www.facebook.com/janbarasek">Facebook</a>

<a href="https://baraja.cz/kontakt">Všetky kontakty</a>
