PHP funkce time_nanosleep()
===========================

> id: "481733e0-3060-4750-b1e8-c3ef63988d0a"
> slug:
> 	cs: funkce-time-nanosleep
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Zpoždění v řádu sekund a nanosekund


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$seconds` | `int` | *není* | Musí to být celé kladné číslo. |
| `$nanoseconds` | `int` | *není* | Musí to být kladné celé číslo menší než 1 miliarda. |


Návratové hodnoty
----------------

`bool`

Vrátí `true` v případě úspěchu, jinak `false` v případě neúspěchu.

Pokud bylo zpoždění přerušeno signálem, bude vráceno asociativní pole s komponentami:

- sekundy - počet sekund zbývajících do konce zpoždění
- nanosekundy - počet nanosekund zbývajících do zpoždění

Další zdroje
------------

[Oficiální dokumentace funkce time-nanosleep](https://www.php.net/manual/en/function.time-nanosleep.php)
