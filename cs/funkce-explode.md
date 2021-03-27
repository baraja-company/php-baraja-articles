PHP funkce explode()
================================

> id: a1ea6a22-7673-4cac-8624-1aa48194bf8b
> slugCS: funkce-explode
> perex: Rozdělení řetězce na více částí podle oddělovače.
> publicationDate: 2019-09-11 10:59:21
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec

Dostupnost ve verzích: `PHP 4.0`

Rozdělí string podle oddělovače typu string. Nelze nastavit více oddělovačů.

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$delimiter` | `string` |  | Oddělovací string |
| `$string` | `string` |  | Vstupní řetězec |
| `$limit` | `int` | null | Když je limit kladné číslo, vrácené pole bude obsahovat maximálně tolik prvků, kolik je limit. V posledním prvku pole budou případně další nenaparsované hodnoty, které se nevešly do pole. Pokud je limit záporný, parsuje se od konce řetězce. |


Návratové hodnoty
----------------

`array`

Vrátí pole prvků získaných podle oddělovače.

Když je oddělovač prázdn, vrátí `false`.

Pokud oddělovač obsahuje hodnotu, která není v řetězci obsažena a je použit záporný limit, bude vráceno prázdné pole. Pro ostatní nastavení limitu vrátí pole s jediným prvkem, kde bude původní string v nezměněné podobě.

```php
$items = explode(',', '2,4,6,8');

foreach ($items as $item) {
	echo $item . '<hr>';
}
```

Další zdroje
------------

https://php.net/manual/en/function.explode.php
