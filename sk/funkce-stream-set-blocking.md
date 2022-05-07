PHP funkcia stream_set_blocking()
=================================

> id: '1b06582b-7e6d-4a2e-bf35-ead2ccc2cb76'
> slug:
> 	cs: funkce-stream-set-blocking
> 	sk: php-funkcia-stream-set-blocking
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '4f005b5e45790ca928cff252e2a15922'

Dostupnosť vo verziách: `PHP 4.3.0`

Nastavenie blokovacieho/neblokovacieho režimu na toku


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$stream` | `resource` | *not* | Prúd. |
| `$mode` | `int` | *not* | Ak je mode 0, daný stream sa prepne do neblokujúceho režimu, a ak je 1, prepne sa do blokujúceho režimu. To ovplyvňuje volania ako fgets a fread, ktoré čítajú z prúdu. V neblokujúcom režime sa volanie fgets vždy vráti hneď, zatiaľ čo v blokujúcom režime sa bude čakať, kým budú dáta v prúde k dispozícii.


Vrátené hodnoty
----------------

`bool`

Vráti `true` pri úspechu, inak `false` pri neúspechu.

Ďalšie zdroje
------------

[Oficiálna dokumentácia k blokovaniu súboru prúdov](https://www.php.net/manual/en/function.stream-set-blocking.php)
