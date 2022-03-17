PHP funkce stripos()
====================

> id: "93bcf683-be6f-4cfa-84ee-69b04af69604"
> slug:
> 	cs: funkce-stripos
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.0`

Vyhledání pozice prvního výskytu řetězce bez rozlišení velikosti znaků.

> **TIP:**
>
> Funkce `stripos` byla v minulosti používána pro ověření, zda řetězec obsahuje podřetězec.
> Od PHP 8.0 na toto existuje nativní funkce `str_contains()`.

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$haystack` | `string` | *není* | Řetězec, ve kterém se má hledat |
| `$needle` | `string` | *není* | Všimněte si, že jehla může být řetězec jednoho nebo více znaků. |
| `$offset` | `int` | null | Nepovinný parametr offset umožňuje určit, který znak v kupce sena se má začít hledat. Vrácená pozice je stále relativní vůči začátku zásobníku. |


Návratové hodnoty
----------------

`int` - vrátí pozici, kde začíná nalezený podřetězec.

Pokud se podřetězec v prohledávaném řetězci nenachází, vrátí hodnotu `false`.

Další zdroje
------------

[Oficiální dokumentace funkce stripos](https://www.php.net/manual/en/function.stripos.php)
