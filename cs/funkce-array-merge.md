PHP funkce array_merge()
========================

> id: b6f27bc4-d565-4a43-91b4-c5083596cc03
> slug:
> 	cs: funkce-array-merge
>
> publicationDate: "2020-02-06 09:44:11"
> mainCategoryId: "59874540-664b-4474-9869-7e6742ab6051"

Dostupné ve všech verzích PHP

Spojí dvě a více polí dohromady a vytvoří jedno velké.

Parametry
---------

| Parametr  | Datový typ | Výchozí hodnota | Poznámka |
|-----------|------------|-----------------|----------|
| `$array1` | `array`    |  *není*         | Základní pole pro merge. |
| `$array2` | `array`    | `null`          | Druhé pole pro merge. |
| `$_`      | `array`    | `null`          | n-té pole pro merge. |

Polí můžeme předat libovolný počet, minimálně však dvě.

Návratové hodnoty
----------------

Funkce vrátí mergované pole typu `array`.

Další zdroje
------------

<a href="/mergovani-velkeho-pole">Mergování velkých polí v PHP</a>, užitečné pro mergování mnoha polí v cyklu.

[Oficiální manuál](https://www.php.net/manual/en/function.array-merge.php)
