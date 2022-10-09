Autoloading af klasser i PHP
============================

> id: f6cd5762-261f-4153-b27b-075dd8b5ed13
> slug:
> 	cs: autoloading-trid
> 	da: autoloading-af-klasser-i-php
> 
> publicationDate: '2020-02-09 10:00:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '441a1d4107bb32e8cfe4dbb926c2decd'

Jeg er sikker på, at du kender det: Når vi programmerer PHP-scripts, deler vi koden op i mange filer, og for at have alle dele til rådighed indlæser vi dem med en række `include`, `require` eller helst `require_once`-opkald, hvilket garanterer, at koden kun indlæses én gang.

I koden ser det således ud:

```php
require_once 'Router.php';
require_once 'Side.php';
require_once 'Paginator.php';
```

Den største ulempe ved denne fremgangsmåde er, at programmøren hele tiden skal sørge for, at alt altid er indlæst. Men hvis han indlæser meget, mister han unødvendigt ydelse og når disken mange gange. Så den manuelle løsning har kun problemer.

Indfødt autoloading
-------------------

Heldigvis er der indbygget understøttelse i PHP for såkaldt **Class Autoloading**, som er logik i koden, der kun indlæser en klassefil, når der først er brug for den (typisk når en instans først oprettes).

En simpel implementering kan se således ud:

```php
spl_autoload_register(function (string $className): void {
    include 'src/' . $className . '.php';
});

$obj  = new MyClass1();
$obj2 = new MyClass2();
```

Når der oprettes en instans af klassen `MyClass1`, læser funktionen `spl_autoload_register` filen `MyClass1.php` fra mappen `src`. Denne implementering forudsætter, at hver klasse er i en separat fil, der kaldes med klasse- eller grænsefladenavnet.

Mere komplekse tilfælde af selvladning
-------------------------------

I en rigtig applikation kan der opstå mange ubehagelige situationer, som f.eks. kan komplicere autoload:

- Klassen eller grænsefladen findes slet ikke
- Filen er allerede blevet indlæst én gang
- Der er flere klasser eller grænseflader i den samme fil
- Stien til klassen eller grænsefladen passer ikke til navnet
- Placeringen af klassen eller grænsefladen ændres over tid

Vi behøver dog ikke at programmere vores egen løsning til alt dette, men kan bruge den eksisterende løsning, når den først er designet.

Hvis du bruger <a href="https://getcomposer.org/doc/01-basic-usage.md">Composer</a>, bruger du sandsynligvis også dens indbyggede autoloading. Det skyldes, at når du installerer en pakke, genererer Composer automatisk et `class map`, som er en oversigt over klasserne og deres fysiske placering.

I begyndelsen af koden (typisk i `index.php`) skal du så bare bruge:

```php
require __DIR__ . '/vendor/autoload.php';
```

Autoloading genereres dog kun én gang, når kommandoen `composer dump` kaldes, så det er nødvendigt at generere autoloading på ny, hver gang programmet ændres.

RobotLoader - en elegant løsning til udvikling
----------------------------------------

Til lokal udvikling kan jeg virkelig godt lide pakken `nette/robot-loader`, som automatisk gennemgår mappestrukturen og gemmer klassernes placering i cache. Så hvis vi indlæser en klasse, kigger den først i cachen, og kun hvis den ikke findes, indekseres projektet automatisk igen. Derfor behøver programmøren ikke at holde styr på, hvor en fil eller klasse er placeret, men kan bare programmere.

Installation via Composer:

```txt
composer require nette/robot-loader
```

Den grundlæggende forklaring af funktionaliteten er beskrevet i <a href="https://doc.nette.org/cs/3.0/robotloader">dokumentationen</a> selv:

> På samme måde som Googles robot gennemtrawler og indekserer websider, gennemtrawler RobotLoader alle PHP-scripts og noterer, hvilke klasser, grænseflader og egenskaber den har fundet i dem. Derefter lagrer den resultaterne af sine undersøgelser i en cache og bruger dem i den næste anmodning. Så du skal blot angive, hvilke mapper der skal gennemses, og hvor der skal gemmes cache.

Den er derefter meget nem at bruge:

```php
$loader = new Nette\Loaders\RobotLoader;

// tilføj mapper, som RobotLoader skal indeksere
$loader->addDirectory(__DIR__ . '/app');
$loader->addDirectory(__DIR__ . '/libs');

// indstille caching til disk i mappen "temp
$loader->setTempDirectory(__DIR__ . '/temp');
$loader->register(); // Start RobotLoader
```

Indstilling af `$loader->setAutoRefresh(true eller false)` bestemmer, om RobotLoader skal indeksere filer igen, når den støder på en ny klasse. Dette bør være deaktiveret på produktionsservere.

Så behøver du aldrig mere at bekymre dig om autoloading.

Kombineret løsning
------------------

Jeg bruger en kombineret løsning, når jeg udvikler et rigtigt projekt.

I virkeligheden fungerer det sådan, at jeg indlæser de installerede pakker via Composer autoload (hvilket er meget effektivt), og det løser indlæsningen af alle klasser i `vendor`-mappen.

Koden til det specifikke projekt placeres derefter i mappen `app`, hvor jeg håndterer autoloading af blot nogle få klasser via RobotLoader. Det vigtige er altid at holde den konkrete applikation så lille som muligt og bruge færdige pakker så meget som muligt, da det fremmer genanvendeligheden meget.

Projektets struktur ser således ud:

```txt
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

Prøvning af autoloading
------------------------

Nogle gange kan det ske, at ikke alle filer altid indlæses, og du vil finde problemer.

Til fejlfinding anbefaler jeg <a href="/get-list-of-all-loaded-files">get_included_files()</a>-funktionen.
