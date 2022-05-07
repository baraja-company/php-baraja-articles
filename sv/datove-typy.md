Datatyper i PHP
===============

> id: cd63818c-259a-484e-b006-86c35b362019
> slug:
> 	cs: datove-typy
> 	sv: datatyper-i-php
> 
> perex:
> 	- 'Datové typy v PHP: String, integer, float, boolean, pole (array), objekt a další.'
> 	- 'Datatyper i PHP: sträng, heltal, float, boolean, array, objekt med mera.'
> 
> publicationDate: '2019-08-23 15:44:28'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: fcb2c647973c5f94e601c789b0a882f1

Alla uppgifter som behandlas i PHP är av en viss typ. Till exempel ett heltal, en sträng eller en boolean (sant/falskt).

Grundläggande datatyper
--------------------

Grundläggande typer kallas också **primitiva datatyper** eller <a href="/function-is-scalar">skalartyper</a>.

| Typ | Namn | Beskrivning |
|---------|-----------------|-------|
| `int` | Integer | (`integer`) Innehåller endast heltal. Det maximala värdet bestäms av antalet bitar. För 32-bitars PHP är intervallet från `-2 147 483 648` till `-2 147 483 647` (~ ± 2 miljarder), 64-bitars PHP har ett intervall från `-9 223 372 036 854 775 808` till `-9 223 372 036 854 775 807` (~ ± 9 kvintiljoner). Det maximala värdet kan alltid erhållas genom att anropa konstanten `PHP_INT_MAX`. Om det maximala värdet för ett heltal överskrids kommer PHP att representera talet som `float` och automatiskt skriva över det.
| `float` | Floating Point Decimal | Detta är en variant av floating point-tal för vilken regeln *"ju mindre desto mer exakt "* gäller. Talet lagras internt som en så kallad **mantissa** och **exponent**, så du lagrar faktiskt två tal mellan vilka operationen `mantissa * (2^exponent)` utförs, vilket gör det möjligt att lagra ett riktigt stort antal tal. Detta bygger på principen att för stora tal behöver vi inte alltid känna till deras exakta värde, men vi vill spara så mycket minne som möjligt. Tal av typen `float` behöver inte lagras exakt och bör inte användas för att beräkna pengar.
| `string` | String | Innehåller en sekvens av tecken som avgränsas av citationstecken eller apostrofer. Den maximala längden begränsas endast av driftsminnets kapacitet. En sträng kan lagras i vilken kodning som helst, innehålla emoji eller binära data.
| `bool` | Boolean | (`boolean`) Boolskt värde från boolsk algebra, kan bara innehålla `true` eller `false`.
| `null` | Tomt värde | Det tomma värdet `null` är användbart när vi vill uttrycka att något inte existerar. En artikel har till exempel ingen kategori. Ibland ersätts `null` felaktigt med null (`0`) eller tomma strängar (`''`), men detta är ingen bra lösning för att uttrycka icke-existens.

**Varning:** Typen `null` är inte skalär.

Noll (0) mot noll
----------------

Många utvecklare har svårt att förstå skillnaden mellan `0` (noll) och `null` (ett obefintligt värde) när de börjar utveckla.

Denna skillnad kan förklaras mycket bra och humoristiskt med följande bild:

<img src="{$baseUrl}/images/0-vs-null.jpg" alt="0 vs. null" class="w-100 mb-3">

Manuell ompackning
--------------------

Vissa typer kan konverteras mellan varandra. Måldatatypen anges med runda parenteser och kan anges var som helst i programmet, till exempel när du listar ett värde.

Till exempel:

```php
$pi = 3.14;

echo $pi;       // ger 3.14

echo (int) $pi; // skriver ut 3
```

Dynamisk påfyllning
---------------------

Betrakta följande två variabler:

```php
$x = 10;
$y = '10';
```

Vad är skillnaden mellan innehållet i variablerna `$x` och `$y`?

Variabeln `$x` är ett tal, `$y` är en sträng (som innehåller en "1" och en "0"), så om vi bara lagrar variabeln i minnet och inte utför någon operation kommer värdet att påverkas. Till exempel ger följande två poster samma resultat:

```php
echo $x + 5;	// skriver ut 15
echo $y + 5;	// skriver ut 15
```

I det andra fallet sker en så kallad **dynamisk överskrivning**, dvs. variabeln ändrar sin datatyp så att en beräkningsoperation kan utföras på den. Detta beteende kan man inte alltid lita på, utan är mer ett korrigerande beteende för att rätta till dåligt skrivna nybörjarskript. Om möjligt ska du alltid skriva tal med en datatyp för att lagra dem, eftersom det ökar deras precision och gör dem lättare att använda i framtiden.

> **Note:** Det är viktigt att notera att vi inte kan konvertera datatyper helt godtyckligt, så detta är inte alltid möjligt. Om du skriver över en datatyp till en annan (inkompatibel) datatyp kan det hända att konverteringen inte sker alls, eller att det ursprungliga innehållet skadas eller förstörs helt och hållet och ersätts med en annan datatyp. Om du till exempel skriver om en sträng till ett heltal (och en text som inte är ett tal lagras i variabeln) kommer värdet "1" att lagras i variabeln i stället för det numeriska värdet.

Jämförelse av typer
----------------

När du jämför värden måste du ta hänsyn till olika datatyper.

I allmänhet används operatören `==` för allmän jämförelse av två värden (oavsett typ), medan `===` jämför både värden och datatyper.

Till exempel:

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
