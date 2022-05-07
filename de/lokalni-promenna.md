Lokale Variablen in PHP
=======================

> id: '9c3fbf4e-8612-4599-8127-d66b9ec5e980'
> slug:
> 	cs: lokalni-promenna
> 	de: lokale-variablen-in-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: cffee22c4dbcc2ec331b73235349c409

Lokale Variablen sind nur innerhalb des Körpers einer **<a href="/prikazy-a-function">Funktion</a>** oder **Methode** (in der objektorientierten Programmierung) gültig.

Wenn wir im Kontext eines regulären Skripts arbeiten, geschieht alles wie erwartet:

```php
$x = 5;

echo $x; // druckt 5
```

Wenn wir jedoch eine benutzerdefinierte Funktion definieren, ändert sich das Verhalten leicht:

```php
$x = 5;

function mojeFunkce(): int
{
    $x = 3;

    echo $x; // druckt 3
}

echo $x;     // druckt 5
```

Der Grund dafür ist, dass die Variable $x nur im Kontext der aktuellen Funktion oder Methode existiert. Dieses Verhalten ist beabsichtigt.

Wenn wir einen Wert aus dem umgebenden Code an eine Funktion übergeben wollen, müssen wir sie mit den erforderlichen Parametern aufrufen:

```php
echo mojeFunkce(5);	// druckt 6

function mojeFunkce(int $x): int
{
    return $x + 1;
}
```

In Funktionen werden Werte über Parameter eingegeben. Wenn Sie über die Parameter hinaus weitere Variablen an die Funktion übergeben wollen, können Sie <a href="/global-variable">globale Variablen</a> verwenden, aber das ist keine gute Idee.

> Die Verwendung lokaler Variablen macht einen großen Unterschied bei der Programmierung einer größeren Anwendung. Wenn wir nicht zwischen der Gültigkeit von Variablen in verschiedenen Kontexten unterscheiden würden, wäre es unmöglich zu garantieren, dass eine Variable nicht an einer Stelle überschrieben wird, an der wir nicht mit ihr rechnen (deshalb sind globale Variablen böse).

```php
$x = 5;
$y = 3;

function soucet(int $x, int $y): int
{
    return $x + $y;
}

echo $x;             // druckt 5
echo soucet($x, $y); // druckt 8
```
