Lokala variabler i PHP
======================

> id: '9c3fbf4e-8612-4599-8127-d66b9ec5e980'
> slug:
> 	cs: lokalni-promenna
> 	sv: lokala-variabler-i-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: cffee22c4dbcc2ec331b73235349c409

Lokala variabler är endast giltiga inom kroppen av **<a href="/prikazy-a-function">funktion</a>** eller **metod** (i objektorienterad programmering).

Om vi arbetar i ett vanligt skript händer allting som förväntat:

```php
$x = 5;

echo $x; // skriver ut 5
```

Men när vi definierar en egen funktion ändras beteendet något:

```php
$x = 5;

function mojeFunkce(): int
{
    $x = 3;

    echo $x; // skriver ut 3
}

echo $x;     // skriver ut 5
```

Anledningen är att variabeln $x endast existerar inom ramen för den aktuella funktionen eller metoden. Detta beteende är avsiktligt.

Om vi vill skicka ett värde från den omgivande koden till en funktion måste vi anropa den med de nödvändiga parametrarna:

```php
echo mojeFunkce(5);	// skriver ut 6

function mojeFunkce(int $x): int
{
    return $x + 1;
}
```

Värden anges i funktioner med hjälp av parametrar. Om du vill skicka ytterligare variabler till funktionen utöver parametrarna kan du använda <a href="/global-variable">globala variabler</a>, men det är ingen bra idé.

> Användningen av lokala variabler gör stor skillnad när man programmerar ett större program. Om vi inte skiljer på variablers giltighet i olika sammanhang skulle det vara omöjligt att garantera att en variabel inte åsidosätts på ett ställe där vi inte räknar med den (vilket är anledningen till att globala variabler är onda).

```php
$x = 5;
$y = 3;

function soucet(int $x, int $y): int
{
    return $x + $y;
}

echo $x;             // skriver ut 5
echo soucet($x, $y); // skriver ut 8
```
