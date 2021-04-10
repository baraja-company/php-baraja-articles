PHP funkce array_slice()
========================

> id: df678ab7-3929-4833-9223-0afdca5f94d9
> slug:
> 	cs: funkce-array-slice
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "59874540-664b-4474-9869-7e6742ab6051"

Dostupnost ve verzích: `PHP 4.0`

Extract a slice of the array


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$array` | `array` |  | The input array. |
| `$offset` | `int` |  | If offset is non-negative, the sequence will start at that offset in the array. If offset is negative, the sequence will start that far from the end of the array. |
| `$length` | `int` | null, | If length is given and is positive, then the sequence will have that many elements in it. If length is given and is negative then the sequence will stop that many elements from the end of the array. If it is omitted, then the sequence will have everything from offset up until the end of the array. |
| `$preserve_keys` | `bool` | false | Note that array_slice will reorder and reset the array indices by default. You can change this behaviour by setting preserve_keys to true. |


Návratové hodnoty
----------------

`array`

the slice.

Další zdroje
------------

https://php.net/manual/en/function.array-slice.php
