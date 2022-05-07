Matematik i PHP
===============

> id: '69a432ee-7635-46e2-bb21-8492cb62c4e6'
> slug:
> 	cs: matematika
> 	sv: matematik-i-php
> 
> perex:
> 	- 'Matematika a matematické funkce v PHP. Definice čísel, operátorů a vhodné typy pro výpočty a ukládání čísel.'
> 	- 'Matematik och matematiska funktioner i PHP. Definitioner av tal, operatorer och lämpliga typer för beräkningar och lagring av tal.'
> 
> publicationDate: '2020-02-16 18:24:10'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '1070f11c80eff1faf3388e47721b01a2'

Precis som i andra språk representeras siffror i PHP i det binära systemet (systemet med ettor och nollor), så vissa matematiska operationer kan ge något annorlunda resultat än vad vi skulle förvänta oss. Den här sidan ger en översikt över hur beräkningar fungerar i olika sammanhang. Minst grundläggande kunskaper om **numeriska tekniker** och **nummersystem** krävs för att förstå.

Representation av tal i minnet
---------------------------

Det sätt på vilket tal representeras kännetecknas av datatypen, till exempel omvandlas heltal direkt från källkoden från decimal till binär. Vi behöver inte oroa oss för att konvertera siffror alls, allt sker automatiskt.

Konsekvensen av denna omvandling är att det maximala värdet av ett heltal bestäms av antalet tillgängliga bitar i minnet. För 32-bitars PHP är intervallet från `-2 147 483 648` till `-2 147 483 647` (~ ± 2 miljarder), 64-bitars PHP har ett intervall från `-9 223 372 036 854 775 808` till `-9 223 372 036 854 775 807` (~ ± 9 kvintiljoner). Det maximala värdet kan alltid erhållas genom att anropa konstanten `PHP_INT_MAX`.

Decimaltal lagras i datatypen **float**, som har begränsad precision, så det lagras internt som ett par tal som är föremål för den matematiska operationen `mantissa * (2^exponent)`. Den praktiska konsekvensen är att vi kan lagra ett relativt stort antal (i storleksordningen miljarder) på ett litet antal bitar, men inte särskilt exakt.

Konsekvensen av att använda datatypen **float** är att resultatet av beräkningen av `1 - 0,9` inte är exakt `0,1`.

Exempel från <a href="https://diskuse.jakpsatweb.cz/?action=vthread&forum=9&topic=97912">webbforum</a>:

```php
$x = 0.3;
$y = 0.4;
echo 0.7 - $x - $y; // skriver ut -5.5511151231258E-17
```

Om vi justerar siffrorna något:

```php
$x = 6.5;
$y = 7.5;
echo 14.0 - $x - $y; // skriver ut 0
```

I allmänhet diskuteras denna fråga <a href="https://vtm.zive.cz/clanky/proc-pocitacum-delaji-problemy-desetinna-cisla/sc-870-a-185672/default.aspx">i en separat artikel på VTM.e15.cz: Why computers have problems with decimal numbers</a>, eller i allmänhet om <a href="https://cs.wikipedia.org/wiki/Pohyblivá_řádová_čárka">flytande punkt på Wikipedia</a>.

> I allmänhet är det en bra idé att avrunda resultatet efter beräkningen eller att undvika decimaler helt och hållet. Lagra t.ex. priset inte i kronor (t.ex. 14,90) utan i pennies (t.ex. 1490) och arbeta sedan med det exakt. Avrundning diskuteras i nästa avsnitt av denna artikel.

Matematiska operationer
-------------------

```php
$a = 5;
$b = 3;

echo $a + $b;
```

Vanliga matematiska konventioner kan användas för operationer, som respekterar operatörernas företräde enligt matematikens regler (multiplikation har företräde framför addition osv.). Värden kan skrivas direkt eller läsas via variabler.

> Det kanske inte är uppenbart vid första anblicken, men i räkneexemplet är likhetstecknet (`=`) inte skrivet. Av detta följer att endast enkla "binära operationer" kan lösas, som alltid fungerar med endast "två element" (räkna inte ekvationer, du måste använda ett specialiserat bibliotek för det).

Översikt över grundläggande operatörer
----------------------------

> I kolumnen `result` listar jag vad som skrivs ut och antar:

```php
$a = 5;
$b = 3;
$c = 10;
```

| Operation | Markera | Markera med ord | Exempel på notation | Resultat
|-------------------|-----------|---------------|-----------------------|----------
| Addition | `+` | Plus | `echo $a + $b;` | 8
| Subtraktion | `-` | Minus | `echo $a - $b;` | 2
| Multiplikation | `*` | Asterisk | `echo $a * $b;` | 15
| division | `/` | snedstreck | `echo $a / $b;` | 1.66666666666666667
| parentes | `( )` | parentes | `echo $a + ($b * $c);`| 35
| String concatenation | `.` | Period | `echo $a . $b . $c;` | 5310
| Rest efter division | `%` | Procent | `echo $a % $b;` | 2
| Lägg till en | `++` | två plus | `echo $a++;` | 6
| Subtrahera ett | `--` | två minus | `echo $a--;` | 4
| Power | `**` | två asterisker | `echo $a ** $b;` | 125

