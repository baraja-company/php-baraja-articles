PHP funkce is_scalar()
======================

> id: b791a763-52ae-429f-bedb-a3c8aec73bb1
> slug:
> 	cs: funkce-is-scalar
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "5bf11342-01a0-47e1-a6a8-c8c22bf92af9"

Dostupnost ve verzích: `PHP 4.0.5`

Ověří, jestli je vstup skalární hodnota.

Skalární proměnné jsou proměnné obsahující `integer` (celé číslo), `float`, `string` nebo `boolean`. Typ `array`, `object` a `resource` není skalární.

Pozor: Hodnota `null` není skalární!

> **Poznámka:**
>
> is_scalar() nepovažuje hodnoty typu `resource` za skalární, protože jde o abstraktní datový typ (který se může v čase změnit). Na tento detail implementace by se nemělo spoléhat, protože se může změnit.

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$var` | `mixed` | *není* | Proměnná, která se bude kontrolovat |


Návratové hodnoty
----------------

`bool`

Vrátí `true`, pokud je vstup skalární. V ostatních případech `false`.

Další zdroje
------------

[Oficiální dokumentace funkce is-scalar]([Oficiální dokumentace](https://www.php.net/manual/en/function.is-scalar.php))
