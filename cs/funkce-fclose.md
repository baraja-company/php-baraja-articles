PHP funkce fclose()
===================

> id: b3724c46-cbdf-44a9-998a-6d51ed22b3c2
> slug:
> 	cs: funkce-fclose
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Closes an open file pointer


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$handle` | `resource` | *není* | The file pointer must be valid, and must point to a file successfully opened by fopen or fsockopen. |


Návratové hodnoty
----------------

`bool`

vrátí `true` v případě úspěchu, jinak `false` v případě neúspěchu.

Další zdroje
------------

[Oficiální dokumentace funkce fclose](https://www.php.net/manual/en/function.fclose.php)
