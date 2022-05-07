PHP funkcia html_entity_decode()
================================

> id: '54ac4951-8085-4cd8-9f75-114188fb6248'
> slug:
> 	cs: funkce-html-entity-decode
> 	sk: php-funkcia-html-entity-decode
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '7d96910f8c63e74b035c34f360fe76cb'

Dostupnosť vo verziách: `PHP 4.3.0`

Konvertovať všetky entity HTML na príslušné znaky


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$string` | `string` | *not* | Spracovávaný reťazec. |
| `$quote_style` | `int` | null, | Nepovinný druhý parameter quote_style umožňuje definovať, čo sa bude robiť s "jednoduchými" a "dvojitými" úvodzovkami. Preberá jednu z troch konštánt, pričom predvolená je ENT_COMPAT: <table> Dostupné konštanty štýlu quote_style <tr valign="top"> <td>Konštanta Názov</td> <td>Popis</td> </tr> <tr valign="top"> <td>ENT_COMPAT</td> <td>Prevedie dvojité úvodzovky a jednoduché ponechá.</td> </tr> <tr valign="top"> <td>ENT_QUOTES</td> <td>Prevedie dvojité aj jednoduché úvodzovky.</td> </tr> <tr valign="top"> <td>ENT_NOQUOTES</td> <td>Prevedie dvojité aj jednoduché úvodzovky.</td> </tr> </table> |
| `$charset` | `string` | null | Pre nepovinnú tretiu znakovú sadu sa predvolene používa znaková sada ISO-8859-1. Definuje znakovú sadu použitú pri konverzii. |


Vrátené hodnoty
----------------

`string`

dekódovaný reťazec.

Ďalšie zdroje
------------

[Oficiálna dokumentácia html-entity-decode](https://www.php.net/manual/en/function.html-entity-decode.php)
