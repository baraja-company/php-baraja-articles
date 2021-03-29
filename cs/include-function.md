Konstrukt `include`
===================

> id: "881cc6ef-b1dc-4a71-ab22-d1943ce8095b"
> slugCS: include-function
> publicationDate: "2019-09-11 10:14:16"
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f

| Podpora       | PHP 4, PHP 5, PHP 7
|---------------|---------
| Stručný popis | Připojí do scriptu jiný textový soubor nebo script.
| Požadavky     | Jiný vkládaný textový soubor nebo script.
| Poznámka      | Neumí načítat externí soubory.

Popis
--------------------------

Vloží do stránky jiný textový soubor nebo script. Podporuje soubory typu plain/text.

Vložené soubory se chovají jako kdyby ve stránce byly přímo.

Vložené scripty se automaticky spouštějí.

Vložený script přenáší hodnotu proměnných.

> Nelze načítat z externího uložiště. Umí číst jen čisté neformátované texty a PHP soubory.

Povolené formáty cest:

- `script.php` - soubor ve stejném adresáři, bude spuštěn
- `script.html` - soubor ve stejném adresáři, nebude spuštěn
- `./soubor.php` - soubor ve stejném adresáři, bude spuštěn
- `../stranka.html`
- `slozka\DalsiSlozka\soubor.php` - zápis ve Windows
- `adresar/DalsiAdresa/soubor.php` - zápis v Unixových systémech

Lomítka typu `**\**` a `**/**` umí vzájemně převádět, takže to nemusíte řešit.

Nepovolený zápis cesty: `https://domena.pripona/slozka/soubor.php`

Podobné funkce
--------------------------

- <a href="/file-get-contents">file_get_contents()</a>

Příklad
--------------------------

```php
include 'file.php';
```

Do stránky vloží script `file.php` a spustí ho.

`vars.php`
```php
$color = 'green';
$fruit = 'apple';
```

`test.php`
```php
$color = '';
$fruit = '';

echo 'A ' . $color . ' ' . $fruit; // A

include 'vars.php';

echo 'A ' . $color . ' ' . $fruit; // A green apple
```

> VAROVÁNÍ: Následující zápis není možný, obsah proměnných přenášejte tím, že je definujete!

```php
include 'soubor.php?parametr=neco';
```

Návratové hodnoty
--------------------------

Žádné, jen vloží soubor.

**POZNÁMKA:** Umožňuje porovnat obsah souboru, je to ale bezpečnostní riziko. Na pořadí závorek záleží! Příklad:

```php
if ((include 'soubor.php') == 'OK') {
    echo 'Hodnota je "OK"';
}
```


> Jedná se o potenciální bezpečnostní riziko!
>
> Lze vyřešit funkcí **file_get_contents()**, **readfile()** nebo **fopen()**. Fopen() je vhodné použít jen na .txt a .html soubory.
>
> Bezpečnější čtení souboru lze vyřešit definováním vlastní funkce.

Příklad:

```php
$string = get_include_contents('somefile.php');

function get_include_contents($filename) {
    if (is_file($filename)) {
        ob_start();
        include $filename;
        return ob_get_clean();
    }
    return false;
}
```

Poznámky a tipy ostatních vývojářů
--------------------------

**Jakub Vrána** mi do mailu napsal:

```php
include 'clanky/' . $_GET['clanek'] . '.html';
```

Tohle je extrémně nebezpečné.

Útočník může jako název článku předat odkaz do jiného adresáře používající `../` nebo něco podobného, někdy je možné se zbavit i koncovky předáním nulového bajtu na konci.

Je potřeba použít alespoň funkci `basename()`, lépe ale povolit jen hodnoty z whitelistu.
