PHP funkce sleep()
==================

> id: f125cbbd-ce50-4ff2-913d-e25e03936d34
> slug:
> 	cs: funkce-sleep
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupnost ve verzích: `PHP 4.0`

Uspání scriptu.

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$seconds` | `int` | *není* | Jak dlouho spát v sekundách. |


Návratové hodnoty
----------------

`int`

V případě správného průběhu vrátí nulu (`0`), nebo `false` v případě chyby.

Pokud bylo úspání uměle přerušeno, vrátí počet chybějících sekund do konce spánku.

```php
echo 'Ahoj!'; // Vypíše se hned

sleep(3); // Script bude 3 sekundy pozastavený

echo 'Jak se vede?'; // Vypíše se až za 3 sekundy
```

Další zdroje
------------

[Oficiální dokumentace funkce sleep](https://www.php.net/manual/en/function.sleep.php)
