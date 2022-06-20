Villkor i PHP - IF() {...} - förgreningsalternativ
==================================================

> id: '3e4b81bb-a8bb-4e99-8842-e76f1658a371'
> slug:
> 	cs: if
> 	sv: villkor-i-php-if-foergreningsalternativ
> 
> perex:
> 	- 'Podmínky, logické výrazy, booleovská logika a možnosti větvení PHP scriptů. Vyhodnocování logických výrazů a operátory. Možnosti skládání výrazu.'
> 	- 'Villkor, boolesk logik, boolesk logik och förgreningsalternativ för PHP-skript. Utvärdering av booleska uttryck och operatörer. Alternativ för att fälla uttrycket.'
> 
> publicationDate: '2019-11-26 11:55:09'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '0d8a00928bcfa899d4e6dbf6c067394e'

> **Note:** Den här artikeln kan vara lite rörig för vissa nybörjare eftersom den förutsätter en grundläggande förståelse för PHP. Om du är intresserad av hur villkor fungerar kan du läsa <a href="/villkor">om villkor i nybörjarkursen</a>.

| Stöd: | Alla versioner: PHP 4, PHP 5, PHP 7
|-------------------|----------|
| Kort beskrivning: | Validering av en eller flera uttalanden
| Typ: | <a href="/statements-and-functions">Statement, konstruera</a> (inte en funktion)

Beskrivning
-----

Ofta måste du avgöra om en jämlikhet gäller eller om ett påstående är sant, det är vad villkor är till för. PHP använder följande syntax som många andra språk (särskilt C):

```php
if (/* logiskt uttalande */) {
    // konstruera
}
```

Varje logiskt uttryck har värdet `TRUE` (sant) eller `FALSE` (falskt), det finns inga andra alternativ.

Exempel på att jämföra om variabeln `$x` är större än variabeln `$y`:

```php
$x = 10;
$y = 5;

if ($x > $y) {
    // Denna del av skriptet utförs om villkoret är sant.
} else {
    // Den här delen av skriptet körs om villkoret inte gäller.
}
```

Konditionskonstruktionen har ett obligatoriskt innehåll inom runda parenteser, där uttrycket som ska testas anges och som består av operatörer (översikt nedan). Flera uttryck kan kopplas samman med hjälp av logiska operatörer (översikt nedan).

Dessutom innehåller villkoret två valfria förklaringsblock.

- Om villkoret är uppfyllt
- Om villkoret inte är uppfyllt

Av praktiska skäl ska du alltid inkludera åtminstone det första blocket av påståenden när villkoret är sant, annars skulle det inte vara meningsfullt att testa uttrycket.

Användning av semikolon och parenteser
--------------------------

I allmänhet:
- **Runda** parenteser används för att separera logiska uttryck (andra runda parenteser kan användas för att uppnå mer komplexa uttryck).
- Den **komplexa** parentesen används för att avgränsa ett block av kommandon och funktioner.
- **mittfältet** används inte för att ange ett villkor (blocket av kommandon avgränsas av en sammansatt parentes), utan för att separera de enskilda kommandona inom villkoret.)

Den enda möjliga notationen med semikolon (utom när du använder konstruktionen **endif**):

```php
if ($x > $y);
```

Ett sådant villkor är dock meningslöst eftersom resultatet av jämförelsen i båda fallen kommer att förkastas och inget uttalande som hör till villkoret kommer att utföras.

Alternativa beteckningar
--------------------------

Under vissa omständigheter kan konstruktionen `if` användas utan att de sammansatta parenteserna används. Detta kan endast uppnås i följande fall:

- Om vi bara utför ett uttalande i villkoret.
- Om vi använder kolon och endif i stället för sammansatta parenteser;
- Om vi använder "in-line"-notering.

Mer detaljerad information finns i de tre följande kapitlen.

> **`1. Endast ett kommando ~ förkortad syntax`**

Om du skapar ett villkor där du vill utföra endast en konstruktion (ett uttalande) kan du använda antingen den klassiska sammansatta parentesnotationen:

```php
if ($x > 10) { $y = $x; }
```

Eller så kan vi utelämna parenteserna:

```php
if ($x > 10) $y = $x;
```

Detta beteende gäller dock endast för ett kommando i tillståndets omedelbara närhet.

Ett bättre exempel (endast konstruktionen `$y = $x` utförs villkorligt, resten utförs alltid):

```php
$x = 5;
$y = 3;
$z = 10;
if ($x > $y)
$y = $x;
$x = 3;
```

> **`2. Kolon och endif;`**

```php
if (/* uttryck */):
    konstrukt;
    konstrukt;
    konstrukt;
endif;
```

Denna notation har dock länge ansetts vara föråldrad eftersom den minskar i orientering när flera förhållanden är nedsänkta i varandra.

> **Notis:** Jag vill notera att denna stil också uppskattas av vissa personer, t.ex. Yuh (<a href="https://www.jakpsatweb.cz/php/php-tahak.html#vetveni">se hans artikel</a>). Gud förbjude att du använder den här någonstans.

> **`3. Tärnära uttryck ~ en enda rad "in-line"-notation`**

Ibland kan det vara bra att göra en enkel jämförelse på samma gång som en annan åtgärd (t.ex. tillsammans med att definiera en ny variabel). Om vi vill utföra endast ett uttalande kan hela proceduren reduceras till en enda rad, samtidigt som den hålls så enkel som möjligt.

```php
$x = 5;
$isBiggerThanTwo = ($x > 2 ? true : false);

// eller ännu kortare:

$isBiggerThanTwo = ($x > 2);

// eller utan parenteser:

$isBiggerThanTwo = $x > 2;
```

Operatörsbord
-----------------

