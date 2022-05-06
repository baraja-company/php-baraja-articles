PHP function abs()
==================

> id: '98593615-ad35-4ae0-bbb3-81156a1a50de'
> slug:
> 	cs: funkce-abs
> 	en: php-function-abs
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '9fb426345146517821c15b22913d553c'

Available in `PHP 4.0`, `PHP 5`, `PHP 7`

Absolute value of the number

Parameters
---------

| Parameter | Data type | Default value | Note |
|-----------|------------|-----------------|----------|
| `$number` | `mixed` | *not* | Numeric conversion value |

```php
$cislo = -15.4;
$abs = abs($cislo); // 15.4
```

Returns the absolute value of the number.

Return values
----------------

`number` (int, float, ...)

- Returns the absolute value of a number (i.e. unsigned).
- If the input is a float, it will also return a float.
- If the input is an integer, it returns an integer.
- If the input is integer greater than the contents of integer, it will be mapped to float.

```php
abs(5); // 5
abs(-3); // 3
abs(-3.14); // 3.14
```

Example
-------

```php
$abs = abs(-4.2); // $abs = 4.2; (double/float)
$abs2 = abs(5); // $abs2 = 5; (integer)
$abs3 = abs(-5); // $abs3 = 5; (integer)
```

The return values are listed in the comment

Other sources
------------

[Official abs function documentation]([PHP.net documentation](https://www.php.net/manual/en/function.abs.php))
