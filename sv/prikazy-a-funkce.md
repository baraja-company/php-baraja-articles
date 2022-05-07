Kommandon, nyckelord och funktioner i PHP
=========================================

> id: '96a00928-4410-479d-ada0-612de21cdacd'
> slug:
> 	cs: prikazy-a-funkce
> 	sv: kommandon-nyckelord-och-funktioner-i-php
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '4534cc03d2ae66454b9f398139f232a4'

Varje PHP-skript består av kommandon och funktioner som tillsammans kallas **konstruktioner**. När skriptet bearbetas spåras dessa språkuttryck (denna process kallas **tokenisering**) och utifrån *nyckelorden* bestämmer tolkaren hur processorn ska bete sig.

Kommandon
--------------------------

Kommandon (<a href="https://www.php.net/manual/en/reserved.keywords.php">De kallas **keywords** på engelska</a>) är redan förprogrammerade delar av språket, de är reserverade specifikt för sitt syfte, kan användas omedelbart i alla situationer (de kräver inga ytterligare specialbibliotek eller installation) och utgör grunden för alla skript.

Till exempel <a href="/echo">extraherar en sträng till HTML-kod</a>:

```php
echo 'Echo är ett språkkommando för att lista innehållet';
```

När det gäller kommandon är det viktigt att notera att de inte returnerar något värde och därför inte kan användas som en variabel. Kommandon kan också identifieras genom att de inte innehåller parenteser.

Exempel:

```php
echo 'Hej, världen!';
echo ('Hej, världen!');
```

Om jag sätter kommandots innehåll inom parentes kommer beteendet inte att ändras och det kommer att betraktas som ett **uttryck**. Det är samma sak som om vi skrev i matematik:

```php
5 + 3

// eller:

(5 + 3)
```

Tekniskt sett finns det en skillnad, men i praktiken förändras ingenting.

Funktioner
--------------------------

Om det bara fanns kommandon skulle det vara ganska tråkigt. Funktioner är en samling av flera kommandon.

Vi vill ofta köra samma kod på flera ställen upprepade gånger. I det fallet skulle konstant kopiering vara för mycket arbete och skulle kunna leda till fel, så vi lindar in sådan kod i en funktion som vi namnger och sedan bara kallar den vid namn.

När vi anropar funktionen skickar vi parametrarna till den som variabler, den utför koden och returnerar resultatet, som vi kan använda vidare.

```php
$text = 'PHP är mitt favoritspråk!';

echo 'Text "' . $text . '" är en lång' . strlen($text) . 'karaktärer.';
```

PHP har många fördefinierade funktioner (<a href="/documentation">se dokumentationen för en fullständig lista</a>), men det är ofta praktiskt att definiera egna funktioner:

```php
function mojeFunkce(int $x, int $y): int
{
   $z = $x + $y;   // lägger till inparametrarna och lagrar dem i en variabel

   return $z;      // returnerar variabeln $z
}

echo mojeFunkce(5, 3); // skriver ut siffran 8, eftersom siffrorna har behandlats av funktionen
```

Variabler i funktioner
--------------------------

Lokala variabler används inom funktioner, vilket innebär att de endast kan användas inom funktionen och inte kan manipuleras (eller definieras) utanför funktionen. De får sina startvärden från funktionsparametrarna direkt i funktionsdefinitionen.

Exempel:

```php
$z = 5;

function prvniFunkce(int $x, int $y): int
{
   return $x - $y; // detta skulle ge skillnaden i antal siffror
}

function druhaFunkce(): mixed
{
   return $z; // Detta ger ett fel eftersom variabeln $z
              // inte definierad inom funktionen
}
```

Ibland är det lämpligt att ställa in vissa parametrar som valfria, vilket görs genom att definiera ett alternativt värde (standardvärde):

```php
echo prictiCislo(5);    // ger 6
echo prictiCislo(5, 7); // ger 12

function prictiCislo(int $x, int $y = 1): int
{
   return $x + $y;
}
```

Funktionen `prictiCislo()` lägger till värdet från variabeln `$y` till variabeln `$x`. Om variabeln `$y` inte är definierad (anges som en parameter när funktionen anropas) används dess standardvärde som skrivs i funktionsdefinitionen. Den andra funktionsparametern (variabeln `$y`) är därför valfri.

> Varje funktion kan bara ha en utgång (en retur), om du anger flera utgångar i en funktion returneras den som anges först. Om du vill returnera flera värden måste du använda en array.
>
> I PHP (till skillnad från andra språk) behöver du inte ange någon return alls, i så fall returnerar funktionen ingenting (void) på alla indata. Sådana funktioner används oftast för att lagra data eller för att skriva ut källkod. I allmänhet rekommenderas dock att alltid returnera något värde, åtminstone en bekräftelse på att allt gick bra.
