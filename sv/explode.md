PHP-funktionen Explode - dela upp strängar med separator
========================================================

> id: '4dc7cec6-0e96-4a6b-aee8-32c8817ba11e'
> slug:
> 	cs: explode
> 	sv: php-funktionen-explode---dela-upp-straengar-med-separator
> 
> perex:
> 	- Rozdělení řetězce na více částí podle oddělovače.
> 	- Dela upp kedjan i flera delar enligt separatorn.
> 
> publicationDate: '2019-11-26 11:39:36'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '66725772be4aae0df8af399e7ce2ba07'

Explode används för att enkelt dela upp en sträng med en separator.

- Den returnerar de enskilda resultaten som en matris numrerad från noll,
- Du kan inte infoga en array, endast strängen kan matas in,
- kan inte ändra avgränsare under analysen, kan inte välja flera avgränsare.

| Stöd | PHP 4 och senare |
|---------------|-----------------|
| Kort beskrivning | Dela en sträng till en matris med separator.
| Krav | Inga
| Obs | Du kan inte infoga en array, bara en sträng.

Ofta behöver vi dela upp en sträng enligt en enkel regel. Till exempel en kommaseparerad lista med siffror.

Funktionen explode() är utmärkt för detta, eftersom den tar separatorn (som separerar strängen) som första parameter och själva datan som andra parameter:

```php
$cisla = '3, 1, 4, 1, 5, 9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
	echo $cislo . '<br>';
	// Här kan vi bearbeta siffrorna ytterligare
}
```

Men vad händer om siffrorna är separerade med kommatecken, men det finns mellanslag runt dem?

Lösningen är att analysera den minsta gemensamma delsträngen och sedan ta bort oönskade tecken runt den (mellanslag och andra vitrymder):

```php
$cisla = '3, 1,4, 1 , 5 ,9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
     echo trim($cislo) . '<br>';
     // Här kan vi bearbeta siffrorna ytterligare
}
```

I det här fallet tar funktionen `trim()` på ett elegant sätt bort vitrymden runt tecknen (mellanslag, tabulatorer, radbrytningar, ...) och ger endast rena data.

Begränsning, begränsar antalet analyserade enheter i en matris.
--------------------------

> TIPS: För många exempel är explode() olämpligt och det är mycket bättre att använda reguljära uttryck.

Ofta vill vi dock bara analysera data upp till ett visst avstånd, och den tredje (valfria) parametern limit kan användas för detta ändamål.

Låt oss till exempel ha strukturerade data som är separerade med kolon och där vi vill hämta innehållet efter det första kolonet och ignorera de andra kolonerna.
Exempel:

```php
$cas = 'format: "j.n.Y - H:i".';
```

Om vi skulle analysera strängen endast som:

```php
$parser = explode(':', $cas);
```

Vi får dessa tre understrängar (i andra fall kan det finnas många fler):

```php
'format'
'"j.n.Y - H'
'i"'
```

Därför sätter vi en gräns för hur många element som ska hämtas (och eventuellt kommer alla andra element att läggas till i slutet av det sista elementet):

```php
$parser = explode(':', $cas, 2);

// komma tillbaka:
echo $parser[0]; // format
echo $parser[1]; // "j.n.Y - H:i"
```

> **Note:** Oönskade citationstecken kan tas bort från strängen, till exempel genom att använda funktionen `trim()`:

```php
echo trim($parser[1], '"'); // den andra parametern anger den karta med tecken som ska tas bort
```

Between, för att få en sträng mellan två strängar
--------------------------

Ofta behöver vi hämta en sträng som avgränsas av två andra strängar. Det finns ingen funktion för detta direkt i PHP, men vi kan skriva den själva:

```php
function between(string $left, string $right, string $data): string
{
   $l = explode($left, $data);
   $r = explode($right, $l[1]);

   return $r[0];
}
```

Regelbundna uttryck
--------------------------

Mycket elegantare uppdelning och arbete med strängar kan uppnås med <a href="/regex">regelbundna uttryck</a>, som jag diskuterar på en separat sida.

Liknande funktioner
--------------------------

- <a href="/funktion-implode">Implode()</a> - sammanfoga en array till en sträng

Exempel på tolkning av IP-adresser
--------------------------

```php
$ip = '10.0.0.138';
$parser = explode('.', $ip);
echo $parser[0]; // skriver ut "10"
echo $parser[1]; // skriver ut "0";
```

Variabeln `$ip` innehåller en inmatningssträng som analyseras enligt avgränsaren `.`, och resultatet är en array. Parsningen fortsätter till slutet av strängen om inte en gräns har angetts.

Parametrar
--------------------------

| # | Typ | Beskrivning
|---|--------|------|
| 1 | string | Separationssträng.
| 2 | string | Parsed string.
| 3 | int | gräns för analysering. Detta är en valfri parameter. Exempel:

```php
$text = 'PHP är mitt favoritspråk!';
$parser = explode('', $text, 1);

echo $parser[0]; // skriver ut det första ordet
echo $parser[1]; // skriver ut resten av texten
echo $parser[2]; // ger inte ut något eftersom en gräns har fastställts!
```

Returvärden
--------------------------

Returvärdet är en array med en analyserad sträng.

Indexen är numrerade från noll till `X` om inte en gräns anges.

Skillnader från tidigare versioner
--------------------------

| PHP-version | Beskrivning |
|-----------|-------|
| 5.1.0 | Stöd för negativ gräns för passeringar har lagts till.
| 4.0.1 | Parametern **limit** har lagts till som valfritt alternativ.

Tips och anteckningar
--------------------------

När du använder en negativ **gräns** anges antalet element från strängens slut.

Exempel:

```php
$str = 'ett|två|tre|fyra';

// positiv gräns
print_r(explode('|', $str, 2));

// negativ gräns (sedan PHP 5.1)
print_r(explode('|', $str, -1));
```

Följande fält kommer att returneras:

```php
Array
(
    [0] => one
    [1] => two|three|four
)

Array
(
    [0] => one
    [1] => two
    [2] => three
)
```
