PHP funkce var_dump()
=====================

> id: "3afb12dc-e91f-4126-9f64-e350449e095c"
> slug:
> 	cs: funkce-var-dump
> 
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Vypíše informace o proměnné přímo na výstup (do stránky).

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$expression` | `mixed` |  | Proměnná, kterou chceme vypsat |
| `$_` | `mixed` | null |  |


Návratové hodnoty
----------------

`void`

Funkce nevrací nic. Výstup vykresluje přímo do stránky podobně jako `echo`.

/--code php
var_dump('Mám rád PHP'); // string(13) "Mám rád PHP"
\--

/--code php
var_dump([1, 2, 3]);

// Vrátí toto:

array(3) {
  [0]=>
  int(1)
  [1]=>
  int(2)
  [2]=>
  int(3)
}
\--

Další zdroje
------------

https://php.net/manual/en/function.var-dump.php
