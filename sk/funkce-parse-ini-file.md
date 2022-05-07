PHP funkcia parse_ini_file()
============================

> id: d58e96e6-670d-422b-8a0d-b255a9d66692
> slug:
> 	cs: funkce-parse-ini-file
> 	sk: php-funkcia-parse-ini-file
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '6331e31e0c1797975e3d409f95fbf9ce'

Dostupnosť v `PHP 4.0`

Rozbor konfiguračného súboru


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Názov analyzovaného ini súboru. |
| `$process_sections` | `bool` | null, | Nastavením parametra process_sections na hodnotu true získate viacrozmerné pole s názvami sekcií a nastaveniami. Predvolená hodnota pre process_sections je false |
| `$scanner_mode` | `int` | null | Môže byť INI_SCANNER_NORMAL (predvolené) alebo INI_SCANNER_RAW. Ak je zadaný INI_SCANNER_RAW, hodnoty možností sa nebudú analyzovať.


Vrátené hodnoty
----------------

`array`

|bool Nastavenia sa pri úspechu vrátia ako asociatívne pole,
a false pri zlyhaní.

Ďalšie zdroje
------------

[Oficiálna dokumentácia funkcie parse-ini-file](https://www.php.net/manual/en/function.parse-ini-file.php)
