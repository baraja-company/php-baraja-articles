PHP funkce uniqid()
===================

> id: "6871ccc8-590b-4cc9-ba18-d2aeab2b0838"
> slugCS: funkce-uniqid
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Vygeneruje unikátní identifikátor.

Používá se zejména pro generování ID v tabulkách, které jsou uloženy po částech na více serverech a dochází k pravidelné synchronizaci. Pokud databáze běží na více místech současně, nelze zaručit unikátnost každého číselného ID jako autoincrement (protože by musel existovat centrální server, co je přiděluje), proto se používá UID, tedy string, který má na každém stroji jiný prefix a generuje se jen jeho vnitřní struktura, ale základ zůstane stejný. Nedochází tedy ke kolizi identifikátorů při následné synchronizaci.

Vygenerovaný identifikátor obsahuje pomlčky a další znaky. Pokud potřebujeme garantovat vždy stejnou délku po sobě jdoucích písmen a číslic, můžeme identifikátor zahashovat: `md5(uniqid())`.

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$prefix` | `string` | "", | Může být užitečné, například, pokud generujete identifikátory současně na několika serverech, které by mohly generovat identifikátor ve stejné mikrosekundě. |
| `$more_entropy` | `bool` | false | Je-li nastavena na hodnotu `true`, uniqid přidá na konci návratové hodnoty další entropii (pomocí kombinovaného lineárního kongruenčního generátoru), což by mělo činit výsledky jedinečnější. |


Návratové hodnoty
----------------

`string`

Unikátní identifikátor jako string.

Další zdroje
------------

https://php.net/manual/en/function.uniqid.php