Två typer av operatörer används i villkoret:

- **Komparativ** ~ de jämför ett specifikt förhållande,
- **Logiskt** ~ kombinera flera uttryck för att skapa komplexa villkor.

Jämförande aktörer
-----------------------

| Operatör | Betydelse |
|----------|----------|
| `==` | Liknar
| `===` | Är likvärdig och har samma datatyp
| `!=` | Är inte lika med
| `>=` | Är lika eller större
| `<=` | Lika eller mindre
| `>` | Större
| `<` | Mindre

Exempel (giltigt när `$x inte är 5`):

```php
if ($x != 5) { ... }
```

Logiska operatörer
--------------------

| Operatör | Alternativ | Betydelse | Sant när:
|-----------|--------------|----------------|--------------------------------------------
| `&&` | AND | och samtidigt | båda värdena är sanna
| `|||` | OR | eller | minst ett värde är sant
| `^^` | XOR | exclusive OR | minst en är sann eller falsk, men aldrig båda.
| `!` | *Doesn't* | negation av uttryck | `true` när `false` och vice versa
| `()` | *gör inte* | uttryck negation| beror på omständigheterna

Ett mer komplicerat exempel:

```php
$x = 5;
$y = 3;
$z = 8;
if ($x > 0 && !($y != 2 && $z == $x) || $z > $y) { ... }
```

Utelämna logiska och jämförelseoperatörer
---------------------------------------------

Ofta har vi råd att utelämna någon av operatorerna (eller till och med båda), men vi får aldrig glömma reglerna för korrekt användning för att få det resulterande uttrycket att fungera.

När vi testar ett uttryck utan operatör testar vi i allmänhet om dess värde är "sant" eller om det inte är tomt (t.ex. om det innehåller ett tal som inte är noll, en sträng som inte är tom, ...).

Exempel:

```php
$x = 5;
$y = 3;
$z = 8;

if ($x) { ... }         // går igenom eftersom $x inte är tomt
if ($x && $y) { ... }   // går igenom eftersom $x och $y inte är tomma
if (!$x) { ... }        // misslyckas eftersom TRUE är negerad
if (isset($z)) { ... }  // går igenom eftersom variabeln $z finns
```

Men det kan uppstå svåra situationer, särskilt när:
- Jag frågar efter `if ($x)` och om variabeln `$x` innehåller noll (`0`) är villkoret inte uppfyllt.
- Eller så innehåller variabeln `$x` strängen ``0``` (siffran noll), eftersom den går över till noll och uttrycket därför inte är sant.
- Det finns en funktion i villkoret som alltid returnerar en icke-tom sträng, eftersom villkoret då alltid är sant.
- Eller om vi kontrollerar inmatningen från användaren och han returnerar "false" som en sträng, är villkoret återigen sant eftersom strängen inte är tom.

Jag rekommenderar en enkel och effektiv lösning på detta - fråga efter antalet tecken som returneras. Om strängen är tom (eller om variabeln inte finns) returneras noll tecken och villkoret uppfylls inte. Ett enkelt exempel:

```php
$x = '0';
if ($x) { ... }			// villkoret gäller normalt inte
if (strlen($x)) { ... }	// villkoret är giltigt eftersom $x innehåller 1 tecken
```

Därefter kan vi testa om en variabel finns med hjälp av funktionen `isset()`.

Jämförelse av strängar
-----------------

Det är lätt att konstatera att strängarna är identiska:

```php
$a = 'Katt';
$b = 'cat';

if ($a === $b) {
    // Om strängarna är desamma
} else {
    // Om strängarna är olika
}
```

Det är viktigt att hålla ett ordentligt öga på datatyperna om en post kan vara likvärdig med någon annan.

Den tomma strängen `$a = '';` skiljer sig till exempel från strängen `NULL`: `$b = NULL;`. Vi måste göra denna skillnad, till exempel på grund av databaser där det är skillnad mellan att ett värde inte finns och att det är tomt.

```php
$a = '';
$b = null;

if ($a == $b) {
    // Det kommer att utvärderas som TRUE eftersom
    // datatypen konverteras.
}

if ($a === $b) {
    // Utför en mycket noggrannare validering
    // och det går inte igenom eftersom det är en annan
    // innehåll och en annan datatyp, därför
    // den här koden kommer aldrig att köras.
}
```

Det är också en bra idé att ignorera vita (osynliga) tecken som mellanslag, tabulatorer och radbrytningar när du jämför strängar. Detta är till exempel användbart när du anger ett lösenord och skickar det till en hashfunktion:

```php
$password = '81dc9bdb52d04dc20036dbd8313ed055'; // 1234
$userPassword = '1234';

if (md5(trim($userPassword)) === $password) {
    // Funktionen trim() tar automatiskt bort mellanslag.
}
```

Okänt (obefintligt) värde?
--------------------------

Ibland kan det hända att värdet inte existerar (det är varken `TRUE` eller `FALSE`), det är främst ett värde som hämtas från databasen (till exempel frågar vi efter en kolumn som inte finns), i det här fallet returneras datatypen `NULL`.

I allmänhet utvärderas `NULL` som `FALSE`, dvs. villkoret gäller inte. Detta beteende är dock inte alltid praktiskt, eftersom ett obefintligt värde inte nödvändigtvis innebär att det inte finns någon post.

> Exempel från praktiken: Vi har en användarprofil och frågar efter användarens webbsida. Alla användare behöver inte ha en webbsida, så i det här fallet returneras `NULL`, men användaren existerar fortfarande. I det här fallet bör vi hellre använda funktionen `isset()` för att testa om variabeln finns (eller inte finns) och inte dra en slutsats utifrån ett specifikt värde.
