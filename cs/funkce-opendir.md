PHP funkce opendir()
====================

> id: "9f9a1934-6b93-4105-ad55-59ab43dc74df"
> slug:
> 	cs: funkce-opendir
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Open directory handle


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$path` | `string` |  | The directory path that is to be opened |
| `$context` | `resource` | null | For a description of the context parameter, refer to the streams section of the manual. |


Návratové hodnoty
----------------

`resource`

|bool a directory handle resource on success, or
false on failure.
</p>
<p>
If path is not a valid directory or the
directory can not be opened due to permission restrictions or
filesystem errors, opendir returns false and
generates a PHP error of level
E_WARNING. You can suppress the error output of
opendir by prepending
'@' to the
front of the function name.

Další zdroje
------------

https://www.php.net/manual/en/function.opendir.php
