Htmlspecialchars
================

> id: '46f04729-3956-4889-bb40-58362cb46b2a'
> slug:
> 	cs: htmlspecialchars
> 	sk: htmlspecialchars
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0743dd02-12d0-4766-ba42-8fd7e9c4ae8a'
> sourceContentHash: '566e10c57527a45f32906beb6c21430f'

Htmlspecialchars() je funkcia na prevod špeciálnych znakov na entity HTML.

Popis
-----

```php
$variable = htmlspecialchars($text);
```

Niektoré špeciálne znaky majú pre prehliadače špeciálny význam, preto by sa mali konvertovať na entity. Tým sa zabráni všeobecnej bezpečnosti skriptu a zabráni sa nesprávnemu vykresleniu stránky.

Najčastejšie sa používa na ochranu formulárov a všetkých miest, kam používateľ vkladá text a kde hrozí vloženie značiek HTML.

| Znak | Poznámka | Zmeny na
|------|-------------------------|-----------
| `&` | ampersand | `&amp;`
| `"` | dvojitá úvodzovka (zmení sa, keď je vypnuté `ENT_NOQUOTES`) | `&quot;`
| `'` | apostrof (zmení sa, keď je zapnutá funkcia `ENT_QUOTES`) | `&#039;`
| `<` | menej ako, HTML zátvorka | `&lt;`
| `>` | väčšia ako, HTML zátvorka | `&gt;`

Parameter
--------

**Následný reťazec** na konverziu

**príznaky** Rôzne nastavenia správania

**charset** Určuje znakovú sadu (kódovanie). Predvolená znaková sada je `ISO-8859-1`.

Môžete použiť `ISO-8859-1`, `ISO-8859-15`, `UTF-8`, `cp866`, `CP1251`, `CP1252` a `KOI8-R`.

> Poznámka: Podpora len od verzie PHP 4.3.0 a novšej. Akékoľvek iné znakové sady nie sú rozpoznané a podporované.

**double_encode** Ak je funkcia `double_encode` vypnutá, PHP nebude kódovať existujúce entity HTML, predvolene sa konvertuje všetko.

Vrátené hodnoty
-----------------

Previesť reťazec.

Ak reťazec obsahuje neplatné jednotky v rámci zadanej znakovej sady v `ENT_IGNORE` (nie je nastavené), vráti sa prázdny reťazec.

Zmeny vo verziách
----------------

| Verzia | Poznámka
|-------|---------
| 5.4.0 | Pridanie konštánt `ENT_SUBSTITUTE`, `ENT_DISALLOWED`, `ENT_HTML401`, `ENT_XML1`, `ENT_XHTML` a `ENT_HTML5`.
| 5.3.0 | Pridanie konštanty `ENT_IGNORE`.
| 5.2.3 | Pridanie parametra `double_encode`.
| 4.1.0 | Pridanie parametra `charset`.

Príklad
-------

```php
$new = htmlspecialchars(
	'<a href="test">Test<a>',
	ENT_QUOTES
);

echo $new; // <a href="test">Test<a>
```
