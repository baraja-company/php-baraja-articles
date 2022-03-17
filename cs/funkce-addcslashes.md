PHP funkce addcslashes()
========================

> id: caa941b6-05e0-426f-97b7-f969c7874638
> slug:
> 	cs: funkce-addcslashes
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Quote string with slashes in a C style


Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$str` | `string` | *není* | Řetězec, který má být escapován. |
| `$charlist` | `string` | *není*  | Seznam znaků, které mají být escapovány. Pokud charlist obsahuje znaky `\n`, `\r` atd., jsou převedeny do stylu C, zatímco ostatní nealfanumerické znaky s ASCII kódy nižšími než 32 a vyššími než 126 jsou převedeny do osmičkové reprezentace. |


Návratové hodnoty
----------------

`string`

the escaped string.

Další zdroje
------------

[Oficiální dokumentace funkce addcslashes](https://www.php.net/manual/en/function.addcslashes.php)
