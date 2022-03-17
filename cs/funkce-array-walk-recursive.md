PHP funkce array_walk_recursive()
=================================

> id: e71c1de4-4333-4d8d-9160-8c0d30481f46
> slug:
> 	cs: funkce-array-walk-recursive
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "59874540-664b-4474-9869-7e6742ab6051"

Dostupnost ve verzích: `PHP 5.0`

Apply a user function recursively to every member of an array


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$input` | `array` | *není* | The input array. |
| `$funcname` | `callback` | *není* | Typically, funcname takes on two parameters. The input parameter's value being the first, and the key/index second. |
| `$userdata` | `mixed` | null | If the optional userdata parameter is supplied, it will be passed as the third parameter to the callback funcname. |


Návratové hodnoty
----------------

`bool`

true on success or false on failure.

Další zdroje
------------

https://www.php.net/manual/en/function.array-walk-recursive.php
