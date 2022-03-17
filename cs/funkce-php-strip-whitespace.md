PHP funkce php_strip_whitespace()
=================================

> id: "8420ca89-7498-43c2-a4fa-8a7f6aa093cf"
> slug:
> 	cs: funkce-php-strip-whitespace
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Return source with stripped comments and whitespace


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` | *není* | Path to the PHP file. |


Návratové hodnoty
----------------

`string`

The stripped source code will be returned on success, or an empty string
on failure.
</p>
<p>
This function works as described as of PHP 5.0.1. Before this it would
only return an empty string. For more information on this bug and its
prior behavior, see bug report
#29606.

Další zdroje
------------

[Oficiální dokumentace funkce php-strip-whitespace](https://www.php.net/manual/en/function.php-strip-whitespace.php)
