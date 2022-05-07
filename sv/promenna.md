Variabler i PHP
===============

> id: b89774e7-143c-4a8c-8dc6-3b3d2c78d5b7
> slug:
> 	cs: promenna
> 	sv: variabler-i-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '23e4d537f18a3b2e1946c79be1aacf52'

Den här sidan är en fullständig sammanfattning av hur variabler fungerar i PHP. Texten är skriven i en något teknisk stil och är kanske inte helt begriplig för nybörjare. Om du är intresserad av de fullständiga grunderna kan du läsa <a href="/first-script">begynderhandledning</a> och <a href="/principles-of-prominent-script">principer för att skriva variabler</a>.

Beskrivning
-----

En variabel är en virtuell plats i driftsminnet, den definieras av `name` och `data type`. Inom datatypen har variabeln sedan ett visst "innehåll".

Internt representerar PHP variabler som en så kallad hashtabell, dvs. alla variabler lagras i en tabell som är mycket snabb att söka i, så tiden som krävs för att komma åt varje variabel är *nästan* konstant.

Exempel på skrivande:

```php
$a = 10;
$b = 'cat';
$c = true;
```

Varje rad i urvalet anger definitionen av en variabel. Namnet på varje variabel börjar med dollartecknet `$` följt av själva namnet. Ekvivalentecknet kan användas för att tilldela ett värde till variabeln.

Internt lagras variablerna i minnet som en hashtabell:

| Namn | Typ | Förkortning | Värde |
|-------|---------|---------|---------|
| `$a` | heltal | int | `10` |
| `$b` | sträng | sträng | sträng | `cats` |
| `$c` | boolean | bool | `true` |

Variabeltyper
---------------

Variablerna klassificeras enligt åtkomsträttigheter och användning:

- <a href="/lokal-variabel">lokala variabler</a> (giltiga inom ett sammanhang, dvs. inom funktioner och metoder),
- <a href="/global-variabel">globala variabler</a> (gäller i hela skriptet),
- <a href="/superglobal-variabel">Superglobala variabler</a> (gäller i hela servermiljön - vanligtvis användardata),
- <a href="/promenna-variabel">variabelvariabler</a> (en särskild variabel som innehåller en referens (länk) till en annan variabel).

* "Globala variabler" och "variabla variabler" bör inte användas eftersom de bidrar till oläsbarhet i koden och "magiska" (oväntade) programbeteenden.*

Tillåtet innehåll i variabler
--------------------------

Variabler kan innehålla allt som deras aktuella datatyp tillåter. Om datatypen inte anges bestäms den automatiskt utifrån innehållet (detta rekommenderas inte eftersom det kan leda till fel).

Datatyper fungerar oberoende av varandra, så vi kan använda nästan vilken typ som helst. Om vi vill utföra någon sammanslagning måste vi dock alltid se till att konverteringen sker till ett format.

Ett bra exempel är addition och multiplikation av tal:

```php
$x = 5;       // heltal
$y = 3;       // heltal
$z = $y + $y; // variabeln $z kommer att komponeras utifrån flera variabler
```

I det här fallet ställs PHP inför frågan om vilken datatyp den nyskapade variabeln `$z` ska ha. Om de är av samma datatyp och om operationen är möjlig ärvs datatypen.

Ibland kan vi dock utföra en operation med flera datatyper:

```php
$x = 1;       // heltal
$y = 3.14159; // float
$z = $y + $y; // float
```

I det här fallet slår vi ihop heltal och float. Utgången blir ett decimaltal, så float används. I det här fallet gör PHP något som kallas **dynamisk omfördelning**.

Vi kan dock inte alltid lita på detta beteende. Hur skulle du till exempel vilja slå ihop ett nummer och en sträng?

```php
$x = 256;     // heltal
$y = 'Hallå! Hallå!'; // float
$z = $y + $y; // ???
```

Datatyper (översikt över de viktigaste)
--------------------------------------

PHP är ett tolkat språk, så det har vissa särdrag jämfört med andra programmeringsspråk, en av dem är att vi inte behöver ange datatyper för variabler, dvs. varje variabel ändrar dynamiskt sin datatyp beroende på dess innehåll (om inget annat anges). Det är ändå bra att känna till åtminstone de grundläggande datatyperna, särskilt när du optimerar program eller arbetar med databaser.

Notationen kan se ut så här:

```php
$x = (int) 25; // skapar en variabel av heltalstyp
```

<a href="/datove-typy">Översikt över datatyper</a>.

Arv av datatyper
-----------------------

Vilken datatyp kommer variabeln `$x` att ha om vi bara känner till den här delen av källkoden?

```php
$x = $y;
```

Det beror på datatypen för variabeln `$y` från vilken både värdet och dess datatyp ärvs. I det här fallet känner vi inte till variabeln `$x`, så vi kan inte fortsätta att utvärdera koden och ett felmeddelande skulle visas.

Dynamiskt åsidosättande
---------------------

Vi har följande två variabler:

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

Representera strängar som matriser
------------------------------

Alla strängar lagras internt som en array av tecken. Det vill säga, varje tecken har sitt eget **index** och kan refereras. Om vi inte anger något index används hela strängen.

```php
$x = 'Låt oss programmera i PHP!';
$n = 3;

echo $x;		// Detta skriver ut hela innehållet i variabeln $x.
echo $x[0];		// Detta skriver ut nolltecknet i variabeln $x.
echo $x[$n];	// Detta skriver ut det nionde tecknet i variabeln $x.
```

> **Note:** PHP-nummer från noll, dvs. nolltecken är "P" och det första tecknet är "r".
>
> > Dessutom är tecken byte-växlade. Tecknet "no" i UTF-8-kodning är till exempel 2 bytes långt, så teckenindexet i strängen kommer inte att motsvara den verkliga positionen vid rullning och 2 index kommer att användas för att lagra tecknet.

Förekomsten av ett arrayelement bör alltid verifieras med funktionen `isset()`:

```php
if (isset($x[$n])) {
    echo $x[$n];
}
```

Alternativt kan du skriva det fint med en ternär operatör:

```php
echo $x[$n] ?? '';
```

Kopiering av variabler
---------------------

Vi har följande variabel:

```php
$q = 'Lorem ipsum, ...';
```

Kopiera sedan dess värde till nästa variabel:

```php
$qi = $q;
```

Lyckligtvis sker ingen kopiering och PHP lagrar bara en referens till värdet i en **hashtabell**. Värdet kopieras endast när värdet på en av variablerna ändras. Detta beteende hanteras av en komponent som vanligtvis kallas **Garbridge collector**.
