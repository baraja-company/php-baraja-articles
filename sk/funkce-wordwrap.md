PHP funkcia wordwrap()
======================

> id: '2f2222d8-193f-422f-8d46-9f1c485abb12'
> slug:
> 	cs: funkce-wordwrap
> 	sk: php-funkcia-wordwrap
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '166e5f8389bc524d27862a11b73e9201'

Dostupnosť vo verziách: `PHP 4.0.2`

Obalí reťazec na zadaný počet znakov


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$str` | `string` | *not* | Spracovávaný reťazec. |
| `$width` | `int` | 75, | šírka stĺpca. |
| `$break` | `string` | "\n", | Reťazec sa láme pomocou voliteľného parametra break. |
| `$cut` | `bool` | false | Ak je parameter cut nastavený na hodnotu true, reťazec sa vždy zabalí na zadanej šírke alebo pred ňou. Ak je teda slovo väčšie ako zadaná šírka, rozdelí sa. (Pozri druhý príklad).


Vrátené hodnoty
----------------

`string`

Daný reťazec zabalený do zadaného stĺpca.

Ďalšie zdroje
------------

[Oficiálna dokumentácia k wordwrapu](https://www.php.net/manual/en/function.wordwrap.php)
