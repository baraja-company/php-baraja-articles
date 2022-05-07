PHP funkcia print_r()
=====================

> id: '91a2909b-d1ac-4180-b859-ef776ccbfcaf'
> slug:
> 	cs: funkce-print-r
> 	sk: php-funkcia-print-r
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '4a7009063998771456388e99cbbb2715'

Dostupnosť v `PHP 4.0`

Vytlačí ľudsky čitateľné informácie o premennej


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$expression` | `mixed` | *not* | Výraz, ktorý sa má vytlačiť. |
| `$return` | `bool` | null | Ak chcete zachytiť výstup print_r, použite parameter return. Ak je tento parameter nastavený na hodnotu true, print_r vráti svoj výstup namiesto toho, aby ho vytlačil (čo robí v predvolenom nastavení).


Vrátené hodnoty
----------------

`zmiešané`

Ak je zadaný reťazec, celé číslo alebo float,
sa vypíše samotná hodnota. Ak je zadané pole, hodnoty
budú prezentované vo formáte, ktorý zobrazuje kľúče a prvky. Podobné
pre objekty sa používa notácia.

Ďalšie zdroje
------------

[Oficiálna dokumentácia k funkcii print-r](https://www.php.net/manual/en/function.print-r.php)
