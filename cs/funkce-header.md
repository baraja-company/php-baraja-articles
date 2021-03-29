PHP funkce header()
===================

> id: cf7f8ad4-5038-4519-bd1c-712028a494a5
> slugCS: funkce-header
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Odešle HTTP hlavičku a surové (raw) podobě.

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$string` | `string` |  | Obsah hlavičky |
| `$replace` | `bool` | true, | The optional replace parameter indicates whether the header should replace a previous similar header, or add a second header of the same type. By default it will replace, but if you pass in false as the second argument you can force multiple headers of the same type. For example: |
| `$http_response_code` | `int` | null | Forces the HTTP response code to the specified value. |


Návratové hodnoty
----------------

Nevrací nic.


Další zdroje
------------

https://php.net/manual/en/function.header.php
