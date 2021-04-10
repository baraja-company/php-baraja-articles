PHP funkce import_request_variables()
=====================================

> id: ac7effde-4411-49c3-b93b-3ab04b0fbc2a
> slug:
> 	cs: funkce-import-request-variables
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.1.0`

Import GET/POST/Cookie variables into the global scope


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$types` | `string` |  | Using the types parameter, you can specify which request variables to import. You can use 'G', 'P' and 'C' characters respectively for GET, POST and Cookie. These characters are not case sensitive, so you can also use any combination of 'g', 'p' and 'c'. POST includes the POST uploaded file information. |
| `$prefix` | `string` | null | Variable name prefix, prepended before all variable's name imported into the global scope. So if you have a GET value named "userid", and provide a prefix "pref_", then you'll get a global variable named $pref_userid. |


Návratové hodnoty
----------------

`bool`

true on success or false on failure.

Další zdroje
------------

https://php.net/manual/en/function.import-request-variables.php
