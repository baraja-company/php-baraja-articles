PHP funkce touch()
==================

> id: "8fa0546d-5d5c-4597-8387-3d9b83115a8e"
> slug:
> 	cs: funkce-touch
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Sets access and modification time of file


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` |  | The name of the file being touched. |
| `$time` | `int` | null, | The touch time. If time is not supplied, the current system time is used. |
| `$atime` | `int` | null | If present, the access time of the given filename is set to the value of atime. Otherwise, it is set to time. |


Návratové hodnoty
----------------

`bool`

true on success or false on failure.

Další zdroje
------------

https://www.php.net/manual/en/function.touch.php
