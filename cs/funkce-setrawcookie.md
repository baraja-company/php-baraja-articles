PHP funkce setrawcookie()
=========================

> id: d0a01c83-2bec-47e4-b852-34d851efd4d9
> slug:
> 	cs: funkce-setrawcookie
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Send a cookie without urlencoding the cookie value


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$name` | `string` | *není* |  |
| `$value` | `string` | null, |  |
| `$expire` | `int` | null, |  |
| `$path` | `string` | null, |  |
| `$domain` | `string` | null, |  |
| `$secure` | `bool` | null, |  |
| `$httponly` | `bool` | null |  |


Návratové hodnoty
----------------

`bool`

vrátí `true` v případě úspěchu, jinak `false` v případě neúspěchu.

Další zdroje
------------

[Oficiální dokumentace funkce setrawcookie](https://www.php.net/manual/en/function.setrawcookie.php)
