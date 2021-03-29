PHP funkce unserialize()
========================

> id: "2a898eba-4e92-4d6b-ac54-618ff790cde7"
> slugCS: funkce-unserialize
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`, `PHP 7.0`

Creates a PHP value from a stored representation


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$str` | `string` |  | The serialized string. |
| `$options` | `mixed` | null |  |


Návratové hodnoty
----------------

`mixed`

The converted value is returned, and can be a boolean,
integer, float, string,
array or object.
</p>
<p>
In case the passed string is not unserializeable, false is returned and
E_NOTICE is issued.

Další zdroje
------------

https://php.net/manual/en/function.unserialize.php
