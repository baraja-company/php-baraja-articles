Lokální proměnné v PHP
======================

> id: "9c3fbf4e-8612-4599-8127-d66b9ec5e980"
> slug:
> 	cs: lokalni-promenna
> 
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "0e39aee9-2818-480c-8081-e0c2d039bb24"

Lokální proměnné mají platnost jen uvnitř těla **<a href="/prikazy-a-funkce">funkce</a>** nebo **metody** (v objektovém programování).

Pokud pracujeme v kontextu běžného scriptu, tak se vše děje dle očekávání:

```php
$x = 5;

echo $x; // vypíše 5
```

Když ale definujeme vlastní funkci, tak se chování lehce změní:

```php
$x = 5;

function mojeFunkce(): int
{
    $x = 3;

    echo $x; // vypíše 3
}

echo $x;     // vypíše 5
```


Důvod je ten, že proměnná $x existuje jen v kontextu aktuální funkce nebo metody. Toto chování je záměrné.

Pokud chceme do funkce předat hodnotu z okolního kódu, tak ji musíme zavolat s potřebnými parametry:

```php
echo mojeFunkce(5);	// vypíše 6

function mojeFunkce(int $x): int
{
    return $x + 1;
}
```


Hodnoty se do funkcí dostávají za pomoci parametrů. Pokud chcete do funkce přenést další proměnné nad rámec parametrů, můžete použít <a href="/globalni-promenna">globální proměnné</a>, ale není to dobrý nápad.

> Použití lokálních proměnných má obrovský význam při programování větší aplikace. Pokud bychom nerozlišovali platnosti proměnných v rámci různých kontextů, tak by nebylo možné zaručit, že se proměnná nepřepíše na místě, kde s tím nepočítáme (proto jsou globální proměnné zlo).

```php
$x = 5;
$y = 3;

function soucet(int $x, int $y): int
{
    return $x + $y;
}

echo $x;             // vypíše 5
echo soucet($x, $y); // vypíše 8
```
