PHP funkcia socket_set_blocking()
=================================

> id: '1e938780-cb86-4aa2-9893-26167adde9cf'
> slug:
> 	cs: funkce-socket-set-blocking
> 	sk: php-funkcia-socket-set-blocking
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '4c0515dd25c4d3168a748e64e99fb07b'

Dostupnosť v `PHP 4.0`

&Alias; <function>stream_set_blocking</function>


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$socket` | `resource` | *not* | Prúd. |
| `$mode` | `int` | *not* | Ak je mode 0, daný stream sa prepne do neblokujúceho režimu, a ak je 1, prepne sa do blokujúceho režimu. To ovplyvňuje volania ako fgets a fread, ktoré čítajú z prúdu. V neblokujúcom režime sa volanie fgets vždy vráti hneď, zatiaľ čo v blokujúcom režime sa bude čakať, kým budú dáta v prúde k dispozícii.


Vrátené hodnoty
----------------

`bool`

Vráti `true` pri úspechu, inak `false` pri neúspechu.

Ďalšie zdroje
------------

[Oficiálna dokumentácia k blokovaniu zásuviek](https://www.php.net/manual/en/function.socket-set-blocking.php)
