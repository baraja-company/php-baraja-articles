PHP funkcia round()
===================

> id: '7c38379c-cfd2-47ba-93e4-4e770809dfdd'
> slug:
> 	cs: funkce-round
> 	sk: php-funkcia-round
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: f78d726d76ceeef1102c8161fd076146

Dostupnosť v `PHP 4.0`

Vráti zaokrúhlenú hodnotu val so zadanou presnosťou (počet číslic za desatinnou bodkou).
presnosť môže byť aj záporná alebo nulová (predvolené nastavenie).
Poznámka: PHP v predvolenom nastavení nespracúva správne reťazce ako "12 300,2". Pozrite časť Konverzia z reťazcov.


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$val` | `float` | *not* | Hodnota na zaokrúhlenie |
| `$precision` | `int` | 0, | Voliteľný počet desatinných číslic, na ktoré sa zaokrúhľuje. |
| `$mode` | `int` | PHP_ROUND_HALF_UP | Jedna z možností PHP_ROUND_HALF_UP, PHP_ROUND_HALF_DOWN, PHP_ROUND_HALF_EVEN alebo PHP_ROUND_HALF_ODD.


Vrátené hodnoty
----------------

`float`

Zaokrúhlená hodnota

Ďalšie zdroje
------------

[Oficiálna dokumentácia funkcie round](https://www.php.net/manual/en/function.round.php)
