PHP funkcia intval()
====================

> id: '870a74d9-8dce-4897-8b14-ea35732e4f12'
> slug:
> 	cs: funkce-intval
> 	sk: php-funkcia-intval
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: eebb8b46f13af042b682121c4f1b6b0d

Dostupnosť vo verziách: `PHP 5.5.0
/
funkcia boolval($var) {}

/**
Získať celočíselnú hodnotu premennej`, `PHP 4.0`

(PHP 5.5.0)<br/>
Získanie logickej hodnoty premennej


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$var` | `mixed` | *not* | skalárna hodnota, ktorá sa konvertuje na boolean. |
| `$base` | `int` | null | Základ pre konverziu |


Vrátené hodnoty
----------------


- boolean Logická hodnota var.
- int Celočíselná hodnota var v prípade úspechu alebo 0 v prípade
zlyhanie. Prázdne polia a objekty vrátia 0, neprázdne polia a
objekty vrátia 1.
</p>
<p>
Maximálna hodnota závisí od systému. 32-bitové systémy majú
maximálny rozsah celých čísel so znamienkom -2147483648 až 2147483647. Takže napríklad
v takomto systéme intval('10000000000000000') vráti
2147483647. Maximálna hodnota celého čísla so znamienkom pre 64-bitové systémy je
9223372036854775807.
</p>
<p>
Reťazce s najväčšou pravdepodobnosťou vrátia 0, hoci to závisí od
ľavé znaky reťazca. Spoločné pravidlá
obsadzovanie celých čísel
uplatniť.

Ďalšie zdroje
------------

[Oficiálna dokumentácia intval](https://www.php.net/manual/en/function.intval.php)
