PHP funkce stream_context_create()
================================

> id: 813becd1-4e37-4294-8481-c40d23cf40fb
> slugCS: funkce-stream-context-create
> publicationDate: 2019-09-11 10:04:03
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 4.3.0`

Create a streams context


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$options` | `array` | null, | Must be an associative array of associative arrays in the format $arr['wrapper']['option'] = $value. |
| `$params` | `array` | null | Must be an associative array in the format $arr['parameter'] = $value. Refer to context parameters for a listing of standard stream parameters. |


Návratové hodnoty
----------------

`resource`

A stream context resource.

Další zdroje
------------

https://php.net/manual/en/function.stream-context-create.php
