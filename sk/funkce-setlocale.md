PHP funkcia setlocale()
=======================

> id: '7b94a46f-a7d9-4e65-80e5-b1b99db5e148'
> slug:
> 	cs: funkce-setlocale
> 	sk: php-funkcia-setlocale
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: e5f3e8db24e0f09ad241503a8a0294a8

Dostupnosť v `PHP 4.0`

Nastavenie informácií o lokalite


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$category` | `int` | *not* | <p> <em>kategória</em> je pomenovaná konštanta určujúca kategóriu funkcií, na ktoré má nastavenie locale vplyv: |
| `$locale` | `string` | *not* | Ak je locale &null; alebo prázdny reťazec "", názvy locale sa nastavia z hodnôt premenných prostredia s rovnakými názvami ako vyššie uvedené kategórie alebo z "LANG".
| `$_` | `string` | null | | |


Vrátené hodnoty
----------------

`string`

nový aktuálny locale alebo false, ak je funkcia locale
nie je implementovaný na vašej platforme, zadané lokálne prostredie neexistuje alebo
názov kategórie je neplatný.
</p>
<p>
Neplatný názov kategórie tiež spôsobí výstražné hlásenie. Kategória/lokalita
názvy možno nájsť v dokumente RFC 1766
a ISO 639.
Rôzne systémy majú rôzne schémy pomenovania miestnych názvov.
</p>
<p>
Návratová hodnota funkcie setlocale závisí od
v systéme, v ktorom je PHP spustené. Vracia presne
čo vráti systémová funkcia setlocale.

Ďalšie zdroje
------------

[Oficiálna dokumentácia funkcie setlocale](https://www.php.net/manual/en/function.setlocale.php)
