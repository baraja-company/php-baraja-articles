Kommandoer, nøgleord og funktioner i PHP
========================================

> id: '96a00928-4410-479d-ada0-612de21cdacd'
> slug:
> 	cs: prikazy-a-funkce
> 	da: kommandoer-nogleord-og-funktioner-i-php
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '4534cc03d2ae66454b9f398139f232a4'

Ethvert PHP-script består af kommandoer og funktioner, som tilsammen kaldes **konstruktioner**. Når scriptet behandles, spores disse sprogudtryk (denne proces kaldes **tokenisering**), og på baggrund af *nøgleordene* bestemmer fortolkeren, hvordan processoren skal opføre sig.

Kommandoer
--------------------------

Kommandoer (<a href="https://www.php.net/manual/en/reserved.keywords.php">De kaldes **keywords** på engelsk</a>) er allerede forprogrammerede dele af sproget, de er reserveret specifikt til deres formål, kan bruges med det samme i enhver situation (de kræver ikke yderligere specielle biblioteker eller installation) og udgør grundlaget for alle scripts.

F.eks. <a href="/echo">udtrække en streng til HTML-kode</a>:

```php
echo 'Echo er en sprogkommando til at opregne indholdet';
```

Med kommandoer er det vigtigt at bemærke, at de ikke returnerer nogen værdi og derfor ikke kan bruges som en variabel. Kommandoer kan også identificeres ved, at de ikke indeholder parenteser.

Eksempler:

```php
echo 'Hej, verden!';
echo ('Hej, verden!');
```

Hvis jeg omslutter kommandoen i parenteser, vil opførslen ikke ændre sig, og den vil blive betragtet som et **udtryk**. Det er det samme, som hvis vi skrev i matematik:

```php
5 + 3

// eller:

(5 + 3)
```

Teknisk set er der en forskel, men i praksis ændres der ikke noget.

Funktioner
--------------------------

Hvis der kun var kommandoer, ville det være ret kedeligt. Funktioner er en samling af flere kommandoer.

Vi ønsker ofte at udføre den samme kode flere steder gentagne gange. I så fald ville konstant kopiering være for meget arbejde og kunne føre til fejl, så vi pakker sådan kode ind i en funktion, som vi navngiver, og kalder den derefter bare ved navn.

Når vi kalder funktionen, sender vi den parametrene som variabler, den udfører koden og returnerer resultatet, som vi kan bruge videre.

```php
$text = 'PHP er mit yndlingssprog!';

echo 'Tekst "' . $text . '" er en lang' . strlen($text) . 'tegn.';
```

PHP har mange funktioner direkte foruddefineret (<a href="/documentation">Se dokumentationen for en komplet liste</a>), men det er ofte praktisk at definere sine egne:

```php
function mojeFunkce(int $x, int $y): int
{
   $z = $x + $y;   // tilføjer inputparametrene og gemmer dem i en variabel

   return $z;      // returnerer variablen $z
}

echo mojeFunkce(5, 3); // udskriver tallet 8, fordi tallene blev behandlet af funktionen
```

Variabler i funktioner
--------------------------

Lokale variabler bruges inden for funktioner, hvilket betyder, at de kun kan bruges inden for funktionen og ikke kan manipuleres (eller defineres) uden for funktionen. De får deres startværdier fra funktionsparametrene direkte i funktionsdefinitionen.

Eksempel:

```php
$z = 5;

function prvniFunkce(int $x, int $y): int
{
   return $x - $y; // dette ville give forskellen i antal
}

function druhaFunkce(): mixed
{
   return $z; // dette returnerer en fejl, fordi variablen $z
              // ikke defineret i funktionen
}
```

Nogle gange er det nyttigt at angive nogle af parametrene som valgfrie, hvilket gøres ved at definere en alternativ (standard) værdi:

```php
echo prictiCislo(5);    // returnerer 6
echo prictiCislo(5, 7); // returnerer 12

function prictiCislo(int $x, int $y = 1): int
{
   return $x + $y;
}
```

Funktionen `prictiCislo()` tilføjer værdien fra variablen `$y` til variablen `$x`. Hvis variablen `$y` ikke er defineret (angivet som en parameter, når funktionen kaldes), anvendes dens standardværdi, der er skrevet i funktionsdefinitionen. Den anden funktionsparameter (variablen `$y`) er derfor valgfri.

> Hver funktion kan kun have ét output (én returnering), hvis du angiver flere output i en funktion, returneres det først angivne output. Hvis du ønsker at returnere flere værdier, skal du bruge et array.
>
> I PHP (i modsætning til andre sprog) behøver vi slet ikke at angive et return, og i så fald returnerer funktionen intet (void) på ethvert input. Sådanne funktioner bruges oftest til at gemme data eller til at output til kildekode. Generelt anbefales det dog altid at returnere en værdi, i det mindste en bekræftelse på, at alt gik godt.
