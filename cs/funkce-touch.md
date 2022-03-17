PHP funkce touch()
==================

> id: "8fa0546d-5d5c-4597-8387-3d9b83115a8e"
> slug:
> 	cs: funkce-touch
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Nastaví čas přístupu a modifikace souboru


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` | *není* | Název souboru, kterého se dotýkáte. |
| `$time` | `int` | null, | Čas dotyku. Pokud není čas uveden, použije se aktuální systémový čas. |
| `$atime` | `int` | null | Je-li uveden, nastaví se čas přístupu k zadanému názvu souboru na hodnotu atime. V opačném případě se nastaví na hodnotu time. |


Návratové hodnoty
----------------

`bool`

vrátí `true` v případě úspěchu, jinak `false` v případě neúspěchu.

Další zdroje
------------

[Oficiální dokumentace funkce touch](https://www.php.net/manual/en/function.touch.php)
