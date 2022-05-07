PHP funkcia var_dump()
======================

> id: '3afb12dc-e91f-4126-9f64-e350449e095c'
> slug:
> 	cs: funkce-var-dump
> 	sk: php-funkcia-var-dump
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '1f0c27cc2d2c69fa0d3dc417ac5fe5e9'

Dostupnosť v `PHP 4.0`

Vytlačí premenné informácie priamo na výstup (stránku).

Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$expression` | `mixed` | *not* | Premenná, ktorá sa má vypísať |
| `$_` | `mixed` | null |


Vrátené hodnoty
----------------

`void`

Funkcia nevracia nič. Výstup sa zobrazí priamo na stránke ako `echo`.

```php
var_dump("Mám rád PHP); // string(13) "Mám rád PHP"
```

Príklad zložitejšej štruktúry:

```php
var_dump([1, 2, 3]);
```

On to vráti:

array(3) {
  [0]=>
  int(1)
  [1]=>
  int(2)
  [2]=>
  int(3)
}

Ďalšie zdroje
------------

[Oficiálna dokumentácia var-dump](https://www.php.net/manual/en/function.var-dump.php)
