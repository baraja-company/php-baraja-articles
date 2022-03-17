PHP funkce substr_replace()
===========================

> id: "63d688c9-bdd6-43ed-a1e8-44815c4a9f8e"
> slug:
> 	cs: funkce-substr-replace
>
> publicationDate: "2019-09-11 10:04:04"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Nahrazení textu v části řetězce


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$string` | `mixed` | *není* | Zpracovávaný řetězec. |
| `$replacement` | `string` | *není* | Řetězec pro nahrazení. |
 | `$start` | `int` | *není* | Pokud je start kladný, nahrazování začne od počátečního posunu do řetězce. |
 | `$length` | `int` | null | Pokud je zadán a je kladný, představuje délku části řetězce, která má být nahrazena. Pokud je záporný, představuje počet znaků od konce řetězce, na kterém se má nahrazování ukončit. Není-li zadán, pak se implicitně použije hodnota strlen( string ); tj. nahrazování se ukončí na konci řetězce. Samozřejmě, pokud je délka nulová, pak tato funkce bude mít za následek vložení nahrazování do řetězce na zadaném počátečním posunu. |


Návratové hodnoty
----------------

`mixed`

Vrátí se výsledný řetězec. Pokud je řetězec pole, je vráceno pole.

Další zdroje
------------

[Oficiální dokumentace funkce substr-replace](https://www.php.net/manual/en/function.substr-replace.php)
