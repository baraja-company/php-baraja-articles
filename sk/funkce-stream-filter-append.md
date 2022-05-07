PHP funkcia stream_filter_append()
==================================

> id: '884e9069-ed27-407c-8320-e0aed9a01466'
> slug:
> 	cs: funkce-stream-filter-append
> 	sk: php-funkcia-stream-filter-append
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '4a84befa40a22f048b6704f6d172d823'

Dostupnosť vo verziách: `PHP 4.3.0`

Pripojenie filtra k prúdu


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$stream` | `resource` | *not* | Cieľový prúd. |
| `$filtername` | `string` | *not* | Názov filtra. |
| `$read_write` | `int` | null, | V predvolenom nastavení stream_filter_append pripojí filter k reťazcu filtrov čítania, ak bol súbor otvorený na čítanie (t. j. File Mode: r, a/alebo +). Filter bude tiež pripojený k reťazcu filtrov zápisu, ak bol súbor otvorený na zápis (t. j. režim súboru: w, a a/alebo +). Parametrom read_write možno odovzdať aj STREAM_FILTER_READ, STREAM_FILTER_WRITE a/alebo STREAM_FILTER_ALL, aby sa toto správanie prepísalo.
| `$params` | `mixed` | null | Tento filter bude pridaný so zadanými parametrami na koniec zoznamu, a preto bude pri operáciách s prúdom volaný ako posledný. Ak chcete pridať filter na začiatok zoznamu, použite príkaz stream_filter_prepend. |


Vrátené hodnoty
----------------

`resource`

zdroj, ktorý možno použiť na odkaz na tento filter
počas volania funkcie stream_filter_remove.

Ďalšie zdroje
------------

[Oficiálna dokumentácia k filtru stream-filter-append](https://www.php.net/manual/en/function.stream-filter-append.php)