> Power-operatorn (dubbel asterisk) är endast tillgänglig från och med PHP 7.1. I andra versioner av PHP måste du använda den universella funktionen `pow($a, $b)`.

Översikt över grundläggande matematiska funktioner
---------------------------------------

Om du löser någon vanlig beräkningsuppgift finns det sannolikt redan en funktion direkt i PHP, här är en kommenterad översikt över dem.

Avrundning
--------------

Normal avrundning görs med funktionen <a href="/function-round">round()</a>, som har tre parametrar:

- Avrundat tal
- Valfritt: Precision (hur många decimaler)
- Valfritt: Halvering av värdet (från vilket värde ska man avrunda uppåt)

```php
round(5);               // 5
round(5.1);             // 5
round(5.4);             // 5
round(5.5);             // 6
round(5.8);             // 6
round(5483.47621, 2);   // 5483.47
round(5483.47621, -2);  // 5500
```

Det finns också en <a href="function-floor">floor()</a> funktion för avrundning nedåt och en <a href="/function-ceil">ceil()</a> funktion för avrundning uppåt.

> **Var försiktig med datatyper!**
>
> Alla avrundningsfunktioner returnerar resultatet som `float`, även om resultatet är ett heltal.
>
> > Om du vill behandla resultatet som ett heltal är det viktigt att övertypa resultatet. Till exempel: `echo (int) round(3.14);`

PHP arbetar med olika datatyper, t.ex. med **float** kan det lätt hända att resultatet av `round()` inte är ett tal med finit decimalutrymme. För listning rekommenderar jag att du använder funktionen `number_format()`, som jag diskuterar nedan.

Goniometriska funktioner
--------------------

Dessa används för många olika beräkningar och är baserade på enhetscirkeln. De används t.ex. för att plotta cirklar, ellipser, förskjutningar osv.

- <a href="/funktion-sin">sin()</a>
- <a href="/funktion-cos">cos()</a>
- <a href="/funktion-tan">tan()</a>

Cyklometriska funktioner
--------------------

Beräkna vinkeln mellan `x` och `y`. Konvertering av kartesiska och polära koordinater.

- <a href="/funktion-asin">asin()</a>
- <a href="/funktion-acos">acos()</a>
- <a href="/funktion-atan">atan()</a>

Hyperboliska funktioner
-------------------

Baserat på enhetshyperbola. Deras definitioner kan enkelt beskrivas med hjälp av liknelser:

- Ellips, cirkel, cirkel med radie 1
- Hyperbola, jämviktshyperbola, enhetshyperbola

De används t.ex. för terränggenerering och fysisk simulering.

- <a href="/funktion-sinh">sinh()</a>
- <a href="/funktion-cosh">cosh()</a> (kedjelinje)
- <a href="/funktion-tanh">tanh()</a>

Hyperbolometriska funktioner
------------------------

Baserat på enhetshyperbola.

- <a href="/funktion-asinh">asinh()</a>
- <a href="/funktion-acosh">acosh()</a>
- <a href="/funktion-atanh">atanh()</a>

Exempel på användning av goniometriska funktioner
---------------------------------------

```php
echo sin(30);            // -0.98803162409286
echo sin(deg2rad(30));   // 0.5
echo cos(deg2rad(123));  // -0.54463903501503
echo 1/tan(deg2rad(45)); // 1 (onen kotangent)
echo sin(deg2rad(500));  // 0.64278760968654
echo atan2(deg2rad(50), deg2rad(23)); // 1.1396575860761
```

Kalkylator - bearbetning av matematiska uttryck
--------------------------------------------

Ibland kan det hända att vi behöver behandla ett matematiskt uttryck som en användarsträng, till exempel `5+2^(1+3/2)`.

Det finns inget enkelt sätt att bearbeta sådan inmatning i PHP, men jag har programmerat en rad bibliotek som tar itu med detta problem.

Mer information finns i den separata artikeln <a href="/pokrocila-calculator">Räknare i PHP: Bearbetning av ett matematiskt uttryck som en sträng</a>.

Arbete med stora tal och noggrannhet i beräkningarna.
------------------------------------------

Stora tal och decimaler lagras som floats i PHP, och de avrundas till cirka 15 decimaler, vilket ibland kan vara irriterande. Därför skapades **<a href="https://www.php.net/manual/en/book.bc.php">BCMath Arbitrary Precision Mathematics library</a>**.

Tal representeras som **strings** i det här biblioteket, så de är inte begränsade av **float**:s oprecisionsgrad, men de utförda operationerna är flera storleksordningar långsammare (men fortfarande snabbare än om vi programmerade en sådan funktion själva).

Om vi till exempel vill få fram en kvadratrot från 2 till 3 decimaler kallar vi bara:

```php
echo bcsqrt('2', 3); // 1.414
```
