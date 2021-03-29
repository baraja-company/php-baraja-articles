PHP funkce realpath()
=====================

> id: "28a7eaf9-5c2e-4933-b701-5637ea0857ab"
> slugCS: funkce-realpath
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Returns canonicalized absolute pathname


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$path` | `string` |  | The path being checked. |


Návratové hodnoty
----------------

`string`

|bool the canonicalized absolute pathname on success. The resulting path
will have no symbolic link, '/./' or '/../' components.
</p>
<p>
realpath returns false on failure, e.g. if
the file does not exist.

Další zdroje
------------

https://php.net/manual/en/function.realpath.php
