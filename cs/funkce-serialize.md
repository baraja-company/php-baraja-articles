PHP funkce serialize()
================================

> id: bfb18be6-3d30-432f-a55e-66f1f771fcef
> slugCS: funkce-serialize
> publicationDate: 2019-09-11 10:04:03
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 4.0`

Generates a storable representation of a value


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$value` | `mixed` |  | The value to be serialized. serialize handles all types, except the resource-type. You can even serialize arrays that contain references to itself. Circular references inside the array/object you are serializing will also be stored. Any other reference will be lost. |


Návratové hodnoty
----------------

`string`

a string containing a byte-stream representation of
value that can be stored anywhere.

Další zdroje
------------

https://php.net/manual/en/function.serialize.php
