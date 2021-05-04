End()
=====

> id: "879c7c9a-a497-4080-865d-f15e6c9e8578"
> slug:
> 	cs: end
> 
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

- Podpora: PHP 4, PHP 5
- Stručný popis: nastaví vnitřní ukazatel pole na jeho poslední prvek.
- Požadavky: Žádné

Popis
--------------------------

Funkce `end()` nastavuje vnitřní ukazatel array na poslední prvek a vrací jeho hodnotu.

Podobné funkce
--------------------------

- <a href="/current">current()</a>
- <a href="/each">each()</a>
- <a href="/prev">prev()</a>
- <a href="/reset">reset()</a>
- <a href="/next">next()</a>

Příklad
--------------------------

```php
echo end([
    'apple',
    'banana',
    'cranberry',
]); // vypíše cranberry
```

```php
$a = [];
$a[1] = 1;
$a[0] = 0;

echo end($a); // vypíše 0
```

Parametry
--------------------------

| #   | Typ     | Popis |
| --- | ------- | ----- |
| 1   | `array` | Název pole, s kterým se má pracovat. |

Návratové hodnoty
--------------------------

Vrátí hodnotu posledního prvku nebo `false` pro prázdné pole.

Rozdíly oproti starším verzím
--------------------------

Žádné
