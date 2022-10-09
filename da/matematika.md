Matematik i PHP
===============

> id: '69a432ee-7635-46e2-bb21-8492cb62c4e6'
> slug:
> 	cs: matematika
> 	da: matematik-i-php
> 
> perex:
> 	- 'Matematika a matematické funkce v PHP. Definice čísel, operátorů a vhodné typy pro výpočty a ukládání čísel.'
> 	- 'Matematik og matematiske funktioner i PHP. Definitioner af tal, operatorer og passende typer til beregninger og lagring af tal.'
> 
> publicationDate: '2020-02-16 18:24:10'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '1070f11c80eff1faf3388e47721b01a2'

Som i andre sprog er tal i PHP repræsenteret i det binære system (systemet med ettere og nuller), så nogle matematiske operationer kan give lidt anderledes resultater, end vi ville forvente. Denne side giver et overblik over, hvordan beregninger opfører sig i forskellige sammenhænge. Der kræves mindst et grundlæggende kendskab til **numeriske teknikker** og **talsystemer** for at kunne forstå det.

Repræsentation af tal i hukommelsen
---------------------------

Måden, hvorpå tal repræsenteres, er karakteriseret ved datatypen, f.eks. konverteres integer direkte fra kildekoden fra decimal til binær. Vi behøver slet ikke at bekymre os om at konvertere tal, det hele sker automatisk.

Konsekvensen af denne konvertering er, at den maksimale værdi af et heltal bestemmes af det antal bits, der er til rådighed i hukommelsen. På 32-bit PHP er intervallet fra `-2.147.483.648` til `-2.147.483.647` (~ ± 2 milliarder), 64-bit PHP har et interval fra `-9.223.372.036.854.775.808` til `-9.223.372.036.854.775.807` (~ ± 9 quintillioner). Den maksimale værdi kan altid fås ved at kalde konstanten `PHP_INT_MAX`.

Decimaltal gemmes i datatypen **float**, som har begrænset præcision, så de gemmes internt som et par tal, der er underlagt den matematiske operation `mantisse * (2^eksponent)`. Den praktiske konsekvens er, at vi er i stand til at lagre et relativt stort antal (i størrelsesordenen milliarder) på et lille antal bits, bare ikke særlig præcist.

Konsekvensen af at bruge datatypen **float** er, at resultatet af beregningen af `1 - 0,9` ikke er nøjagtigt `0,1`.

Eksempel fra <a href="https://diskuse.jakpsatweb.cz/?action=vthread&forum=9&topic=97912">webfora</a>:

```php
$x = 0.3;
$y = 0.4;
echo 0.7 - $x - $y; // udskriver -5.5511151231258E-17
```

Hvis vi justerer tallene en smule:

```php
$x = 6.5;
$y = 7.5;
echo 14.0 - $x - $y; // udskriver 0
```

Generelt er dette spørgsmål <a href="https://vtm.zive.cz/clanky/proc-pocitacum-delaji-problemy-desetinna-cisla/sc-870-a-185672/default.aspx">omtalt i en separat artikel på VTM.e15.cz: Hvorfor computere har problemer med decimaltal</a>, eller generelt om <a href="https://cs.wikipedia.org/wiki/Pohyblivá_řádová_čárka">flydende punkt på Wikipedia</a>.

> Generelt er det en god idé at afrunde resultatet efter beregningen eller helt at undgå decimaler. Gem f.eks. prisen ikke i kroner (f.eks. 14,90), men i øre (f.eks. 1490), og arbejd derefter nøjagtigt med den. Afrunding behandles i næste afsnit af denne artikel.

Matematiske operationer
-------------------

```php
$a = 5;
$b = 3;

echo $a + $b;
```

Almindelige matematiske konventioner kan anvendes til operationer, som respekterer operatørernes forrang i henhold til matematikkens regler (multiplikation har forrang for addition osv.). Værdier kan skrives direkte eller læses via variabler.

> Det er måske ikke tydeligt ved første øjekast, men i matematikeksemplet er lighedstegnet (`=`) ikke skrevet. Det følger heraf, at kun simple `binære operationer` kan løses, som altid kun fungerer med `to elementer` (tæl ikke ligninger, det skal du bruge et specialiseret bibliotek til).

Oversigt over grundlæggende operatører
----------------------------

> I kolonnen `result` angiver jeg, hvad der er udskrevet, idet jeg antager:

```php
$a = 5;
$b = 3;
$c = 10;
```

| Operation | Markering | Markering i ord | Eksempel på skrivning | Resultat
|-------------------|-----------|---------------|-----------------------|----------
| Addition | `+` | Plus | `echo $a + $b;` | 8
| Subtraktion | `-` | Minus | `echo $a - $b;` | 2
| Multiplikation | `*` | Asterisk | `echo $a * $b;` | 15
| division | `/` | slash | `echo $a / $b;` | 1.6666666666666666667
| parentes | `( )` | parentes | `echo $a + ($b * $c);`| 35
| String concatenation | `.` | Periode | `echo $a . $b . $c;` | 5310
| Rest efter division | `%` | Procent | `echo $a % $b;` | 2
| Tilføjelse af et | `++` | to plus | `echo $a++;` | 6
| Træk én fra | `--` | to minus | `echo $a--;` | 4
| Power | `**` | to stjernetegn | `echo $a ** $b;` | 125

