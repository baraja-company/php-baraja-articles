Autoloading av klasser i PHP
============================

> id: f6cd5762-261f-4153-b27b-075dd8b5ed13
> slug:
> 	cs: autoloading-trid
> 	sv: autoloading-av-klasser-i-php
> 
> publicationDate: '2020-02-09 10:00:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '441a1d4107bb32e8cfe4dbb926c2decd'

Jag är säker på att du känner till det här, när vi programmerar PHP-skript delar vi upp koden i många filer och för att ha alla delar tillgängliga laddar vi dem med en serie anrop `include`, `require` eller helst `require_once`, vilket garanterar att koden laddas bara en gång.

I koden ser det ut så här:

```php
require_once 'Router.php';
require_once 'Sida.php';
require_once 'Paginator.php';
```

Den största nackdelen med detta tillvägagångssätt är att programmeraren hela tiden måste se till att allt alltid är laddat. Men om han laddar mycket förlorar han prestanda i onödan och når disken många gånger. Den manuella lösningen har alltså bara problem.

Autoladdning av den ursprungliga versionen
-------------------

Lyckligtvis finns det stöd i PHP för så kallad **Class Autoloading**, vilket är logik i koden som laddar en klassfil endast när den behövs för första gången (vanligtvis när en instans skapas för första gången).

Ett enkelt genomförande kan då se ut så här:

```php
spl_autoload_register(function (string $className): void {
    include 'src/' . $className . '.php';
});

$obj  = new MyClass1();
$obj2 = new MyClass2();
```

När en instans av klassen `MyClass1` skapas läser funktionen `spl_autoload_register` filen `MyClass1.php` från katalogen `src`. Den här implementationen förutsätter att varje klass finns i en separat fil som kallas med klassens eller gränssnittets namn.

Mer komplexa fall av självladdning
-------------------------------

I en verklig tillämpning kan många obehagliga situationer uppstå som till exempel försvårar autoload:

- Klassen eller gränssnittet finns inte alls.
- Filen har redan laddats en gång
- Det finns flera klasser eller gränssnitt i samma fil.
- Sökvägen till klassen eller gränssnittet matchar inte namnet.
- Klassens eller gränssnittets plats ändras med tiden.

Vi behöver dock inte programmera en egen lösning för allt detta, utan kan använda den befintliga lösningen när den väl är utformad.

Om du använder <a href="https://getcomposer.org/doc/01-basic-usage.md">Composer</a> använder du förmodligen också dess naturliga autoloading. Detta beror på att när du installerar ett paket genererar Composer automatiskt en "class map", som är en översikt över klasserna och deras fysiska placering.

I början av koden (vanligtvis i `index.php`) använder du sedan bara:

```php
require __DIR__ . '/vendor/autoload.php';
```

Autoloading genereras dock bara en gång när kommandot `composer dump` anropas, så det är nödvändigt att generera autoloading på nytt varje gång programmet ändras.

RobotLoader - en elegant lösning för utveckling
----------------------------------------

För lokal utveckling gillar jag verkligen paketet `nette/robot-loader`, som automatiskt går igenom katalogstrukturen och lagrar klasserna i cacheminnet. Så om vi laddar en klass tittar den först i cacheminnet och om den inte finns, indexeras projektet automatiskt på nytt. Därför behöver programmeraren inte hålla reda på var en fil eller klass finns utan kan bara programmera.

Installation via Composer:

```txt
composer require nette/robot-loader
```

Den grundläggande förklaringen av funktionerna beskrivs i <a href="https://doc.nette.org/cs/3.0/robotloader">dokumentationen</a>:

> På samma sätt som Googles robot kryssar och indexerar webbsidor kryssar RobotLoader alla PHP-skript och noterar vilka klasser, gränssnitt och egenskaper som finns i dem. Den lagrar sedan resultaten av sin forskning och använder dem i nästa begäran. Du behöver alltså bara ange vilka kataloger du ska söka i och var du ska lägga cacheminnet.

Den är sedan mycket lätt att använda:

```php
$loader = new Nette\Loaders\RobotLoader;

// lägga till kataloger som RobotLoader ska indexera
$loader->addDirectory(__DIR__ . '/app');
$loader->addDirectory(__DIR__ . '/libs');

// Ställ in cachelagringen på disk i katalogen "temp".
$loader->setTempDirectory(__DIR__ . '/temp');
$loader->register(); // starta RobotLoader
```

Genom att ställa in `$loader->setAutoRefresh(true eller false)` avgörs om RobotLoader ska indexera om filerna när den möter en ny klass. Detta bör inaktiveras på produktionsservrar.

Så, nu behöver du aldrig mer ha att göra med autoloading igen.

Kombinerad lösning
------------------

Jag använder en kombinerad lösning när jag utvecklar ett verkligt projekt.

I verkligheten fungerar det så att jag laddar de installerade paketen via Composer autoload (vilket är mycket effektivt) och detta löser laddningen av alla klasser i `vendor`-katalogen.

Koden för det specifika projektet placeras sedan i katalogen `app`, där jag hanterar autoloading av några få klasser via RobotLoader. Det viktiga är att alltid hålla den konkreta applikationen så liten som möjligt och att använda färdiga paket så mycket som möjligt, det främjar återanvändningen i hög grad.

Projektets struktur ser ut så här:

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

Testning av autoladdning
------------------------

Ibland kan det hända att alla filer inte alltid laddas och att du hittar problem.

För felsökning rekommenderar jag funktionen <a href="/get-list-of-all-loaded-files">get_included_files()</a>.
