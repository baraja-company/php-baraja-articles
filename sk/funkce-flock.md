PHP funkcia flock()
===================

> id: '19bac5b5-20b3-44be-8e79-6f626449fa4c'
> slug:
> 	cs: funkce-flock
> 	sk: php-funkcia-flock
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '58980d964417a1160af7b174ef84cb4a'

Dostupnosť v `PHP 4.0`

Prenosné blokovanie poradenských súborov


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$handle` | `resource` | *not* | Ukazovateľ na otvorený súbor. |
| `$operation` | `int` | *not* | operácia je jedna z nasledujúcich: LOCK_SH na získanie zdieľaného zámku (čítačka). @param int $wouldblock [nepovinné] <p> Nepovinný tretí argument je nastavený na hodnotu true, ak by sa zámok zablokoval (errno podmienka EWOULDBLOCK).
| `$wouldblock` | `int` | null | Nepovinný tretí argument je nastavený na true, ak by sa zámok zablokoval (errno podmienka EWOULDBLOCK).


Vrátené hodnoty
----------------

`bool`

Vráti `true` pri úspechu, inak `false` pri neúspechu.

Ďalšie zdroje
------------

[Oficiálna dokumentácia k funkcii flock](https://www.php.net/manual/en/function.flock.php)
