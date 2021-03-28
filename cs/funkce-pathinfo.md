PHP funkce pathinfo()
================================

> id: b3827a32-edc1-4057-8212-4d550b7a2fda
> slugCS: funkce-pathinfo
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0.3`

Returns information about a file path


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$path` | `string` |  | The path being checked. |
| `$options` | `int` | null | You can specify which elements are returned with optional parameter options. It composes from PATHINFO_DIRNAME, PATHINFO_BASENAME, PATHINFO_EXTENSION and PATHINFO_FILENAME. It defaults to return all elements. |


Návratové hodnoty
----------------

`mixed`

The following associative array elements are returned:
dirname, basename,
extension (if any), and filename.
</p>
<p>
If options is used, this function will return a
string if not all elements are requested.

Další zdroje
------------

https://php.net/manual/en/function.pathinfo.php
