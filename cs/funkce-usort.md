PHP funkce usort()
==================

> id: "34e70aad-f369-4d43-87fb-9f3b7287c97e"
> slug:
> 	cs: funkce-usort
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Sort an array by values using a user-defined comparison function


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$array` | `array` | *není* | The input array. |
| `$cmp_function` | `callback` | *není* | The comparison function must return an integer less than, equal to, or greater than zero if the first argument is considered to be respectively less than, equal to, or greater than the second. |


Návratové hodnoty
----------------

`bool`

vrátí `true` v případě úspěchu, jinak `false` v případě neúspěchu.

Další zdroje
------------

[Oficiální dokumentace funkce usort](https://www.php.net/manual/en/function.usort.php)