> Power-operatoren (dobbelt stjerne) er kun tilgængelig fra og med PHP 7.1. I andre versioner af PHP skal du bruge den universelle funktion `pow($a, $b)`.

Oversigt over grundlæggende matematiske funktioner
---------------------------------------

Hvis du skal løse en almindelig beregningsopgave, findes der højst sandsynligt allerede en funktion direkte i PHP, og her er en kommenteret oversigt over dem.

Afrunding
--------------

Normal afrunding foretages med <a href="/function-round">rund()</a>-funktionen, der har 3 parametre:

- Afrundet tal
- Valgfrit: Præcision (hvor mange decimaler)
- Valgfrit: Halvering af værdien (fra hvilken værdi skal der afrundes opad)

```php
round(5);               // 5
round(5.1);             // 5
round(5.4);             // 5
round(5.5);             // 6
round(5.8);             // 6
round(5483.47621, 2);   // 5483.47
round(5483.47621, -2);  // 5500
```

Der findes også en <a href="function-floor">floor()</a>-funktion til afrunding nedad og en <a href="/function-ceil">ceil()</a>-funktion til afrunding opad.

> **Hold øje med datatyper!**
>
> Alle afrundingsfunktioner returnerer resultatet som `float`, selv om resultatet er et heltal.
>
> > Hvis du ønsker at behandle resultatet som et heltal, er det vigtigt at omskrive resultatet bagefter. For eksempel: `echo (int) round(3.14);`

PHP arbejder med forskellige datatyper, f.eks. med **float** kan det nemt ske, at resultatet af `round()` ikke er et tal med finite decimaludvidelse. Til oplistning anbefaler jeg at bruge funktionen `number_format()`, som jeg beskriver nedenfor.

Goniometriske funktioner
--------------------

Disse bruges til mange forskellige beregninger og er baseret på enhedscirklen. De bruges f.eks. til at tegne cirkler, ellipser, forskydninger osv.

- <a href="/funktion-sin">sin()</a>
- <a href="/funktion-cos">cos()</a>
- <a href="/funktion-tan">tan()</a>

Cyklometriske funktioner
--------------------

Beregn vinklen fra `x` og `y`. Omregning af kartesiske og polære koordinater.

- <a href="/funktion-asin">asin()</a>
- <a href="/funktion-acos">acos()</a>
- <a href="/funktion-atan">atan()</a>

Hyperbolske funktioner
-------------------

Baseret på enhedshyperbolaen. Deres definitioner kan let beskrives ved hjælp af sammenligninger:

- Ellipse, cirkel, cirkel med radius 1
- Hyperbel, Ækviaxial hyperbel, Enhedshyperbel

De anvendes f.eks. til terrængenerering og fysisk simulering.

- <a href="/funktion-sinh">sinh()</a>
- <a href="/funktion-cosh">cosh()</a> (kædelinje)
- <a href="/funktion-tanh">tanh()</a>

Hyperbolometriske funktioner
------------------------

Baseret på enhedshyperbolaen.

- <a href="/funktion-asinh">asinh()</a>
- <a href="/funktion-acosh">acosh()</a>
- <a href="/funktion-atanh">atanh()</a>

Eksempler på anvendelse af goniometriske funktioner
---------------------------------------

```php
echo sin(30);            // -0.98803162409286
echo sin(deg2rad(30));   // 0.5
echo cos(deg2rad(123));  // -0.54463903501503
echo 1/tan(deg2rad(45)); // 1 (onen cotangent)
echo sin(deg2rad(500));  // 0.64278760968654
echo atan2(deg2rad(50), deg2rad(23)); // 1.1396575860761
```

Lommeregner - behandling af matematiske udtryk
--------------------------------------------

Nogle gange kan det ske, at vi har brug for at behandle et matematisk udtryk som en brugerstreng, f.eks. `5+2^(1+3/2)`.

Der er ingen nem måde at behandle sådanne input på i PHP, men jeg har programmeret en række biblioteker, der løser dette problem.

Du kan finde flere oplysninger i den separate artikel <a href="/pokrocila-calculator">Regnemaskine i PHP: Behandling af et matematisk udtryk som en streng</a>.

Operationer med store tal og nøjagtige beregninger
------------------------------------------

Store tal og decimaltal gemmes som floats i PHP, og de afrundes til ca. 15 decimaler, hvilket nogle gange kan være irriterende. Derfor blev **<a href="https://www.php.net/manual/en/book.bc.php">BCMath Arbitrary Precision Mathematics-biblioteket oprettet</a>**.

Tal repræsenteres som **strings** i dette bibliotek, så de er ikke begrænset af **float**'s upræcision, men de udførte operationer er flere størrelsesordener langsommere (men stadig hurtigere, end hvis vi selv programmerede en sådan funktion).

Hvis vi f.eks. ønsker at få en kvadratrod fra 2 til 3 decimaler, kalder vi bare:

```php
echo bcsqrt('2', 3); // 1.414
```
