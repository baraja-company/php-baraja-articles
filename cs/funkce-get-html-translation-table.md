PHP funkce get_html_translation_table()
=======================================

> id: b1c948c8-9a7c-4359-ab15-446b76c37293
> slugCS: funkce-get-html-translation-table
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Returns the translation table used by <function>htmlspecialchars</function> and <function>htmlentities</function>


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$table` | `int` | null, | There are two new constants (HTML_ENTITIES, HTML_SPECIALCHARS) that allow you to specify the table you want. |
| `$quote_style` | `int` | null | Like the htmlspecialchars and htmlentities functions you can optionally specify the quote_style you are working with. See the description of these modes in htmlspecialchars. |


Návratové hodnoty
----------------

`array`

the translation table as an array.

Další zdroje
------------

https://php.net/manual/en/function.get-html-translation-table.php
