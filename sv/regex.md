Reguljära uttryck i PHP
=======================

> id: '32142161-6ace-4cfd-b472-b48031219e9b'
> slug:
> 	cs: regex
> 	sv: reguljaera-uttryck-i-php
> 
> perex:
> 	- 'Regulární výrazy a jejich kompletní vysvětlení. Hledání podřetězce, pokročilé nahrazování a generování řetězců.'
> 	- 'Reguljära uttryck och en fullständig förklaring av dem. Sökning efter delsträngar, avancerad substitution och generering av strängar.'
> 
> publicationDate: '2020-03-08 13:37:38'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '392c8f14e6943dd8f345a9a134e0de92'

Reguljära uttryck är verktyg som gör det enkelt att söka, validera, jämföra, dela upp, sammanfoga och ersätta strängar enligt en mask (mönster). Det är ett mycket kraftfullt och elegant verktyg för avancerad strängmanipulering.

Mask
-----

I början måste vi först komma fram till det reguljära uttrycket som vi ska utföra. Den anges som en textsträng som har en mängd regler och konfigurationsalternativ (detta är en mycket komplex teknik).

Till att börja med är det viktigt att notera att det reguljära uttrycket utvärderas sekventiellt från vänster till höger, och om det finns flera sätt att tolka strängen används alltid den största möjliga överensstämmelsen (det beter sig **hungrigt** och försöker bearbeta så många tecken som möjligt).

Ett reguljärt uttrycks beteende och bearbetningsstrategi kan påverkas av många olika inställningar.

Enkel kontroll av att strängen är ett giltigt e-postmeddelande
-----------------------------------------------

Hur kan vi enkelt kontrollera att strängen `jan@barasek.com` är en giltig e-postadress utan att behöva dela upp den i komplexa delar eller gå igenom den tecken för tecken?

Reguljära uttryck ger svaret (uttrycket ovan är mycket förenklat för exemplets skull, och en riktig implementering av validering av e-postadresser bör vara lite mer komplicerad):

```php
$mail = 'jan@barasek.com';
$regex = '/^.+@.+\.(en|en|com)$/';

if (preg_match($regex, $mail)) {
   echo 'E-postmeddelandet är giltigt';
} else {
   echo 'E-postmeddelandet är inte giltigt';
}
```

Låt oss undersöka uttrycket `/^.+@.+\.(en|en|com)$/` lite mer i detalj:

Först måste vi omsluta hela uttrycket med ett par `/`-tecken (i början och i slutet) som talar om var uttrycket börjar och slutar. Efter `/` i slutet av uttrycket följer eventuella modifieringar (inställningar för uttryckets bearbetningsläge).

När du bearbetar ett uttryck går du från vänster sida tecken för tecken. Var och en av dem har sin egen betydelse, som anges i följande tabell:

| Tecken | Betydelse | Beskrivning | Exempel |
|------|-----------------|-------|-------------
| `^` | Strängens början | Tvingar fram att strängen måste börja vid denna punkt. | Tvingar fram att strängen måste börja med sekvensen `+420` (användbart för nummervalidering, till exempel): `/^+420/`. |
| `$` | Slut på sträng eller rad | Tvingar fram att strängen eller raden måste sluta här. Linjeslutet undviks sedan med `\z`. <a href="https://phpfashion.com/vite-co-znamena-v-regularnim-vyrazu">Detaljerad förklaring</a>. | Filnamnet måste vara en textfil (som slutar med ett punktum och sedan strängen "txt"): `/\.txt$/`. |
| `.` | Alla tecken | Fångar upp absolut alla tecken. | Kontrollerar att strängen innehåller exakt ett av alla tecken: `/^.$/`. |
| `\d` | Nummer | Identifierar tecken `0-9` | Identifierar ett telefonnummer som inte innehåller några mellanslag och har 9 siffror: `/^(\+420)?\d{9}$/`. |
| `\s` | Whitespace | Fånga upp mellanslag, bindestreck och tabulatorer. | Tillåter mellanslag mellan siffror i ett telefonnummer i tresiffriga siffror: `/^(\d{3}\s?){3}$/`. |
| `+` | Flera tecken, men minst ett | Upprepar föregående deluttryck och försöker fånga upp så mycket som möjligt. Deluttrycket måste upprepas minst en gång. | Fångar upp så många siffror som möjligt, men minst en: `/\d+/`. |
| `*` | Flera tecken, kan vara inga | Fungerar på samma sätt som `+`, men kan fånga upp en tom sträng (värdet behöver inte finnas). | Fångar upp så många siffror som möjligt, om det inte finns några, fångar upp en tom sträng: `/\d*/`. |
| `(` ` `)` | Parenteser | Anger ett deluttryck. Detta kan användas för att innesluta flera olika taggar och sedan kräva, till exempel, upprepning över dem, eller för att fånga deras innehåll i en variabel. | Låt oss dela upp postnumret i två delar enligt mellanslag, vilket är valfritt och det kan till och med finnas mer än ett: `/^(\d{3})\s*(\d{2})$/` |
| `\|` | Eller | Innehåller ett deluttryck, eller ett annat deluttryck. | Tal som börjar med `+420` eller `+421`: `/^+(420\|421)\s*\d+$/`. |
| `\.` | Escaping | Om vi vill fånga upp ett tecken i ett uttryck som annars har en speciell betydelse måste vi escaped det på samma sätt som till exempel strängar i PHP. | Fångar upp ett par siffror separerade med en punkt (om vi inte escaped punkt skulle det förstås som "vilket tecken som helst"): `/\d+\d.\d+/`.

För fullständighetens skull ger jag den fullständiga formen av valideringsregeln för e-post som Nette har infört:

```php
/**
 * Tar reda på om en sträng är en giltig e-postadress.
 */
