PHP funkcia nl2br()
===================

> id: cbbf888a-9af2-49b2-984e-2d836b1337be
> slug:
> 	cs: funkce-nl2br
> 	sk: php-funkcia-nl2br
> 
> publicationDate: '2020-02-16 18:48:19'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '68e1dd535c4e114e31699eb3faf7ebfe'

Táto funkcia konvertuje zalomenie riadku (`\n`) v reťazci na značku HTML `<br>`.

Parametre
---------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-------------|------------|--------|-----|
| `$string` | `string` | *not* | Vstupný reťazec. |
| `$is_xhtml` | `bool` | `null` | Prepína metódu escapovania podľa kontextu.

Podrobný opis
--------------

```php
$retezec = 'text
doplnkový text
a niečo iné";

echo nl2br($retezec);
```

**Vrátenie:**

```html
text<br>
viac textu<br>
a niečo iné
```

Prevedie zalomené riadky v texte na značky html. Táto značka sa používa na miestach, kde používateľ zadáva akýkoľvek text (textarea) a kde hrozí riziko použitia viacerých riadkov textu.

Ošetrenie klasických vstupov (`type="text"`) je nezmyselné, pretože tu nemožno zadávať viacriadkový text.

**Poznámka:** Od verzie PHP 4.0.5 je funkcia `nl2br()` XHTML oprávnená. Všetky verzie pred verziou 4.0.5 vrátia namiesto `<br />` reťazec so značkou vloženou pred zalomenie riadku.

Príklad
-------

```php
echo nl2br("Welcome\r\nToto je môj dokument HTML", false);
```

Návraty:

```html
Vitajte<br>
Toto je môj dokument HTML
```

Vrátené hodnoty
----------------

`string`

Vráti upravený reťazec vrátane značiek HTML.

Zmeny vo verziách
----------------

| Verzia | Poznámka
|-------|---------
| 5.3.0 | Pridaný voliteľný parameter is_xhtml.
| 4.0.5 | `nl2br ()` je teraz kompatibilný s XHTML. Všetky predchádzajúce verzie vrátia namiesto `<br />` reťazec s vloženými zalomeniami riadkov.

Ďalšie zdroje
------------

[Oficiálna dokumentácia funkcie nl2br]([Oficiálna dokumentácia funkcie nl2br()](https://www.php.net/manual/en/function.nl2br.php))
