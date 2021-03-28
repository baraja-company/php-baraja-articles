PHP funkce strpos - výskyt podřetězce v řetězci
================================

> id: "4f7386ff-3ff4-4d62-ac65-97c78ef605e0"
> slugCS: funkce-strpos
> publicationDate: "2019-11-26 11:53:30"
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec

Funkce najde pozici prvního výskytu podřetězce v řetězci, což lidsky znamená, že ověří, jestli předaný řetězec obsahuje hledaný výraz a vrátí jeho pozici.

Funkce `strpos` vrací pozici hledaného textu v řetězci. Pokud řetězec obsahuje, vrátí pozici jeho prvního znaku jako `integer` (celé číslo), pokud neobsahuje, vrátí `false` (nepravda). Toho můžeme využít při testování řetězců.

Parametry
---------

| Parametr    | Datový typ | Výchozí hodnota | Poznámka |
|-------------|------------|-----------------|-----|
| `$haystack` | `string`   | *povinné*       | Řetězec, ve kterém se bude hledat |
| `$needle`   | `mixed`    | *povinné*       | Pokud není řetězec, převede se na číslo (integer) a použije se jako pořadová hodnota znaku. |
| `$offset`   | `int`      | `0`             | Pokud je nastavena nějaká hodnota, funkce nejprve odpočítá uvedený počet znaků od začátku řetězece a až poté začne vyhledávat. Na rozdíl od funkce `strrpos` a `strripos` nemůže být offset záporný. |

Výskyt řetězce v textu
----------------------

Často potřebujeme zjistit, jestli text obsahuje určitý řetězec. Bohužel v PHP neexistuje funkce `constains`, ale můžeme si ji snadno napsat sami:

```php
/**
 * Obsahuje $haystack řetězec $needle?
 */
function contains(string $haystack, string $needle): bool
{
    return strpos($haystack, $needle) !== false;
}
```

Podle toho lze sestavit například podmínka, zda řetězec obsahuje druhý řetězec a podle toho se dál zařídit:

```php
if (strpos('jan@barasek.com', '@') !== false) {
    // E-mail obsahuje zavináč
}
```

Výhoda této metody ověření výskytu řetězce je její extrémní rychlost. Pokud potřebujete ověřit složitější shodu, budou potřeba <a href="/regex">regulární výrazy</a>.

Návratové hodnoty
----------------

Funkce vždy vrátí pozici řetězce jako celé číslo (`int`), nebo hodnotu `false` (nepravda), pokud řetězec neobsahuje hledanou frázi.

Další zdroje
------------

[Oficiální manuál](https://php.net/manual/en/function.strpos.php)
