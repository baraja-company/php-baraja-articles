File_get_contents
=================

> id: "6c8889f1-95e7-4540-9c3a-0225c6383954"
> slugCS: file-get-contents
> publicationDate: "2019-09-11 10:18:03"
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f

Funkce **file_get_contents** slouží pro načtení souboru a jeho obsahu vložení do proměnné. Tato funkce je podobná funkci <a href="/include">include</a>, ale narozdíl od include zvládá načítat vzdálené soubory na internetu a jejich obsah přenášet přes proměnné.

Ukázka
------

Funkce lze použít buď pro načtení lokálního souboru z disku:

```php
$news = file_get_contents('news.html');

echo 'Aktuální novinky:<br>' . $news;
```

Nebo ze vzdálené URL:

```php
$page = file_get_contents('https://www.google.com');

echo $page;
```

Při načítání URL lze stáhnout libovolnou adresu a do proměnné získáme její obsah jako string. V případě HTML jde o zdrojový kód.

Stránka se vykresluje špatně
----------------------------

Důvod je ten, že se HTML kód předává přesně tak, jak je na URL umístěn.

Pokud je cesta k obrázku například `<img src="kocka.png">`, tak tento soubor v kontextu našeho serveru nemusí existovat, proto je potřeba cestu opravit například na: `<img src="https://server.cz/kocka.png">`.
