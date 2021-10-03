PHP funkce abs()
================

> id: "98593615-ad35-4ae0-bbb3-81156a1a50de"
> slug:
> 	cs: funkce-abs
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`, `PHP 5`, `PHP 7`

Absolutní hodnota čísla

Parametry
---------

| Parametr  | Datový typ | Výchozí hodnota | Poznámka |
|-----------|------------|-----------------|----------|
| `$number` | `mixed`    | *není*          | Numerická hodnota pro převod |

```php
$cislo = -15.4;
$abs = abs($cislo); // 15.4
```

Vrátí absolutní hodnotu čísla.

Návratové hodnoty
----------------

`číslo` (int, float, ...)

- Vrátí absolutní hodnotu čísla (tedy bez znaménka).
- Pokud je vstup float, vrátí také float.
- Pokud je vstup integer, vrátí integer.
- Pokud je vstup integer větší než obsah integeru, bude přetypován na float.

```php
abs(5);     // 5
abs(-3);    // 3
abs(-3.14); // 3.14
```

Příklad
-------

```php
$abs = abs(-4.2); // $abs = 4.2; (double/float)
$abs2 = abs(5); // $abs2 = 5; (integer)
$abs3 = abs(-5); // $abs3 = 5; (integer)
```

V komentáři jsou uvedené návratové hodnoty

Další zdroje
------------

[PHP.net dokumentace](https://www.php.net/manual/en/function.abs.php)
