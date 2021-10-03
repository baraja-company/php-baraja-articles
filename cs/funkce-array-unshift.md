PHP funkce array_unshift()
==========================

> id: "1933c5e2-c775-47ca-8887-48da5c101371"
> slug:
> 	cs: funkce-array-unshift
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "59874540-664b-4474-9869-7e6742ab6051"

Dostupnost ve verzích: `PHP 4.0`

Funkce připojí jeden nebo více prvků na začátek pole.

> **Pozor:** Číselné indexy budou přečíslovány od nuly. Funkce resetuje vnitřní ukazatel na aktuální prvek v poli.

Použití
--------

```php
array_unshift(array array, mixed var [, mixed ...])
```

Funkce vezme předaný prvek, který umístí na začátek nového pole. Za předaný prvek poté překopíruje původní pole se zachováním klíčů a pořadí.

Funkce vrátí nový počet prvků v poli. Pole se však upravuje přes referenci, takže je potřeba přečíst z proměnné, v které bylo předáno.

Použití je například:

```php
$queue = ['p1', 'p3'];
array_unshift($queue, 'p4', 'p5', 'p6');
```

Upravené pole `$queue` bude mít 5 prvků: "p4", "p5", "p6", "p1" a "p3".

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$array` | `array` |  | Upravované pole. |
| `$var` | `mixed` |  | Hodnota, která půjde na začátek pole. |
| `$_` | `mixed` | null | Volitelně další hodnoty, které půjdou na začátek pole. |

Návratové hodnoty
----------------

Vrátí `int` vyjadřující nový počet prvků v poli.

Další zdroje
------------

[Oficiální dokumentace](https://www.php.net/manual/en/function.array-unshift.php)
