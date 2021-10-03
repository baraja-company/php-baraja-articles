Zvýrazňování syntaxe PHP kódu funkcí highlight_string()
=======================================================

> id: "72338b08-9e4d-45c0-a5d2-7f8ee705ae93"
> slug:
> 	cs: funkce-highlight-string
>
> publicationDate: "2019-09-11 10:04:03"
> mainCategoryId: "5a538abc-5464-4707-9211-ea86885c80d4"

Pro zvýrazňování PHP kódu existuje celá řada knihoven, které mají různou spolehlivost a jsou různě náročné nainstalovat.

Pokud na zvýrazňování nemáme moc velké nároky, lze použít i přímo vestavěné funkce v PHP.

Základní použití
-----------------

Nejmenší možný příklad:

```php
highlight_string('<?php phpinfo(); ?>');
```

Funkce vrátí přímo na výstup zvýrazněný kód pomocí HTML značek (PHP 4):

```html
<code><font color="#000000">
<font color="#0000BB">&lt;?php phpinfo</font><font color="#007700">(); </font><font color="#0000BB">?&gt;</font>
</font>
</code>
```

V PHP 5 vypadá výstup o něco lépe (používá správě atribut `style`):

```html
<code><span style="color: #000000">
<span style="color: #0000BB">&lt;?php phpinfo</span><span style="color: #007700">(); </span><span style="color: #0000BB">?&gt;</span>
</span>
</code>
```

Kód je automaticky escapovaný, nemusíte se bát ho vypsat do stránky. Neobsahuje zranitelnost XSS.

Jak zvýrazňování interně funguje
----------------------------------

Proces zvýraznění kódu je velmi komplikovaný, protože nástroj musí rozumět syntaxi jazyka a znát všechna jeho pravidla.

PHP tuto úlohu řeší tak, že kód přijatý jako string nejprve rozparsuje na sérii malých logických celků procesem zvaným **tokenizace řetězců** (to se dá použít na mnohem víc věcí, třeba na <a href="/pokrocila-kalkulacka">implementaci pokročilé kalkulačky</a>).

Jednotlivé tokeny pak dostanou vlastní barvu a vypíší se ve stejném pořadí, jako byly v původním zdrojovém kódu.

> **POZOR:**
>
> Protože je pro výstup hodnoty interně využíván výstupní buffer, nemůže být funkce použita jako callback bufferovací funkce `ob_start()`.

Parametry
--------------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|-----|-----|-----|-----|
| `$str` | `string` |  | PHP kód, který má být zvýrazněn. Měl by zahrnovat úvodní značku `<?php`. |
| `$return` | `bool` | null | Pokud je `true`, funkce vrátí zvýrazněný kód. |

Návratové hodnoty
----------------

Pokud je `$return` nastaven na `true`, vrací funkce zvýrazněný kód. Pokud je `$return` nastaven na `false`, vrací funkce true při úspěchu a zvýrazněný kód vypisuje.

Hodnotu `false` vrátí v případě selhání.

Konfigurace
-------------

Jednotlivým PHP tokenům můžeme nastavit vlastní barvy funkcí `ini_set()` a to například takto:

```php
ini_set("highlight.comment", "#008000");
ini_set("highlight.default", "#000000");
ini_set("highlight.html", "#808080");
ini_set("highlight.keyword", "#0000BB; font-weight: bold");
ini_set("highlight.string", "#DD0000");
```

Další zdroje
------------

[Oficiální manuál](https://www.php.net/manual/en/function.highlight-string.php)
