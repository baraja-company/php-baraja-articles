PHP funkcia get_html_translation_table()
========================================

> id: b1c948c8-9a7c-4359-ab15-446b76c37293
> slug:
> 	cs: funkce-get-html-translation-table
> 	sk: php-funkcia-get-html-translation-table
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '7707e99387c07a1dbc73658bc7f6036b'

Dostupnosť v `PHP 4.0`

Vracia prekladovú tabuľku používanú funkciami <function>htmlspecialchars</function> a <function>htmlentities</function>.


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$table` | `int` | null, | Existujú dve nové konštanty (HTML_ENTITIES, HTML_SPECIALCHARS), ktoré umožňujú určiť požadovanú tabuľku.
| `$quote_style` | `int` | null | Podobne ako pri funkciách htmlspecialchars a htmlentities môžete voliteľne určiť štýl citácie, s ktorým pracujete. Pozrite si opis týchto režimov v časti htmlspecialchars. |


Vrátené hodnoty
----------------

`array`

prekladovú tabuľku ako pole.

Ďalšie zdroje
------------

[Oficiálna dokumentácia pre get-html-translation-table](https://www.php.net/manual/en/function.get-html-translation-table.php)
