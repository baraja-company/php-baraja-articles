PHP funkce fopen()
================================

> id: "733ba757-1d5f-4717-a088-5ddabe730838"
> slugCS: fopen
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Funkce `fopen()` představuje low-level přístup k souborům na disku.

Programátor si musí zajistit všechno sám (otevření souboru, čtení dat, zapsání nových dat, zavření souboru).

Pokud potřebujete jen rychle číst a zapisovat soubory, existují jednodušší varianty:

- <a href="/file-get-contents">file_get_contents()</a> - čtení ze souboru
- <a href="/file-put-contents">file_put_contents()</a> - zápis do souboru

Základní použití
----------------

```php
$text = 'Libovolný text, který se uloží...';

$file = fopen('soubor.html', 'a+'); // Otevře soubor a režim

fwrite($file, $text); // Uloží do souboru
fclose($file);        // Zavře soubor
```

> Pokud soubor otevřeme pro čtení a nebude uzavřen, tak k němu nemůže jiný proces přistupovat!

Typy režimů práce se soubory
----------------------------

Se soubory můžeme pracovat v různých režimech, které říkají informace o právech k přístupu.

Například chceme soubor otevřít pouze pro čtení, pak stačí režim `r`.

Pokud soubor otevřeme pro zápis, tak bude na disk označen jako `otevřený` a jiný proces (script) do něj nebude moci zapisovat, dokud jej zase nezavřeme. Díky tomuto je zajištěno, že soubor nebude během zápisu poškozen.

| Režim | Význam |
|-------|--------|
| `a`   | Otevře soubor, pokud neexistuje, bude vytvořen |
| `a+`  | Otevře soubor pro přidání dat a nebo čtení dat, pokud neexistuje, bude vytvořen |
| `r`   | Otevře pouze pro čtení |
| `r+`  | Otevře pro čtení a zápis |
| `w`   | Otevře pro zápis, původní data budou smazána a nahrazena novými, pokud neexistuje, bude vytvořen |
| `w+`  | Otevře pro zápis a čtení, původní data budou smazána a nahrazena novými, pokud neexistuje, bude vytvořen |
