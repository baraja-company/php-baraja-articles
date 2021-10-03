PHP funkce addcslashes()
========================

> id: caa941b6-05e0-426f-97b7-f969c7874638
> slug:
> 	cs: funkce-addcslashes
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Quote string with slashes in a C style


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$str` | `string` |  | The string to be escaped. |
| `$charlist` | `string` |  | A list of characters to be escaped. If charlist contains characters \n, \r etc., they are converted in C-like style, while other non-alphanumeric characters with ASCII codes lower than 32 and higher than 126 converted to octal representation. |


Návratové hodnoty
----------------

`string`

the escaped string.

Další zdroje
------------

https://www.php.net/manual/en/function.addcslashes.php
