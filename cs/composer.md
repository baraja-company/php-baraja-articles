Composer - kompletní přehled funkcí pro pokročilé
=================================================

> id: a74d8d59-91ce-4602-ad52-80cf89a647bd
> slug:
> 	cs: composer
> 
> perex: Composer je pokročilý správce balíků a závislostí pro vaše PHP aplikace. Článek popisuje jeho všechny výhody a možnosti použití.
> publicationDate: "2020-03-10 20:18:19"
> mainCategoryId: "4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154"

Jak už víte, tak [Composer](https://getcomposer.org/) je robustní správce balíků a závislostí pro PHP, přes který můžete elegantně spravovat stovky projektů najednou a distribuovat již jednou napsaný kód do všech aplikací současně.

Tento návod slouží jako podrobná komplexní příručka vývojáře. Ukážeme si všechny důležité pokročilé techniky práce s Composerem, vysvětlíme si i technické detaily a relevantní návaznosti.

Instalace Composeru
-------------------

Bez ohledu na platformu stáhneme z [oficiálních stránek Composeru](https://getcomposer.org/).

Interně se stáhne PHP soubor `composer-setup.php`, který po spuštění v CLI režimu umí Composer nainstalovat. Důležité je také vědět, že Composer bez PHP nefunguje, proto nejprve ověřte, že máte v počítači funkční PHP (stačí, aby bylo k dispozici jen pro Terminál).

Na Macu a Linuxu Composer funguje hned po instalaci a stačí zavolat příkaz `composer -v`, kterým rychle ověříte, že je Composer správně nainstalován.

V Linuxu lze pro instalaci použít příkaz: `/usr/bin/php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');“`

Na Windows je dobré doinstalovat nástroj [Git Bash pro Windows](https://gitforwindows.org/), který umožňuje otevřít specifický Terminál, který se chová téměř jako Linux a umožní vám pracovat ve stejném prostředí, jako na Linuxu.

Instalace pro servery je stejná jako na lokálním prostředí. Jenom si ohlídejte správně nastavenou verzi PHP, kterou Composer interně používá.

Dostupné příkazy
----------------

Composer implementuje celou řadu příkazů.

Použití je: `composer <příkaz>`, například: `composer update`.

Přehled pro verzi `1.10.0`:

| Příkaz                | Popis / význam |
|-----------------------|----------------|
| `about`               | Zobrazí stručné informace o Composeru. |
| `archive`             | Vytvoří archiv s obsahem vybraného Composer balíku. |
| `browse`              | Otevře ve webovém prohlížeči domovskou stránku vybraného balíku, autora nebo jinou související stránku. Často může obsahovat dokumentaci k použití. |
| `cc`                  | Vymaže interní Composer cache s verzemi balíků stažených v minulosti. |
| `check-platform-reqs` | Zkontrolujte, zda jsou splněny požadavky pro instalaci v rámci aktuální platformy. |
| `clear-cache`         | Vymaže interní Composer cache. |
| `clearcache`          | Vymaže interní Composer cache. |
| `config`              | Nastaví konfigurační direktivu. |
| `create-project`      | Vytvoří nový projekt na základě zvoleného balíku a automaticky vytvoří složku, do které bude projekt umístěn. |
| `depends`             | Ukáže, které balíky způsobily instalaci zvoleného balíku. |
| `diagnose`            | Diagnostikuje systém, aby identifikoval běžné chyby. Zpracování výstupů si musí vyřešit vývojář sám, jde pouze o výpis. |
| `dump-autoload`       | Vygeneruje nový <a href="/autoloading-trid">autoloader</a>. |
| `dumpautoload`        | Vygeneruje nový <a href="/autoloading-trid">autoloader</a>. |
| `exec`                | Spustí binárky a scripty z Vendoru. |
| `fund`                | Prozkoumá, jak provést/upravit vaše závislosti. |
| `global`              | Umožní spouštění globálních Composer příkazů z proměnné `$COMPOSER_HOME`. |
| `help`                | Vypíše nápovědu pro příkazy. |
| `home`                | Otevře domovskou stránku konkrétního balíku v prohlížeči. |
| `i`                   | Nainstaluje všechny projektové závislosti podle souboru `composer.lock`, pokud existuje a je validní. Pokud nastane problém, použijí se informace z `composer.json` a `composer.lock` vrátí do původního stavu. |
| `info`                | Zobrazí informace o aktuálně nainstalovaných balících v projektu. Zobrazí názvy všech balíků, jejich aktuální verze a krátký popis. |
| `init`                | Vytvoří základní funkční `composer.json` v aktuálním adresáři. |
| `install`             | Nainstaluje všechny projektové závislosti podle souboru `composer.lock`, pokud existuje a je validní. Pokud nastane problém, použijí se informace z `composer.json` a `composer.lock` vrátí do původního stavu. |
| `licenses`            | Zobrazí seznam všech balíků, jejich verze a aktuální licenci. |
| `list`                | Zobrazí přehled dostupných příkazů. |
| `outdated`            | Zobrazí přehled všech balíků, pro které existuje novější verze dostupná k instalaci a splňující závislosti. U každého balíku bude zobrazena nejnovější kompatibilní verze, kterou Composer navrhuje k instalaci. |
| `prohibits`           | Ukáže, které balíky a závislosti brání v instalaci požadovaného balíku nebo jeho požadované verze. |
| `remove`              | Odebere balík z konfigurační sekce `require` nebo `require-dev`. |
| `require`             | Přidá požadovaný balík do `composer.json` a provede instalaci. Pokud nelze splnit závislosti, vrátí se do původního stavu. |
| `run`                 | Spustí definované scripty v `composer.json`. |
| `run-script`          | Spustí definované scripty v `composer.json`. |
| `search`              | Vyhledá balíky podle klíčových slov nebo hledaného dotazu. |
| `self-update`         | Aktualizuje interní `composer.phar` na nejnovější verzi. |
| `selfupdate`          | Aktualizuje interní `composer.phar` na nejnovější verzi.  |
| `show`                | Zobrazí podrobné informace o aktuálně nainstalovaných balících. |
| `status`              | Zobrazí přehled ručně provedených lokálních změn v balících na základě porovnání se zdrojem balíku, odkud byl původně nainstalován. |
| `suggests`            | Zobrazí návrhy k balíkům. Návrhy mohou zahrnovat různé druhy akcí, například instalace bezpečnostních aktualizací a podobně. |
| `update`              | Aktualizuje celý projekt podle závislostí tak, aby byly vždy všechny splněny podle `composer.json`. Pokud se podaří, aktualizuje `composer.lock`, kam zapíše aktuálně nainstalované verze. |
| `upgrade`             | Alias k `update`. |
| `u`                   | Alias k `update`. |
| `validate`            | Zkontroluje `composer.json` a `composer.lock` na syntaktické chyby. |
| `why`                 | Zobrazí, které balíky způsobily instalaci aktuálně vybraného balíku včetně všech závislostí. |
| `why-not`             | Zobrazí, které balíky a verze brání v instalaci zvoleného balíku nebo verze. |

Vytvoření projektu a jeho definice
----------------------------------

Každý projekt spravovaný Composerem je definován souborem `composer.json` v jeho rootu, který definuje veškeré závislosti. Soubor můžeme vytvořit buď pro existující projekt ručně, nebo při zakládání projektu automaticky.

Protože v Composeru je všechno balík, tak i samotný projekt může být založen na základě balíku. Pokud tedy například vytváříte desítky nebo stovky velmi podobných projektů, dává smysl jejich základní konfiguraci a strukturu umístit do samostatného balíku, na základě kterého je možné provést instalaci.

Příkladem takového balíku je můj [Baraja Sandbox](https://github.com/baraja-core/sandbox), který je postaven na čistém Nette 3.0 a přidává základní závislost na můj [Package Manager](https://github.com/baraja-core/package-manager), který používám pro všechny projekty a správu závislostí v Nette konfiguraci.

Sandbox pak nainstalujeme jednoduše příkazem:

```shell
$ composer create-project baraja/sandbox <your-project-name>
```

Podle názvu projektu poté Composer automaticky vytvoří složku, do které projekt nainstaluje (zkopíruje obsah balíku a nainstaluje závislosti).

Ve složce `vendor` poté Composer spravuje všechny balíky (jsou tam jejich fyzické soubory) a generuje autoload tříd, který vložíme ideálně přímo do `index.php` jako řádek:

```php
<?php

require __DIR__ . '/vendor/autoload.php';

// Samotný kód aplikace
```

Instalace dalších balíků a závislostí
-------------------------------------

V rámci funkčního projektu můžeme velmi jednoduše instalovat nové balíky a přidávat závislosti.

Dělá se to 2 způsoby:

- Příkazem `composer require ...`,
- Přidáním závislosti přímo do souboru `composer.json` do sekce `require` a poté příkaz `composer update`.

Zkuste si nainstalovat například PackageManager: `composer require baraja-core/package-manager`, nebo [Doctrine](https://github.com/baraja-core/doctrine): `composer require baraja-core/doctrine`.

Pokud zvolený balík nelze nainstalovat, lze se zeptat na konkrétní důvody a Composer vypíše seznam závislostí, které tomu brání. Často stačí opravit závislost na konkrétní verzi nebo odebrat nekompatibilní kód. Podrobné informace získáme příkazem: `composer why baraja-core/doctrine`.

Aktualizace projektů a balíků
-----------------------------

Dobře navržený projekt se vyvíjí tak, aby šlo v čase jednoduše stahovat aktualizace a měli jste vždy k dispozici nejnovější verze všech balíků. Hlavní benefit je zejména v tom, že získáte opravu všech chyb, často vylepšení výkonu a spoustu nových funkcí. Navíc díky postupnému přecházení nebude update po dlouhé době tak komplikovaný, protože budete řešit problémy za chodu v menším měřítku a vyhnete se nekompatibilitám.

Všechny balíky a závislosti aktualizujete příkazem: `composer update`.

Samotný update proces může v některých případech selhat. Důvodem obvykle bývá buď porušená závislost nebo nekompatibilní balík.

Podrobné informace, proč balík nelze nainstalovat získáme příkazem: `composer why-not baraja-core/doctrine`. Pokud balík už máme a nefunguje jen jeho konkrétní verze (nechce se nainstalovat), můžeme se zeptat i na konkrétní verzi: `composer why-not baraja-core/doctrine:v1.0.20`.

V rámci souboru `composer.json` můžeme také uvést závislost na konkrétní běhové prostředí. To se hodí zejména v případě, kdy projekt vyvíjíme s více lidmi a chceme ověřit, že mají nainstalovány všechna rozšíření.

Typicky se kontroluje verze PHP (musí být verze `7.1.0` nebo novější):

```json
{
   "require": {
      "php": ">=7.1.0"
   }
}
```

Případně i další systémová rozšíření:

```json
{
   "require": {
      "php": ">=7.1.0",
      "ext-json": "*",
      "ext-session": "*",
      "ext-PDO": "*"
   }
}
```

Při instalaci balíků nebo aktualizaci se poté zohledňují i tato pravidla. Pomáhá to předcházet problémům, které by se projevily až za běhu. Typicky například balík pro platební bránu potřebuje komunikovat s API, proto musí přiznávat závislost na rozšíření `curl` a `json`.

Řešení potíží se závislostmi
-----------------------------

Často dochází k porušení závislostí z důvodu špatné verze PHP. Composer v takovém případě vyhodí hlášku například `your PHP version (7.3.11) overridden by "config.platform.php" version (7.1) does not satisfy that requirement.`.

Velmi často za chybu může nastavení přímo v projektovém `composer.json`, kde je následující sekce:

```json
"config": {
   "platform": {
      "php": "7.2"
   }
}
```

Změnu [musíte provést přímo v souboru](https://stackoverflow.com/a/35538178/9183782). V případě globálního projektu (ještě před instalací nebo při globální závislosti) lze verzi Composeru vnutit příkazem: `composer config -g platform.php 7.2.14` (přepínač `-g` znamená `global`).

V některých případech chceme nainstalovat nejnovější verze balíků a ignorovat nastavení lokálního prostředí. V takovém případě lze použít pokročilý příkaz:

```shell
$ composer update --ignore-platform-reqs
```

**Přepínač `ignore-platform-reqs` používejte na vlastní nebezpečí, může způsobit poškození projektu!** Nepoužívejte, pokud nechápete důsledky.

Ruční volání Composeru, parametry a správa paměti
------------------------------------------------------

Composer je ve skutečnosti PHP script zabalený do tzv. PHARu, což je zkompilovaná verze PHP aplikace. Když tuto informaci víme, tak toho lze dobře využít například pro lepší parametrizaci samotného volání.

Při opravdu velkých projektech se totiž občas stane, že dojde operační paměť a je potřeba přidělit mnohem víc, případně prodloužit čas zpracování scriptů.

Například na Windows pro to lze použít tento příkaz:

```shell
$ php -d memory_limit=-1 C:/xampp/htdocs/composer.phar update
```

Přičemž přepínač `memory_limit=-1` říká, aby nebyl Composer náchylný na limit operační paměti a spotřeboval libovolně kolik potřebuje.

Vlastní uživatelské scripty po Composer akci
--------------------------------------------

Po spuštění Composer akcí lze vyvolat automatické spuštění uživatelsky definovaných scriptů, které buď provedou specifické akce nad projektem, nebo například vygenerují konfiguraci po deployi. Tento přístup používám zejména na lokálním serveru pro nabídnutí automatického nástroje pro konfiguraci databáze, vygenerování databázového schématu a podobně.

Požadované scripty zaregistrujeme do sekce `scripts` v `composer.json`:

```json
"scripts": {
   "post-autoload-dump": "Baraja\\PackageManager\\PackageRegistrator::composerPostAutoloadDump"
}
```

V tomto případě se provede automatické zavolání statické metody `composerPostAutoloadDump` v třídě `Baraja\PackageManager\PackageRegistrator`. Composer má tuto třídu k dispozici, protože nejprve provedl generování <a href="/autoloading-trid">autoloaderu tříd</a>.

Pokud chceme spustit jen scripty a neprovádět zbytečné akce s Composerem, tak velmi dobře poslouží příkaz `composer dump`, který pouze vygeneruje nový autoloader (což doporučuji, aby byl vždy aktuální) a poté ihned spustí scripty. Pokud si chcete použití scriptů vyzkoušet, připravil jsem hotový balík [Baraja PackageManager](https://github.com/baraja-core/package-manager), který chytré scripty a interaktivní rozhraní implementuje pro Nette framework.

Verzovat Vendor do Gitu?
------------------------

S vývojáři často řešíme otázku, jestli verzovat obsah složky `vendor` do Gitu, nebo ji nechat vygenerovat na každé instalaci znovu.

Obecně se jako čistější řešení jeví obsah **vůbec neverzovat** a pokaždé vše nainstalovat. Realita je ale taková, že čas od času vývojář přestane balík vyvíjet, nebo ho úplně odstraní. Neustálé stahování balíků navíc komplikuje lokální instalace a aktualizace a také zpomaluje deploye a občas může za krátké výpadky webu, když se špatně stáhnou nové verze balíků.

Zaverzování Vendoru vnímám jako určitou formu "jistoty". Pokud jsou soubory fyzicky ve verzovacím systému, tak máme na produkčním serveru aspoň elementární jistotu, že budou balíky fungovat a jejich kód je přesně takový, jako je v lokálně běžící instalaci. Navíc Vendor často zabírá jen jednotky MB, což za jistotu funkčního webu vzhledem ke kapacitám dnešních disků určitě stojí.

**Praktická poznámka:** U průměrného webu, který spravuji, zabírá `vendor` maximálně `30 MB`, což je pro Git akceptovatelná kapacita. Při klonování repositáře juniorem pak nemusíme řešit jeho zaškolování, jak web zprovoznit a zkrátka "hned funguje".

Vlastní Composer balíky
-----------------------

V rámci Composeru můžete vytvářet vlastní balíky, a to buď veřejně dostupné (registrují se na [Packagist](https://packagist.org/)), nebo privátní (musíte mít vlastní balíkový server, například [Satis](https://getcomposer.org/doc/articles/handling-private-packages-with-satis.md)).

Problematika zakládání, správy, vývoje a verzování balíků je velmi komplexní a vznikne pro to samostný článek.

V mezičase si můžete přečíst článek [Sémantické verzování](https://semver.org/lang/cs/), který jsem pro vás přeložil.
