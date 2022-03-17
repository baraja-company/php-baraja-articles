PHP funkce str_repeat()
=======================

> id: b9749c0e-3003-4b92-846e-40654974d273
> slug:
> 	cs: funkce-str-repeat
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Opakuje string.

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$input` | `string` | *není* | String, který se má opakovat |
| `$multiplier` | `int` | *není* | Kolikrát se má opakovat |


Návratové hodnoty
----------------

`string`

Finální opakovaný string.

```php
echo 'A' . str_repeat('a', 5) . 'hoj'; // Aaaaaahoj
```

Další zdroje
------------

[Oficiální dokumentace funkce str-repeat]([Oficiální dokumentace funkce str-repeat](https://www.php.net/manual/en/function.str-repeat.php))
