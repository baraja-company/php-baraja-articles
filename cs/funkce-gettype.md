PHP funkce gettype()
====================

> id: ea1107b4-1a9f-497f-8e75-9e17ce4ca4c4
> slug:
> 	cs: funkce-gettype
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Vrátí typ proměnné.

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$var` | `mixed` |  | Proměnná, kterou chceme zkontrolovat |


Návratové hodnoty
----------------

`string`

Vrátí typ proměnné.

Vrací jednu z těchto možností:

- `boolean`
- `integer`
- `double` (z historických důvodů se vrací `double` místo `float`, vychází z implementace jazyka PHP v C, kde se používá právě `double`)
- `string`
- `array`
- `object`
- `resource`
- `NULL`
- `unknown type` (neznámý typ)

Další zdroje
------------

https://www.php.net/manual/en/function.gettype.php
