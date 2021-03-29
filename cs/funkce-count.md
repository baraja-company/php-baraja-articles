PHP funkce count()
==================

> id: "5a07a47e-4e88-4d7c-8c1d-d6818aea0d8b"
> slugCS: funkce-count
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Counts all elements in an array, or something in an object.
<p>For objects, if you have SPL installed, you can hook into count() by implementing interface {

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$var` | `array|Countable` |  |  |
| `$mode` | `int` | COUNT_NORMAL |  |


Návratové hodnoty
----------------

`int`

the number of elements in var, which is
typically an array, since anything else will have one
element.
</p>
<p>
If var is not an array or an object with
implemented Countable interface,
1 will be returned.
There is one exception, if var is &null;,
0 will be returned.
</p>
<p>
Caution: count may return 0 for a variable that isn't set,
but it may also return 0 for a variable that has been initialized with an
empty array. Use isset to test if a variable is set.

Další zdroje
------------

https://php.net/manual/en/function.count.php
