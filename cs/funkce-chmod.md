PHP funkce chmod()
==================

> id: bf778176-8523-4fc2-9a6a-7d08badb1deb
> slugCS: funkce-chmod
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Nastaví přístupová práva k souboru (čtení, zápis, smazání).

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` |  | Cesta k souboru (doporučuje se absolutní) |
| `$mode` | `int` |  | Práva k přístupu (například `0777` - přístup mají všichni). Poznámka: Práva budou automaticky převedena do šestnáctkové soustavy. Zápis jako string (například `g+w` není podporován). Pokud chcete zadat číslo v desítkové soustavě, jako první cifru uveďte nulu (například `0654`). |


Návratové hodnoty
----------------

`bool`

Vrátí `true`, když vše proběhlo v pořádku, `false` v případě chyby.

Další zdroje
------------

https://php.net/manual/en/function.chmod.php
