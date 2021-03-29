PHP funkce trim()
=================

> id: a8b76833-94a0-4757-bdef-b480e0aa8d03
> slugCS: funkce-trim
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Smaže bílé znaky (nebo jiné znaky) od začátku a konce řetězce.

Alternativně lze použít `ltrim()` (smaže zleva), nebo `rtrim()` (smaže zprava).

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$str` | `string` |  | Řetězec, který bude oříznut. |
| `$charlist` | `string` | " | Volitelně mohou být trimované znaky také zadány pomocí parametru `$charlist`. Jednoduše uveďte všechny znaky, kterých se chcete zbavit. Pomocí konstrukce `..` můžete určit rozsah znaků. |

Návratové hodnoty
----------------

`string`

Upravený string

/--code php
echo trim('             Ahoj, jak se máš?      '); // "Ahoj, jak se máš?"

Další zdroje
------------

https://php.net/manual/en/function.trim.php
