PHP funkce array_walk()
=======================

> id: "9a37b2cc-6123-404b-ad12-8c430cbf1f14"
> slug:
> 	cs: funkce-array-walk
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "59874540-664b-4474-9869-7e6742ab6051"

Dostupnost ve verzích: `PHP 4.0`

Apply a user function to every member of an array


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$array` | `array` | *není* | The input array. |
| `$funcname` | `callback` | *není* | Typically, funcname takes on two parameters. The array parameter's value being the first, and the key/index second. |
| `$userdata` | `mixed` | null | If the optional userdata parameter is supplied, it will be passed as the third parameter to the callback funcname. |


Návratové hodnoty
----------------

`bool`

vrátí `true` v případě úspěchu, jinak `false` v případě neúspěchu.

Další zdroje
------------

[Oficiální dokumentace funkce array-walk](https://www.php.net/manual/en/function.array-walk.php)
