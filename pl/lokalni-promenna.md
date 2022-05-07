Zmienne lokalne w PHP
=====================

> id: '9c3fbf4e-8612-4599-8127-d66b9ec5e980'
> slug:
> 	cs: lokalni-promenna
> 	pl: zmienne-lokalne-w-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: cffee22c4dbcc2ec331b73235349c409

Zmienne lokalne są ważne tylko wewnątrz ciała **<a href="/prikazy-a-function">funkcji</a>** lub **metody** (w programowaniu obiektowym).

Jeśli pracujemy w kontekście zwykłego skryptu, wszystko przebiega zgodnie z oczekiwaniami:

```php
$x = 5;

echo $x; // drukuje 5
```

Jeśli jednak zdefiniujemy funkcję niestandardową, zachowanie to nieco się zmienia:

```php
$x = 5;

function mojeFunkce(): int
{
    $x = 3;

    echo $x; // drukuje 3
}

echo $x;     // drukuje 5
```

Wynika to z faktu, że zmienna $x istnieje tylko w kontekście bieżącej funkcji lub metody. Takie zachowanie jest celowe.

Jeśli chcemy przekazać wartość z otaczającego kodu do funkcji, musimy ją wywołać z odpowiednimi parametrami:

```php
echo mojeFunkce(5);	// drukuje 6

function mojeFunkce(int $x): int
{
    return $x + 1;
}
```

Wartości są wprowadzane do funkcji za pomocą parametrów. Jeśli chcesz przekazać do funkcji dodatkowe zmienne poza parametrami, możesz użyć <a href="/global-variable">zmiennych globalnych</a>, ale nie jest to dobry pomysł.

> Używanie zmiennych lokalnych robi ogromną różnicę podczas programowania większych aplikacji. Gdybyśmy nie rozróżniali ważności zmiennych w różnych kontekstach, nie dałoby się zagwarantować, że zmienna nie zostanie nadpisana w miejscu, w którym na nią nie liczymy (dlatego zmienne globalne są złe).

```php
$x = 5;
$y = 3;

function soucet(int $x, int $y): int
{
    return $x + $y;
}

echo $x;             // drukuje 5
echo soucet($x, $y); // drukuje 8
```
