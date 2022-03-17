PHP funkce wordwrap()
=====================

> id: "2f2222d8-193f-422f-8d46-9f1c485abb12"
> slug:
> 	cs: funkce-wordwrap
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0.2`

Obalí řetězec na zadaný počet znaků


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$str` | `string` | *není* | Zpracovávaný řetězec. |
| `$width` | `int` | 75, | Šířka sloupce. |
| `$break` | `string` | "\n", | Řádek se zalomí pomocí nepovinného parametru break. |
| `$cut` | `bool` | false | Pokud je parametr cut nastaven na hodnotu true, řetězec se vždy zabalí na zadané šířce nebo před ní. Pokud je tedy slovo větší než zadaná šířka, je rozděleno. (Viz druhý příklad). |


Návratové hodnoty
----------------

`string`

daný řetězec zabalený v zadaném sloupci.

Další zdroje
------------

[Oficiální dokumentace funkce wordwrap](https://www.php.net/manual/en/function.wordwrap.php)
