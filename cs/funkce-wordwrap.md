PHP funkce wordwrap()
=====================

> id: "2f2222d8-193f-422f-8d46-9f1c485abb12"
> slug:
> 	cs: funkce-wordwrap
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0.2`

Wraps a string to a given number of characters


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$str` | `string` | *není* | Zpracovávaný řetězec. |
| `$width` | `int` | 75, | The column width. |
| `$break` | `string` | "\n", | The line is broken using the optional break parameter. |
| `$cut` | `bool` | false | If the cut is set to true, the string is always wrapped at or before the specified width. So if you have a word that is larger than the given width, it is broken apart. (See second example). |


Návratové hodnoty
----------------

`string`

the given string wrapped at the specified column.

Další zdroje
------------

[Oficiální dokumentace funkce wordwrap](https://www.php.net/manual/en/function.wordwrap.php)
