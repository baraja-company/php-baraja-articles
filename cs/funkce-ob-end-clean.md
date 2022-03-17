PHP funkce ob_end_clean()
=========================

> id: "5acd88fb-ec0d-4122-bbcd-38377e3b9394"
> slug:
> 	cs: funkce-ob-end-clean
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Clean (erase) the output buffer and turn off output buffering


Parametry
--------------

Funkce nemá žádné vstupní parametry.

Návratové hodnoty
----------------

`bool`

vrátí `true` v případě úspěchu, jinak `false` v případě neúspěchu. Reasons for failure are first that you called the
function without an active buffer or that for some reason a buffer could
not be deleted (possible for special buffer).

Další zdroje
------------

[Oficiální dokumentace funkce ob-end-clean](https://www.php.net/manual/en/function.ob-end-clean.php)