public static function isEmail(string $value): bool
{
   $atom = "[-a-z0-9!#$%&'*+/=?^_`{|}~]"; // RFC 5322 icke citerade tecken i den lokala delen
   $alpha = "a-z\x80-\xFF"; // överordning av IDN
   return (bool) preg_match("(^
      (\"([ !#-[\\]-~]*|\\\\[ -~])+\"|$atom+(\\.$atom+)*)  # quoted or unquoted
      @
      ([0-9$alpha]([-0-9$alpha]{0,61}[0-9$alpha])?\\.)+    # domain - RFC 1034
      [$alpha]([-0-9$alpha]{0,17}[$alpha])?                # top domain
   \\z)ix", $value);
}
```

`preg_match()` - validering efter mönster
------------------------------------

Den grundläggande funktionen för formatvalidering och parsning är `preg_match()`, den har två obligatoriska parametrar och den tredje kan användas för att ange utmatningsfältet.

Exempel:

```php
$psc = '272 01'; // Kladno

if (preg_match('/^(\d{3})\s*(\d{2})$/', $psc, $parser)) {
   echo 'Postnumret är giltigt [' . $parser[1] . ','. $parser[2] . ']';
} else {
   echo 'Postnumret är ogiltigt';
}
```

Koden returnerar: "Koden är giltig [272, 01]`".

Observera den enkla parentesen, som vi använde för att dela upp uttrycket i flera mindre delar. Detta gör det möjligt att hämta de enskilda deluttrycken som arrayposter. Hela funktionen returnerar sedan `true` eller `false` beroende på om strängen fångades upp.

Ibland är det dock svårt att navigera i parentesernas numeriska ordning, eftersom antalet kan ändras eller det helt enkelt kan vara för många av dem. I det här fallet är det möjligt att namnge parenteserna individuellt och sedan komma åt nycklarna med hjälp av deras namn.

Till exempel:

```php
$phone = '777 123 456';

preg_match('/^(?<operator>\d{3})\s*(?<number>[0-9 ]+)$/', $phone, $parser);

echo $parser['operatör']; // returnerade 777
```

`preg_replace()` - ersättning enligt mönster
----------------------------------------

Det är också möjligt att ersätta strängar med hjälp av regex, vilket är särskilt användbart för olika formatkorrigeringar efter användaren.

Anta att vi vill lagra ett telefonnummer som användaren anger i ett heltal, eftersom detta krävs av ett bibliotek från en tredje part, men användarna kan ange det i ganska vilda format.

I så fall håller jag mig till dikten:

> "Var generös i det du tar emot och sträng i det du skickar".

Det är därför vi automatiskt anpassar formatet. Först använder vi parsing för att bryta upp strängen i dess enskilda delar och sedan viker vi tillbaka den i enlighet med parentesnumren:

```php
function formatPhoneNumber(string $phoneNumber): int
{
   return (int) preg_replace(
      '/^(\+\d{3})\s*(\d{3})\s*(\d{3})\s*(\d{3})$/',
      '$2$3$4',
      $phoneNumber
   );
}

echo formatPhoneNumber('+420 777 123 456');
```

Konstruera en sträng enligt ett reguljärt uttryck
----------------------------------------

Regexer är också mycket användbara när du vill generera nya strängar enligt ett komplext mönster.

Ren PHP har inget stöd för detta, men vi kan ladda ner ett bibliotek <a href="https://github.com/icomefromthenet/ReverseRegex">ReverseRegex</a> från en tredje part som kan göra detta.

Vi kan till exempel vilja generera en uppsättning lösenord baserat på regexet `[a-z]{10}` och ingenting kan stoppa oss:

```txt
jmceohykoa
aclohnotga
jqegzuklcv
ixdbpbgpkl
kcyrxqqfyw
jcxsjrtrqb
kvaczmawlz
itwrowxfxh
auinmymonl
dujyzuhoag
vaygybwkfm
```

Användningen är följande:

```php
use ReverseRegex\Lexer;
use ReverseRegex\Random\SimpleRandom;
use ReverseRegex\Parser;
use ReverseRegex\Generator\Scope;

require 'säljare/autoload.php';

$lexer = new  Lexer('[a-z]{10}');
$gen   = new SimpleRandom(10007);
$result = '';

$parser = new Parser($lexer, new Scope(), new Scope());
$parser->parse()->getResult()->generate($result, $gen);

echo $result;
```

Jag skapar mina matematiska exempel i Nette in Presenter på detta sätt, och det är mycket enkelt:

```php
public function actionRegex(): void
{
   $lexer = new Lexer('\d{1,3}[\+\-\*\/]');
   $parser = new Parser($lexer, new Scope(), new Scope());
   for ($i = 0; $i <= 10; $i++) {
      $result = '';
      $gen = new SimpleRandom($i);
      $parser->parse()->getResult()->generate($result, $gen);
      dump($result);
   }
   $this->terminate();
}
```

Det viktiga för biblioteket är att det fortfarande genererar samma utdata för samma indata (även om det kan verka som om det finns många möjliga strängar att matcha för varje reguljärt uttryck). Om vi vill ändra det genererade uttrycket slumpmässigt måste vi också ändra det "frö" som genererar utdatasträngen. Antingen kan du använda dig av att gå igenom fröintervallet eller kanske funktionen `rand(1, 1e6)` för att göra detta.

Fångst och behandling av fel
-----------------------------

I PHP är det ganska svårt att fånga upp fel i regexer, men det finns ändå en lösning.

Detta förklaras i detalj i artikeln <a href="https://phpfashion.com/zradne-regularni-vyrazy-v-php">Treachable regular expressions in PHP</a> av David Grudel.
