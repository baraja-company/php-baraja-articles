Autoloading tříd v PHP
================================

> id: f6cd5762-261f-4153-b27b-075dd8b5ed13
> slugCS: autoloading-trid
> publicationDate: "2020-02-09 10:00:29"
> mainCategoryId: "3e45c55a-a4cd-4745-b1bb-0332702fefbf"

Určitě to znáte, při programování PHP scriptů si rozdělíme kód na mnoho souborů a abychom měli k dispozici všechny části, tak je načteme sérií volání `include`, `require` nebo nejlépe `require_once`, což zaručí načtení právě jednou.

V kódu to pak vypadá třeba takto:

```php
require_once 'Router.php';
require_once 'Page.php';
require_once 'Paginator.php';
```

Hlavní nevýhoda tohoto přístupu je v tom, že programátor musí neustále hlídat, aby měl vždy všechno načteno. Pokud toho ale načte hodně, tak přichází zbytečně o výkon a mnohokrát se sahá na disk. Ruční řešení má tedy jen samé problémy.

Nativní autoloading
-------------------

Naštěstí v PHP existuje nativní podpora pro tzv. **Autoloading tříd**, což je logika v kódu, která načte soubor s třídou až v okamžiku, kdy je poprvé potřeba (typicky při prvním vytváření instance).

Jednoduchá implementace pak může vypadat třeba takto:

```php
spl_autoload_register(function (string $className): void {
    include 'src/' . $className . '.php';
});

$obj  = new MyClass1();
$obj2 = new MyClass2();
```

Funkce `spl_autoload_register` při vytváření instance třídy `MyClass1` načte soubor `MyClass1.php` z adresáře `src`. Tato implementace předpokládá, že je každá třída v samostatném souboru, který se jmenuje názvem třídy nebo rozhraní.

Složitější případy autoloadingu
-------------------------------

V reálné aplikaci může nastat mnoho nepříjemných situací, které komplikují autoload, a to například:

- Třída nebo rozhraní vůbec neexistuje
- Soubor už byl jednou načten
- V jenom souboru je více tříd nebo rozhraní
- Cesta k třídě nebo rozhraní neodpovídá názvu
- Umístění třídy nebo rozhraní se v čase změní

Pro toto všechno však nemusíme programovat vlastní řešení, ale můžeme použít již jednou navržené a existující.

Pokud používáte <a href="https://getcomposer.org/doc/01-basic-usage.md">Composer</a>, tak pravděpodobně používáte i jeho nativní autoloading. Při instalaci jakéhokoli balíku totiž Composer automaticky generuje tzv. `class map`, což je přehled tříd a jejich fyzické umístění.

Na začátku kódu (typicky v `index.php`) pak stačí použít:

```php
require __DIR__ . '/vendor/autoload.php';
```

Autoloading se však vygeneruje jen jednou při zavolání příkazu `composer dump`, proto je potřeba při každé změně aplikace autoloding přegenerovat.

RobotLoader - elegantní řešení pro vývoj
----------------------------------------

Pro lokální vývoj se mi velmi líbí balík `nette/robot-loader`, který automaticky prochází adresářovou strukturu a umístění tříd si ukládá do cache. Pokud tedy načítáme třídu, nejprve se podívá do cache a až pokud neexistuje, tak provede automatické přeindexování projektu. Programátor proto nemusí vůbec hlídat, kde je jaký soubor nebo třída umístěna a prostě programuje.

Instalace přes Composer:

```
composer require nette/robot-loader
```

Základní vysvětlení funkčnosti popisuje sama <a href="https://doc.nette.org/cs/3.0/robotloader">dokumentace</a>:

> Podobně, jako Google robot prochází a indexuje webové stránky, tak i RobotLoader prochází všechny PHP skripty a zaznamenává si, které třídy, rozhraní a traity v nich našel. Výsledky bádání si poté uloží do cache a použije při dalším požadavku. Stačí tedy určit, které adresáře má procházet a kam ukládat cache.

Použití je pak extrémně snadné:

```php
$loader = new Nette\Loaders\RobotLoader;

// přidáme adresáře, které má RobotLoader indexovat
$loader->addDirectory(__DIR__ . '/app');
$loader->addDirectory(__DIR__ . '/libs');

// nastavíme cachování na disk do adresáře 'temp'
$loader->setTempDirectory(__DIR__ . '/temp');
$loader->register(); // spustíme RobotLoader
```

Nastavením `$loader->setAutoRefresh(true nebo false)` určíme, zda má RobotLoader reindexovat soubory, když narazí na novou třídu. To by mělo být vypnuto na produkčních serverech.

Tak, a od teď už nemusíte autoloading nikdy řešit.

Kombinované řešení
------------------

Při vývoji reálného projektu používám kombinované řešení.

Reálně to funguje tak, že nainstalované balíky načítám přes Composer autoload (který je velmi efektivní) a tím se vyřeší loading všech tříd v adresáři `vendor`.

Kód konkrétního projektu je pak umístěn v adresáři `app`, kde řeším autoloading jen několika málo tříd přes RobotLoader. Důležité je, aby byla konkrétní aplikace vždy co nejmenší a maximálně se používaly hotové balíky, hodně to podporuje znovupoužitelnost.

Struktura projektu pak vypadá třeba takto:

```
/app
    Bootstrap.php <-- konfigurace
    /model
        UserForm.php <-- projektové třídy
        RegisterFactory.php
        ...
/vendor
    ... <-- knihovny
/www
    index.php <-- inicializace
```

Testování autoloadingu
------------------------

Někdy se může stát, že se ne vždy každý soubor načte a budete zjišťovat potíže.

Pro debugování doporučuji funkci <a href="/ziskani-seznamu-vsech-nactenych-souboru">get_included_files()</a>.
