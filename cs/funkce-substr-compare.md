PHP funkce substr_compare()
===========================

> id: "67b0a6e5-9779-4326-a489-53e114b777a5"
> slug:
> 	cs: funkce-substr-compare
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Binární bezpečné porovnání 2 řetězců od offsetu až do délky znaků


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$main_str` | `string` | *není* | Porovnávaný hlavní řetězec. |
| `$str` | `string` | *není* | Porovnávaný vedlejší řetězec. |
| `$offset` | `int` | *není* | Počáteční pozice pro porovnání. Pokud je záporná, začíná se počítat od konce řetězce. |
| `$délka` | `int` | null, | Délka porovnávání. |
| `$case_insensitivity` | `bool` | null | Pokud je case_insensitivity true, porovnání nerozlišuje velká a malá písmena. |


Návratové hodnoty
----------------

`int`

- `0` pokud `main_str` z pozice offset je menší než str
- `0`, pokud je větší než str
- `0`, pokud se rovnají.

Pokud je offset roven nebo větší než délka `main_str` nebo je délka nastavena a je menší než `1`, substr_compare vypíše varování a vrátí false.

Další zdroje
------------

[Oficiální dokumentace funkce substr-compare](https://www.php.net/manual/en/function.substr-compare.php)
