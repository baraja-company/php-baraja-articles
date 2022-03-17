PHP funkce current()
====================

> id: b34059cd-9c40-4ef1-81e0-d21339137430
> slug:
> 	cs: funkce-current
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Return the current element in an array


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$array` | `array` | *není* | The array. |


Návratové hodnoty
----------------

`mixed`

The current function simply returns the
value of the array element that's currently being pointed to by the
internal pointer. It does not move the pointer in any way. If the
internal pointer points beyond the end of the elements list or the array is
empty, current returns false.

Další zdroje
------------

[Oficiální dokumentace funkce current](https://www.php.net/manual/en/function.current.php)
