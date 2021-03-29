PHP funkce key()
================

> id: ab9a57bb-1796-451e-a310-6e61a23acb82
> slugCS: funkce-key
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Fetch a key from an array


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$array` | `array` |  | The array. |


Návratové hodnoty
----------------

`mixed`

The key function simply returns the
key of the array element that's currently being pointed to by the
internal pointer. It does not move the pointer in any way. If the
internal pointer points beyond the end of the elements list or the array is
empty, key returns &null;.

Další zdroje
------------

https://php.net/manual/en/function.key.php
