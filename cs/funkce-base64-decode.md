PHP funkce base64_decode()
==========================

> id: a587925c-9696-42d8-93e0-e9db64747a6b
> slugCS: funkce-base64-decode
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Decodes data encoded with MIME base64


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$data` | `string` |  | The encoded data. |
| `$strict` | `bool` | null | Returns false if input contains character from outside the base64 alphabet. |


Návratové hodnoty
----------------

`string`

|bool the original data or false on failure. The returned data may be
binary.

Další zdroje
------------

https://php.net/manual/en/function.base64-decode.php
