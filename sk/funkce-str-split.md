PHP funkcia str_split()
=======================

> id: '32f08691-047b-4b91-b626-e3ba88a4ba79'
> slug:
> 	cs: funkce-str-split
> 	sk: php-funkcia-str-split
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '7156ecb6fe9b8ea3ccc93edbbe8f7468'

Dostupnosť v `PHP 5.0`

Prevod reťazca na pole


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$string` | `string` | *not* | Spracovávaný reťazec. |
| `$split_length` | `int` | 1 | Maximálna dĺžka časti. |


Vrátené hodnoty
----------------

`array`

Ak je voliteľný parameter split_length
vrátené pole sa rozdelí na časti, pričom každá
s dĺžkou split_length, inak každý kúsok
bude mať dĺžku jedného znaku.
</p>
<p>
false sa vráti, ak je split_length menšia ako 1.
Ak dĺžka split_length prekročí dĺžku
reťazec, celý reťazec sa vráti ako prvý
(a jediný) prvok poľa.

Ďalšie zdroje
------------

[Oficiálna dokumentácia str-split](https://www.php.net/manual/en/function.str-split.php)
