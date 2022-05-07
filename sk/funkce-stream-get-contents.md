PHP funkcia stream_get_contents()
=================================

> id: '3fd19ad5-566c-488b-bd67-e13be297c480'
> slug:
> 	cs: funkce-stream-get-contents
> 	sk: php-funkcia-stream-get-contents
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '144064fb186dfcc56b74e704854e6a1d'

Dostupnosť v `PHP 5.0`

Čítanie zvyšku prúdu do reťazca


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$handle` | `resource` | *not* | Proudový zdroj (napr. vrátený z fopen) |
| `$maxlength` | `int` | null, | Maximálny počet bajtov na čítanie. Predvolená hodnota je -1 (prečítanie celej zostávajúcej vyrovnávacej pamäte).
| `$offset` | `int` | null | Pred čítaním vyhľadajte zadaný offset. |


Vrátené hodnoty
----------------

`string`

|bool reťazec alebo false pri zlyhaní.

Ďalšie zdroje
------------

[Oficiálna dokumentácia pre stream-get-contents](https://www.php.net/manual/en/function.stream-get-contents.php)
