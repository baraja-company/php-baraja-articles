PHP funkce phpversion()
================================

> id: 9f5d1018-f604-453c-91f3-062e8d9c32b3
> slugCS: funkce-phpversion
> publicationDate: 2019-09-11 10:04:04
> mainCategoryId: 0eeab3a7-a54b-46db-a253-ca6100145648

Dostupnost ve verzích: `PHP 4.0`

Gets the current PHP version


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$extension` | `string` | null | An optional extension name. |


Návratové hodnoty
----------------

`string`

If the optional extension parameter is
specified, phpversion returns the version of that
extension, or false if there is no version information associated or
the extension isn't enabled.

Další zdroje
------------

https://php.net/manual/en/function.phpversion.php
