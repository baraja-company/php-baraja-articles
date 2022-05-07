Undantag och hur man fångar upp dem i PHP
=========================================

> id: '61b0f178-bb1c-4166-9e8a-af49de2e2a8c'
> slug:
> 	cs: vyjimky
> 	sv: undantag-och-hur-man-fangar-upp-dem-i-php
> 
> publicationDate: '2020-02-16 22:18:18'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: d843fe11a092943db429cb8a28384a31

Undantag är verktyg för objektorienterad programmering, som ger ett elegant sätt att kasta och hantera (behandla) programfel.

Ett undantag kastas först (`thrown`), behandlas (`try`) och fångas (`catch`). Endast kast är obligatoriskt.

Filosofi för generering av undantag
-------------------------

Innan undantagen kom till var felhanteringen i programmeringen mycket komplicerad eftersom vi var tvungna att förlita oss på funktionernas returvärden och fånga upp dem på vårt eget sätt och bete oss därefter.

Faktum är att funktioner i sig inte kräver felhantering, vilket kan leda till ödesdigra problem, men David har skrivit om detta i <a href="https://phpfashion.com/programatori-chyby-neignoruji">Programmerare ignorerar inte fel</a>.

Ett exempel på glömd felhantering:

```php
// flyttar från disk till disk
copy('c:/oldfile', 'd:/ny fil');
unlink('c:/oldfile');
// Om den första åtgärden misslyckas raderas filen oåterkalleligt.
```

Detta beror på att det korrekta sättet att hantera utdata från funktionen "copy()`" är att inte fortsätta och att kasta ett fel. När det gäller gamla funktioner kan det se ut så här:

```php
function backup(): bool
{
   if (copy('c:/oldfile', 'd:/ny fil')) {
      return unlink('c:/oldfile');
   }

   return false;
}
```

Vår funktion `backup()` kommer att återge `true` endast om funktionen `copy()` inte misslyckades och funktionen `unlink()` återgav `true`. I annat fall returneras `false`.

Men är detta nu säkert för tillämpningen? Det är det inte! Eftersom vi nu måste behandla resultatet av funktionen "backup()` vid den tidpunkt då vi anropar den, och om den misslyckas vet vi inte ens varför. Kort sagt, den returnerar `false` och vi måste upptäcka felet själva på något sätt. Jag antar att det i det här fallet är bra att se att programmerare ofta ger upp felhanteringen, eller helt enkelt glömmer att hantera något och att programmet därför skapar fel som är svåra att upptäcka.

Lösningen på detta problem är att använda undantag som tvingar fram behandlingen, och om de inte behandlas kraschar programmet helt och hållet och vi får alltid reda på varför.

Grundläggande definition av undantag
--------------------------

I PHP är ett undantag en speciell typ av gränssnitt som implementeras av den inhemska klassen `Exception` som vi kommer att använda.

Om bearbetningen av någon del av programmet misslyckas kastar vi helt enkelt ett undantag som beskriver problemet:

```php
if (copy('c:/oldfile', 'd:/ny fil') === false) {
   throw new \Exception('Kan inte kopiera filen "oldfile".');
}
```

Att kasta ett undantag görs med nyckelordet `throw`, följt av att skapa en instans av klassen med undantaget. Vi kan också få en instans på andra sätt (t.ex. genom att skicka den från en variabel), och att bara skapa en undantagsinstans gör inte att den kastas.

Det första argumentet i konstruktören för klassen `\Exception` accepterar undantagets text, som på ett kortfattat sätt ska förklara vad som just hände. Det är bra att även inkludera information om den operation som utförs och en hänvisning till uppgifterna. Om en filkopiering har misslyckats är det till exempel bra att lämna filnamnet. Om utförandet av SQL-frågan misslyckas, skickar vi återigen över den fråga som utförs. Detta kommer att hjälpa oss mycket senare när vi hanterar fel, eftersom vi kan se exakt vad problemet är.

