PHP funkce array_search()
================================

> id: d4edadb2-ba8b-4c7f-957e-98e148f4ffbf
> slugCS: funkce-array-search
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "59874540-664b-4474-9869-7e6742ab6051"

Dostupnost ve verzích: `PHP 4.0.5`

Searches the array for a given value and returns the corresponding key if successful


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$needle` | `mixed` |  | The searched value. |
| `$haystack` | `array` |  | The array. |
| `$strict` | `bool` | null | If the third parameter strict is set to true then the array_search function will also check the types of the needle in the haystack. |


Návratové hodnoty
----------------

`mixed`

the key for needle if it is found in the
array, false otherwise.
</p>
<p>
If needle is found in haystack
more than once, the first matching key is returned. To return the keys for
all matching values, use array_keys with the optional
search_value parameter instead.

Další zdroje
------------

https://php.net/manual/en/function.array-search.php
