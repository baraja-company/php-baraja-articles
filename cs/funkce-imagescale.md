PHP funkce imagescale()
================================

> id: "57134d08-36c4-417b-90ab-74becd2aa754"
> slugCS: funkce-imagescale
> publicationDate: "2020-02-16 16:23:15"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 5.5.0`

Změní měřítko (rozměry) obrázku podle dané šířky a výšky.

Parametry
--------------

| Parametr      | Datový typ | Výchozí hodnota | Poznámka |
|---------------|------------|--------|-----|
| `$image`      | `resource` | *není* | Zdroj obrázku (datový typ `resource`), který získáte některou z funkcí pro vytvoření nebo načtení obrázku, jako například [imagecreatetruecolor()](https://www.php.net/manual/en/function.imagecreatetruecolor.php). |
| `$new_width`  | `int`      | *není* | Nová šířka. |
| `$new_height` | `int`      | `-1`   | Nová výška. |
| `$mode`       | `int`      | `IMG_BILINEAR_FIXED` | Způsob úpravy obrázku. |


Návratové hodnoty
----------------

`resource` nebo `false`.

Funkce vrátí upravený obrázek jako datový typ `resource`. V případě chyby vrátí `false`.

Další zdroje
------------

- [funkce imagescale()](https://www.php.net/manual/en/function.imagescale.php)
- [funkce imagecreatetruecolor()](https://www.php.net/manual/en/function.imagecreatetruecolor.php)
