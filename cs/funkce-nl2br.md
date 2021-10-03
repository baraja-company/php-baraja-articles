PHP funkce nl2br()
==================

> id: cbbf888a-9af2-49b2-984e-2d836b1337be
> slug:
> 	cs: funkce-nl2br
>
> publicationDate: "2020-02-16 18:48:19"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Funkce převede zalomení řádků (`\n`) ve stringu na HTML tag `<br>`.

Parametry
---------

| Parametr    | Datový typ | Výchozí hodnota | Poznámka |
|-------------|------------|--------|-----|
| `$string`   | `string`   | *není* | Vstupní řetězec. |
| `$is_xhtml` | `bool`     | `null` | Přepne způsob escapování podle kontextu. |

Podrobný popis
--------------

```php
$retezec = 'text
další text
a ještě něco';

echo nl2br($retezec);
```

**Vrátí:**

```html
text<br>
další text<br>
a ještě něco
```

Zalomené řádky v textu převede na html značky. Tento tag se používá na místech, kde uživatel zadává jakýkoliv text (textarea) a hrozí případ, že použije více řádkový text.

Ošetření klasických inputů (`type="text"`) nemá význam, jelikož zde nelze zadat víceřádkový text.

**Poznámka:**  Od PHP 4.0.5, je funkce `nl2br()` XHTML způsobilá. Všechny verze před 4.0.5 vrátí string se značkou vloženou před konce řádků místo `<br />`.

Příklad
-------

```php
echo nl2br("Welcome\r\nThis is my HTML document", false);
```

Vrátí:

```html
Welcome<br>
This is my HTML document
```

Návratové hodnoty
----------------

`string`

Vrací upravený řetězec včetně HTML značek.

Změny ve verzích
----------------

| Verze | Poznámka
|-------|---------
| 5.3.0 | Byl přidán volitelný parametr is_xhtml parametr.
| 4.0.5 | `nl2br ()` je nyní kompatibilní s XHTML. Všechny starší verze vrátí řetězec se zalomením vkládají řádků místo `<br />`.

Další zdroje
------------

[Oficiální dokumentace nl2br()](https://www.php.net/manual/en/function.nl2br.php)