Hantering av undantag
-----------------

Låt oss till exempel ha en funktion `backup()` som säkerhetskopierar data och som kan kasta ett par fel:

```php
function backup(): void
{
   if (copy('c:/oldfile', 'd:/ny fil')) {
      if (unlink('c:/oldfile') === false) {
         throw new \Exception('Det går inte att ta bort gamla filer.');
      }
   }

   throw new \Exception('Kan inte kopiera säkerhetskopior.');
}
```

Observera att funktionen inte returnerar något utdata och att vi anger typen `void` i definitionen. Funktionen behöver inte returnera något, eftersom framgång anses vara ett tillstånd där inget fel uppstår och vi behöver inte behandla ett positivt scenario.

Om vi skulle använda funktionen i en tillämpning utan behandling, till exempel på följande sätt:

```php
echo 'Säkerhetskopiera filer...';
backup();
echo 'Säkerhetskopieringen är klar.';
```

Det är det normala sättet att fungera. Om ett fel inträffar avslutas dock skriptet automatiskt och undantagstexten visas i utmatningen. Det är viktigt att den inte fortsätter att köra koden och vi vet att inga data kommer att skadas.

Om vi vill fortsätta att köra måste vi **rensa** felet, vilket vi gör genom att använda konstruktionerna `try` och `catch`:

```php
echo 'Säkerhetskopiera filer...';
try {
   backup();
} catch (\Exception $e) {
   echo 'Säkerhetskopieringen misslyckades:' . $e->getMessage();
}
echo 'Säkerhetskopieringen är klar.';
```

Om ett undantag uppstår anropas koden i området `catch()` (som accepterar undantaget om det matchar dess datatyp) och den interna koden körs.

Vi får alltid en instans av undantagsklassen, som kan användas för att till exempel visa ett felmeddelande, vilket hanteras av metoden `getMessage()`. Det är också bra att känna till metoden `getFile()`, som returnerar diskvägen till filen som innehåller felet, `getCode()`, som returnerar felstatuskoden, och `getLine()`, som returnerar radnumret där undantaget utlöstes.

Förberedda undantag
------------------------

Förutom det grundläggande undantaget `\Exception` innehåller PHP andra fördefinierade undantagstyper som lämpar sig för olika användningsområden.

| Datatyp | Förklaring |
|------------|-----------|
| `LogicException` | Logiskt fel, förutsägbart vid programutformningen |
| `BadFunctionCallException` | Fel i funktionskallning; funktionen finns inte; anropet är inte tillåtet |
| `BadMethodCallException` | Fel i metodanropet |
| `InvalidArgumentException` | Felaktigt (ogiltigt) argument som överlämnats till en funktion eller metod |
| `OutOfRangeException` | Värde utanför området för array eller samling |
| `LengthException` | Värdet överskrider tillåten längd |
| `DomainException` | Värdet faller inte inom den önskade domänen eller det önskade intervallet |
| `RuntimeException` | Fel som endast kan upptäckas vid körning (t.ex. misslyckande med att skriva till en fil) |
| `OverflowException` | Buffert- eller aritmetiskt överflöde, ofta orsakat av att mer data än förväntat behandlas |
| `UnderflowException` | Underflöde av en buffert eller aritmetisk operation, färre data än förväntat överfördes |
| `OutOfBoundsException` | Index utanför området för array eller samling |
| `RangeException` | Värdet ligger inte inom det begärda intervallet |
| `UnexpectedValueException` | Oväntat värde (t.ex. returvärdet av en funktion) |

Undantagen `LogicException` och `RuntimeException` bör förhindras genom korrekt programdesign. Personligen använder jag dem endast i exceptionella situationer, t.ex. när det inte går att skriva till en fil eller när man kommunicerar med en extern tjänst.

Jag rekommenderar att du inte fångar upp `RuntimeException` alls och låter programmet misslyckas. Detta är vanligtvis ett allvarligt problem som bör rapporteras så snart som möjligt.
