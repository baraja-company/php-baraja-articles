PHP funkce acos()
=================

> id: "94dfdeaa-3a05-43ce-aad3-e973c9cc4000"
> slug:
> 	cs: funkce-acos
> 
> publicationDate: "2020-02-16 16:41:10"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Arkus kosínus (arcus cosinus).

Funkce vrátí arcus cosinus parametru `$arg` v radiánech. `acos()` je inverzní funkce k funkci `cos()`, což znamená, že `$a === cos(acos($a))` (pozor na zaokrouhlení!) pro každou hodnotu z definičního oboru funkce `acos()`.

Parametry
---------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|----------|------------|-----|-----|
| `$arg`   | `float`    | *není* | Argument ke zpracování. |


Návratové hodnoty
----------------

`float`

Vrátí arcus cosinus argumentu v radiánech.

Další zdroje
------------

- <a href="/funkce-acosh">Funkce `acosh()`</a>
- [Oficiální dokumentace k acos()](https://php.net/manual/en/function.acos.php)
- <a href="/matematika">Jak funguje matematika v PHP</a>
