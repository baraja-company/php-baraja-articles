PHP funkce unserialize()
========================

> id: "2a898eba-4e92-4d6b-ac54-618ff790cde7"
> slug:
> 	cs: funkce-unserialize
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`, `PHP 7.0`

Vytvoří hodnotu PHP z uložené reprezentace


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$str` | `string` | *není* | Serializovaný řetězec. |
| `$options` | `mixed` | null |  |


Návratové hodnoty
----------------

`mixed`

Vrací se převedená hodnota, která může být logická, celočíselná, floatová, řetězcová, pole nebo objektová.

V případě, že předaný řetězec není neserializovatelný, je vrácena false a je vydána `E_NOTICE`.

Další zdroje
------------

[Oficiální dokumentace funkce unserialize](https://www.php.net/manual/en/function.unserialize.php)
