PHP funkcia sleep()
===================

> id: f125cbbd-ce50-4ff2-913d-e25e03936d34
> slug:
> 	cs: funkce-sleep
> 	sk: php-funkcia-sleep
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '55cd4508d0e744a9ee61e3aa30f5f469'

Dostupnosť v `PHP 4.0`

Skript spánku.

Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$seconds` | `int` | *not* | Ako dlho sa má spať v sekundách. |


Vrátené hodnoty
----------------

`int`

Vráti nulu (`0`), ak je správny, alebo `false`, ak je nesprávny.

Ak bol spánok umelo prerušený, vráti počet sekúnd, ktoré chýbajú do konca spánku.

```php
echo 'Hello!'; // Vytlačí sa okamžite

sleep(3); // Skript bude pozastavený na 3 sekundy

echo 'Ako sa máte?'; // Vypíše sa po 3 sekundách
```

Ďalšie zdroje
------------

[Oficiálna dokumentácia funkcie sleep](https://www.php.net/manual/en/function.sleep.php)
