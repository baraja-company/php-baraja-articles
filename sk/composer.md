Composer - kompletný prehľad pokročilých funkcií
================================================

> id: a74d8d59-91ce-4602-ad52-80cf89a647bd
> slug:
> 	cs: composer
> 	sk: composer---kompletny-prehlad-pokrocilych-funkcii
> 
> perex: Composer je pokročilý správce balíků a závislostí pro vaše PHP aplikace. Článek popisuje jeho všechny výhody a možnosti použití.
> publicationDate: '2020-03-10 20:18:19'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '68340d4b4d3c8a6ed143ede176fbf04e'

Ako už viete, [Composer](https://getcomposer.org/) je robustný správca balíkov a závislostí pre PHP, pomocou ktorého môžete elegantne spravovať stovky projektov naraz a distribuovať raz napísaný kód do všetkých aplikácií súčasne.

Tento návod slúži ako podrobná komplexná príručka pre vývojárov. Budeme sa venovať všetkým dôležitým pokročilým technikám práce s programom Composer a vysvetlíme technické detaily a príslušné závislosti.

Inštalácia aplikácie Composer
-------------------

Bez ohľadu na platformu sťahujeme z [oficiálnej webovej stránky Composer](https://getcomposer.org/).

Interne sa stiahne súbor PHP `composer-setup.php`, ktorý po spustení v režime CLI môže nainštalovať Composer. Dôležité je tiež vedieť, že Composer nefunguje bez PHP, preto si najprv overte, či máte v počítači PHP spustené (musí byť dostupné len pre terminál).

V systémoch Mac a Linux funguje Composer hneď po inštalácii a stačí zavolať príkaz `composer -v`, aby ste rýchlo overili, či je Composer správne nainštalovaný.

V Linuxe možno na inštaláciu použiť nasledujúci príkaz: `/usr/bin/php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"`

V systéme Windows je dobré nainštalovať nástroj [Git Bash for Windows](https://gitforwindows.org/), ktorý umožňuje otvoriť špecifický terminál, ktorý sa správa takmer ako Linux a umožňuje pracovať v rovnakom prostredí ako v Linuxe.

Inštalácia pre servery je rovnaká ako v miestnom prostredí. Len sa uistite, že máte správnu verziu PHP, ktorú Composer interne používa.

Dostupné príkazy
----------------

Composer implementuje niekoľko príkazov.

Použitie je: `composer <príkaz>`, napríklad: `composer update`.

Prehľad pre verziu `1.10.0`:

| Príkaz | Popis / Význam |
|-----------------------|----------------|
| `o` | Zobrazí stručné informácie o programe Composer.
| `archive` | Vytvorí archív s obsahom vybraného balíka Composer.
| `browse` | Otvorí domovskú stránku vybraného balíka, autora alebo inú súvisiacu stránku vo webovom prehliadači. Často môže obsahovať dokumentáciu o tom, ako ho používať.
| `cc` | Vymaže internú vyrovnávaciu pamäť programu Composer s verziami balíkov stiahnutých v minulosti.
| `check-platform-reqs` | Skontrolujte, či sú splnené požiadavky na inštaláciu pre aktuálnu platformu.
| `clear-cache` | Vymažte internú vyrovnávaciu pamäť programu Composer.
| `clearcache` | Vymazať internú vyrovnávaciu pamäť programu Composer.
| | `config` | Nastavenie konfiguračnej smernice.
| `create-project` | Vytvorí nový projekt na základe vybraného balíka a automaticky vytvorí priečinok, do ktorého sa projekt umiestni.
| `Závisí` | Zobrazí, ktoré balíky spôsobili inštaláciu vybraného balíka.
| `diagnose` | Diagnostikuje systém a identifikuje bežné chyby. Spracovanie výstupu je na vývojárovi, je to len výpis.
| `dump-autoload` | Generuje nový <a href="/autoloading-trid">autoloader</a>.
| `dumpautoload` | Generuje nový <a href="/autoloading-trid">autoloader</a>.
| `exec` | Spúšťa binárne súbory a skripty od dodávateľa.
| `fund` | Skúma, ako vytvoriť/zmeniť svoje závislosti.
| `global` | Umožňuje spúšťať globálne príkazy Composeru z premennej `$COMPOSER_HOME`.
| `help` | Vypíše nápovedu pre príkazy.
| `home` | Otvorí domovskú stránku konkrétneho balíka v prehliadači.
| `i` | Nainštaluje všetky závislosti projektu podľa súboru `composer.lock`, ak existuje a je platný. Ak nastane problém, použijú sa informácie zo súboru `composer.json` a súbor `composer.lock` sa vráti do pôvodného stavu.
| `info` | Zobrazí informácie o aktuálne nainštalovaných balíkoch v projekte. Zobrazí názvy všetkých balíkov, ich aktuálne verzie a krátky popis.
| `init` | Vytvorí základnú funkciu `composer.json` v aktuálnom adresári.
| `install` | Nainštaluje všetky závislosti projektu podľa súboru `composer.lock`, ak existuje a je platný. Ak nastane problém, použijú sa informácie zo súboru `composer.json` a súbor `composer.lock` sa vráti do pôvodného stavu.
| `licenses` | Zobrazí zoznam všetkých balíkov, ich verzií a aktuálnych licencií.
| `list` | Zobrazí zoznam dostupných príkazov.
| | `outdated` | Zobrazí zoznam všetkých balíkov, pre ktoré je k dispozícii novšia verzia na inštaláciu a ktoré spĺňajú závislosti. Pre každý balík sa zobrazí najnovšia kompatibilná verzia, ktorú Composer navrhuje na inštaláciu.
| `Zakázané` | Zobrazí, ktoré balíky a závislosti bránia inštalácii požadovaného balíka alebo verzie.
| `remove` | Odstráni balík z konfiguračnej sekcie `require` alebo `require-dev`. |
| `require` | Pridá požadovaný balík do súboru `composer.json` a nainštaluje ho. Ak sa závislosti nedajú splniť, vráti sa do pôvodného stavu.
| `run` | Spustí skripty definované v súbore `composer.json`. |
| `run-script` | Spustí skripty definované v súbore `composer.json`. |
| `search` | Vyhľadáva balíky podľa kľúčového slova alebo vyhľadávacieho dotazu.
| `self-update` | Aktualizuje interný súbor `composer.phar` na najnovšiu verziu.
| `selfupdate` | Aktualizuje interný súbor `composer.phar` na najnovšiu verziu.
| | `show` | Zobrazí podrobné informácie o aktuálne nainštalovaných balíkoch.
| `status` | Zobrazí prehľad lokálnych zmien vykonaných v balíkoch ručne na základe porovnania so zdrojom balíka, z ktorého bol pôvodne nainštalovaný. |
| `suggests` | Zobrazí návrhy balíkov. Návrhy môžu zahŕňať rôzne druhy činností, napríklad inštaláciu aktualizácií zabezpečenia a podobne.
| `update` | Aktualizuje celý projekt podľa závislostí tak, aby boli vždy všetky splnené v súbore `composer.json`. Ak sa to podarí, aktualizuje sa súbor `composer.lock`, do ktorého sa zapíšu aktuálne nainštalované verzie.
| `upgrade` | Alias na `update`. |
| `u` | Alias na `update`. |
| `validate` | Kontroluje súbory `composer.json` a `composer.lock` na syntaktické chyby.
| `why` | Zobrazí, ktoré balíky spôsobili inštaláciu aktuálne vybraného balíka vrátane všetkých závislostí.
| `why-not` | Zobrazí, ktoré balíky a verzie bránia inštalácii vybraného balíka alebo verzie.

Vytvorenie a definovanie projektu
----------------------------------

Každý projekt spravovaný programom Composer je definovaný súborom `composer.json` v jeho koreňovom adresári, ktorý definuje všetky závislosti. Súbor možno vytvoriť buď ručne pre existujúci projekt, alebo automaticky pri vytváraní projektu.

Keďže všetko v aplikácii Composer je balík, aj samotný projekt môže byť založený na balíku. Ak teda napríklad vytvárate desiatky alebo stovky veľmi podobných projektov, má zmysel umiestniť ich základnú konfiguráciu a štruktúru do samostatného balíka, na ktorom bude založená inštalácia.

Príkladom takéhoto balíka je môj [Baraja Sandbox](https://github.com/baraja-core/sandbox), ktorý je založený na čistom Nette 3.0 a pridáva základnú závislosť do môjho [Package Manager](https://github.com/baraja-core/package-manager), ktorý používam na všetky projekty a správu závislostí v konfigurácii Nette.

Sandbox sa potom nainštaluje jednoducho pomocou príkazu:

$ composer create-project baraja/sandbox <your-project-name>

Na základe názvu projektu potom program Composer automaticky vytvorí priečinok na inštaláciu projektu (skopíruje obsah balíka a nainštaluje závislosti).

V priečinku `vendor` potom Composer spravuje všetky balíky (sú tam ich fyzické súbory) a generuje autoload tried, ktoré v ideálnom prípade vložíme priamo do riadku `index.php`:

```php
require __DIR__ . '/vendor/autoload.php';

// Samotný kód aplikácie
```

Inštalácia ďalších balíkov a závislostí
-------------------------------------

V rámci funkčného projektu môžeme veľmi jednoducho inštalovať nové balíky a pridávať závislosti.

Existujú 2 spôsoby, ako to urobiť:

- Príkazom `composer require ...`,
- Pridaním závislosti priamo do súboru `composer.json` v časti `require` a následným použitím príkazu `composer update`.

Skúste nainštalovať napríklad PackageManager: `composer require baraja-core/package-manager`, alebo [Doctrine](https://github.com/baraja-core/doctrine): `composer require baraja-core/doctrine`.

Ak vybraný balík nie je možné nainštalovať, môžete sa opýtať na konkrétne dôvody a program Composer vypíše zoznam závislostí, ktoré tomu bránia. Často stačí opraviť závislosť od konkrétnej verzie alebo odstrániť nekompatibilný kód. Podrobné informácie získate pomocou príkazu: `composer why baraja-core/doctrine`.

Aktualizácia projektov a balíkov
-----------------------------

Dobre navrhnutý projekt je vyvinutý tak, aby ste mohli časom ľahko sťahovať aktualizácie a vždy mali k dispozícii najnovšie verzie všetkých balíkov. Hlavnou výhodou je, že získate všetky chyby, často aj vylepšenia výkonu a veľa nových funkcií. Okrem toho postupný prechod na nový systém po dlhšom čase menej komplikuje aktualizáciu, pretože problémy budete odstraňovať za chodu v menšom rozsahu a vyhnete sa nekompatibilite.

Ak chcete aktualizovať všetky balíky a závislosti, použite príkaz `composer update`.

Samotný proces aktualizácie môže v niektorých prípadoch zlyhať. Dôvodom je zvyčajne nefunkčná závislosť alebo nekompatibilný balík.

Podrobné informácie o tom, prečo balík nemožno nainštalovať, získate pomocou príkazu: `composer why-not baraja-core/doctrine`. Ak už balík máme a nefunguje nám len konkrétna verzia (nechce sa nainštalovať), môžeme si tiež vyžiadať konkrétnu verziu: `composer why-not baraja-core/doctrine:v1.0.20`.

V súbore `composer.json` môžeme tiež uviesť závislosť od konkrétneho runtime. To je užitočné najmä vtedy, keď vyvíjame projekt s viacerými ľuďmi a chceme overiť, či majú nainštalované všetky rozšírenia.

Zvyčajne sa kontroluje verzia PHP (musí byť `7.1.0` alebo novšia):

{
   "require": {
      "php": ">=7.1.0"
   }
}

Prípadne ďalšie rozšírenia systému:

{
   "require": {
      "php": ">=7.1.0",
      "ext-json": "*",
      "ext-session": "*",
      "ext-PDO": "*"
   }
}

Tieto pravidlá sa potom zohľadňujú pri inštalácii balíkov alebo aktualizácií. Pomáha to predchádzať problémom, ktoré by sa prejavili počas behu. Napríklad balík platobnej brány potrebuje komunikovať s API, takže musí pripustiť závislosti na rozšíreniach `curl` a `json`.

Riešenie problémov so závislosťami
-----------------------------

K porušeniu závislostí často dochádza kvôli zlej verzii PHP. V takom prípade Composer vyhodí napríklad správu `vaša verzia PHP (7.3.11) nadradená verzii "config.platform.php" (7.1) nespĺňa túto požiadavku.`.

Veľmi často je chyba spôsobená nastaveniami priamo v projekte `composer.json`, kde sa nachádza nasledujúca časť:

"config": {
   "platform": {
      "php": "7.2"
   }
}

Zmena **musí byť vykonaná priamo v súbore**. V prípade globálneho projektu (pred inštaláciou alebo s globálnou závislosťou) môžete vynútiť verziu Composeru pomocou príkazu `composer config -g platform.php 7.2.14` (prepínač `-g` znamená `global`).

V niektorých prípadoch chceme nainštalovať najnovšie verzie balíkov a ignorovať nastavenia miestneho prostredia. V tomto prípade môžeme použiť rozšírený príkaz:

$ composer update --ignore-platform-reqs

**Prepínač `ignore-platform-reqs` používajte na vlastné riziko, môže spôsobiť poškodenie projektu!** Nepoužívajte ho, ak nerozumiete dôsledkom.

Ručné volania programu Composer, parametre a správa pamäte
------------------------------------------------------

Composer je vlastne skript PHP zabalený do takzvaného PHAR, čo je skompilovaná verzia aplikácie PHP. Znalosť týchto informácií sa dá dobre využiť, napríklad na lepšiu parametrizáciu samotného volania.

Pri skutočne veľkých projektoch sa niekedy stáva, že nám dôjde pamäť RAM a musíme jej prideliť oveľa viac alebo zvýšiť čas spracovania skriptov.

V systéme Windows môžete napríklad použiť tento príkaz:

$ php -d memory_limit=-1 C:/xampp/htdocs/composer.phar update

Prepínač `memory_limit=-1` povie programu Composer, aby nebol citlivý na limit pamäte RAM a spotreboval toľko pamäte, koľko potrebuje.

Vlastné používateľské skripty po akcii Composer
--------------------------------------------

Po spustení akcií programu Composer môžete vyvolať automatické vykonávanie používateľom definovaných skriptov, ktoré buď vykonávajú konkrétne akcie v projekte, alebo napríklad generujú konfiguráciu po nasadení. Tento prístup používam hlavne na lokálnom serveri, aby som ponúkol nástroj na automatickú konfiguráciu databázy, vygeneroval databázovú schému atď.

Požadované skripty zaregistrujeme v časti `scripts` súboru `composer.json`:

"scripts": {
   "post-autoload-dump": "Baraja\\PackageManager\\PackageRegistrator::composerPostAutoloadDump"
}

V tomto prípade sa automaticky zavolá statická metóda `composerPostAutoloadDump` v triede `Baraja\PackageManager\PackageRegistrator`. Composer má túto triedu k dispozícii, pretože ako prvý vykonal generovanie tried <a href="/autoloading-trid">autoloader</a>.

Ak chceme len spustiť skripty a nevykonávať zbytočné činnosti s programom Composer, je veľmi užitočný príkaz `composer dump`, ktorý len vygeneruje nový autoloader (ktorý odporúčam vždy aktualizovať) a potom okamžite spustí skripty. Ak chcete skúsiť používať skripty, pripravil som hotový balík [Baraja PackageManager](https://github.com/baraja-core/package-manager), ktorý implementuje inteligentné skripty a interaktívne rozhranie pre framework Nette.

Verzovanie dodávateľa do systému Git?
------------------------

Otázka, ktorú často diskutujeme s vývojármi, je, či verzovať obsah priečinka `vendor` do systému Git, alebo ho pri každej inštalácii vytvárať nanovo.

Vo všeobecnosti sa zdá, že čistejším riešením je vôbec neoverovať obsah a zakaždým všetko nainštalovať. V skutočnosti však vývojár z času na čas prestane vyvíjať balík alebo ho úplne odstráni. Neustále sťahovanie balíkov tiež komplikuje miestne inštalácie a aktualizácie, spomaľuje zavádzanie a niekedy môže spôsobiť krátky výpadok webu, keď sa nové verzie balíkov stiahnu nesprávne.

Pozastavenie činnosti predajcu považujem za formu "bezpečnosti". Ak sú súbory fyzicky v systéme verzií, máme na produkčnom serveri aspoň základnú istotu, že balíky budú fungovať a ich kód je presne taký, ako je v lokálne spustenej inštalácii. Okrem toho Vendor často zaberá len jednotky MB, čo pri dnešných kapacitách diskov určite stojí za zabezpečenie fungujúceho webu.

**Praktická poznámka:** Na priemernej stránke, ktorú spravujem, nezaberá `predajca` viac ako `30 MB`, čo je pre Git prijateľná kapacita. Pri klonovaní úložiska s juniorom sa nemusíme zaoberať jeho zaškolením, ako spustiť web a hneď to funguje.

Vlastné balíky Composer
-----------------------

V aplikácii Composer môžete vytvárať vlastné balíky, a to buď verejne dostupné (registrované na adrese [Packagist](https://packagist.org/)), alebo súkromné (musíte mať vlastný server balíkov, napríklad [Satis](https://getcomposer.org/doc/articles/handling-private-packages.md)).

Problematika vytvárania, údržby, vývoja a verzií balíkov je veľmi zložitá a bude predmetom samostatného článku.

Zatiaľ si môžete prečítať článok [Semantic Versioning](https://semver.org/lang/cs/), ktorý som pre vás preložil.
