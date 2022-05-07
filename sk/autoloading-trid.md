Automatické načítanie tried v PHP
=================================

> id: f6cd5762-261f-4153-b27b-075dd8b5ed13
> slug:
> 	cs: autoloading-trid
> 	sk: automaticke-nacitanie-tried-v-php
> 
> publicationDate: '2020-02-09 10:00:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '441a1d4107bb32e8cfe4dbb926c2decd'

Určite to poznáte, pri programovaní PHP skriptov rozdeľujeme kód do mnohých súborov a aby sme mali k dispozícii všetky časti, načítavame ich pomocou série volaní `include`, `require` alebo najlepšie `require_once`, čo zaručuje načítanie len raz.

V kóde to vyzerá takto:

```php
require_once 'Router.php';
require_once 'Page.php';
require_once 'Paginator.php';
```

Hlavnou nevýhodou tohto prístupu je, že programátor sa musí neustále starať o to, aby bolo všetko vždy načítané. Ak však načíta veľa, zbytočne stráca výkon a na disk sa dostane mnohokrát. Manuálne riešenie má teda len problémy.

Natívne automatické nabíjanie
-------------------

Našťastie v PHP existuje natívna podpora takzvaného **Class Autoloading**, čo je logika v kóde, ktorá načíta súbor triedy len vtedy, keď je to prvýkrát potrebné (zvyčajne pri prvom vytvorení inštancie).

Jednoduchá implementácia by potom mohla vyzerať takto:

```php
spl_autoload_register(function (string $className): void {
    include 'src/' . $className . '.php';
});

$obj  = new MyClass1();
$obj2 = new MyClass2();
```

Pri vytváraní inštancie triedy `MyClass1` funkcia `spl_autoload_register` načíta súbor `MyClass1.php` z adresára `src`. Táto implementácia predpokladá, že každá trieda je v samostatnom súbore nazvanom podľa názvu triedy alebo rozhrania.

Zložitejšie prípady automatického nabíjania
-------------------------------

V reálnej aplikácii môže nastať mnoho nepríjemných situácií, ktoré komplikujú napríklad automatické nabíjanie:

- Trieda alebo rozhranie vôbec neexistuje
- Súbor už bol raz načítaný
- V tom istom súbore je viacero tried alebo rozhraní
- Cesta k triede alebo rozhraniu sa nezhoduje s názvom
- Umiestnenie triedy alebo rozhrania sa v priebehu času mení

Na to všetko však nemusíme programovať vlastné riešenie, ale môžeme použiť už raz navrhnuté riešenie.

Ak používate <a href="https://getcomposer.org/doc/01-basic-usage.md">Composer</a>, pravdepodobne používate aj jeho natívne automatické ukladanie. Pri inštalácii akéhokoľvek balíka totiž Composer automaticky vygeneruje mapu tried, ktorá predstavuje prehľad tried a ich fyzické umiestnenie.

Potom na začiatku kódu (zvyčajne v `dex.php`) stačí použiť:

```php
require __DIR__ . '/vendor/autoload.php';
```

Automatické načítanie sa však generuje len raz pri volaní príkazu `composer dump`, takže je potrebné automatické načítanie regenerovať pri každej zmene aplikácie.

RobotLoader - elegantné riešenie pre vývoj
----------------------------------------

Na lokálny vývoj sa mi veľmi páči balík `nette/robot-loader`, ktorý automaticky prechádza adresárovú štruktúru a ukladá triedy do vyrovnávacej pamäte. Ak teda načítame triedu, najprv sa pozrie do vyrovnávacej pamäte a až keď neexistuje, automaticky ju preindexuje. Programátor preto vôbec nemusí sledovať, kde sa ktorý súbor alebo trieda nachádza, a jednoducho programuje.

Inštalácia prostredníctvom aplikácie Composer:

composer require nette/robot-loader

Základné vysvetlenie funkcií je opísané v samotnej <a href="https://doc.nette.org/cs/3.0/robotloader">dokumentácii</a>:

> Podobne ako robot spoločnosti Google prehľadáva a indexuje webové stránky, RobotLoader prehľadáva všetky skripty PHP a zaznamenáva, ktoré triedy, rozhrania a vlastnosti v nich našiel. Výsledky svojho výskumu potom uloží do vyrovnávacej pamäte a použije ich pri ďalšej požiadavke. Stačí teda určiť, ktoré adresáre sa majú prehľadávať a kde sa má ukladať do vyrovnávacej pamäte.

Jeho používanie je potom veľmi jednoduché:

```php
$loader = new Nette\Loaders\RobotLoader;

// pridať adresáre, ktoré by mal RobotLoader indexovať
$loader->addDirectory(__DIR__ . '/app');
$loader->addDirectory(__DIR__ . '/libs');

// nastavenie ukladania do vyrovnávacej pamäte na disk v adresári 'temp'
$loader->setTempDirectory(__DIR__ . '/temp');
$loader->register(); // spustenie RobotLoader
```

Nastavenie `$loader->setAutoRefresh(true alebo false)` určuje, či má RobotLoader znovu indexovať súbory, keď narazí na novú triedu. Táto funkcia by mala byť na produkčných serveroch vypnutá.

Teraz už nikdy nebudete musieť riešiť automatické nabíjanie.

Kombinované riešenie
------------------

Pri vývoji skutočného projektu používam kombinované riešenie.

V praxi to funguje tak, že nainštalované balíky načítam cez Composer autoload (čo je veľmi efektívne) a tým sa vyrieši načítanie všetkých tried v adresári `vendor`.

Kód pre konkrétny projekt je potom umiestnený v adresári `app`, kde sa starám o automatické načítanie len niekoľkých tried pomocou RobotLoader. Dôležité je, aby konkrétna aplikácia bola vždy čo najmenšia a aby sa v čo najväčšej miere používali hotové balíky, čo veľmi podporuje opätovné použitie.

Štruktúra projektu vyzerá takto:

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

Testovanie automatického nabíjania
------------------------

Niekedy sa môže stať, že nie každý súbor sa vždy načíta a vyskytnú sa problémy.

Na ladenie odporúčam funkciu <a href="/get-list-of-all-loaded-files">get_included_files()</a>.
