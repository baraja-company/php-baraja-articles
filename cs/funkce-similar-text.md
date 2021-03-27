PHP funkce similar_text()
================================

> id: 9eed8734-b132-4203-9ccb-c09889d0c023
> slugCS: funkce-similar-text
> publicationDate: 2019-09-11 10:04:03
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 4.0`

Calculate the similarity between two strings


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$first` | `string` |  | The first string. |
| `$second` | `string` |  | The second string. |
| `$percent` | `float` | null | By passing a reference as third argument, similar_text will calculate the similarity in percent for you. |


Návratové hodnoty
----------------

`int`

the number of matching chars in both strings.

Další zdroje
------------

https://php.net/manual/en/function.similar-text.php
