PHP funkcia parse_ini_string()
==============================

> id: '82a91c92-38e8-4a37-a4b4-26f52755f240'
> slug:
> 	cs: funkce-parse-ini-string
> 	sk: php-funkcia-parse-ini-string
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: c2695f6672329b5e7826aeb3bd7974e9

Dostupnosť vo verziách: `PHP 5.3.0`

Rozbor konfiguračného reťazca


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$ini` | `string` | *not* | Obsah analyzovaného ini súboru. |
| `$process_sections` | `bool` | null, | Nastavením parametra process_sections na hodnotu true získate viacrozmerné pole s názvami sekcií a nastaveniami. Predvolená hodnota pre process_sections je false |
| `$scanner_mode` | `int` | null | Môže byť INI_SCANNER_NORMAL (predvolené) alebo INI_SCANNER_RAW. Ak je zadaný INI_SCANNER_RAW, hodnoty možností sa nebudú analyzovať.


Vrátené hodnoty
----------------

`array`

|bool Nastavenia sa pri úspechu vrátia ako asociatívne pole,
a false pri zlyhaní.

Ďalšie zdroje
------------

[Oficiálna dokumentácia funkcie parse-ini-string](https://www.php.net/manual/en/function.parse-ini-string.php)
