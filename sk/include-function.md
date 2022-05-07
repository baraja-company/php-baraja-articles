Konštrukcia zahŕňa
==================

> id: '881cc6ef-b1dc-4a71-ab22-d1943ce8095b'
> slug:
> 	cs: include-function
> 	sk: konstrukcia-zahrna
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '53294e511809a77c08883100fd7416c6'

| Podpora | PHP 4, PHP 5, PHP 7
|---------------|---------
| Krátky popis | Pripojí ďalší textový súbor alebo skript k skriptu.
| Požiadavky | Iný textový súbor alebo skript, ktorý sa má vložiť.
| Poznámka | Nemožno načítať externé súbory.

Popis
--------------------------

Vloží do stránky ďalší textový súbor alebo skript. Podporuje obyčajné/textové súbory.

Vložené súbory sa správajú, ako keby boli priamo na stránke.

Vložené skripty sa vykonávajú automaticky.

Vložený skript prenáša hodnoty premenných.

> Nie je možné načítať z externého úložiska. Dokáže čítať iba neformátovaný text a súbory PHP.

Povolené formáty ciest:

- `script.php` - súbor v tom istom adresári, bude spustený
- `script.html` - súbor v tom istom adresári, nebude spustený
- `./file.php` - súbor v rovnakom adresári, bude spustený
- `../page.html`
- `folder\file\file.php` - zápis v systéme Windows
- `address/DalsiAddress/file.php` - zápis na unixových systémoch

Lomítka typu `****` a `**/**` sa môžu vzájomne konvertovať, takže sa tým nemusíte zaoberať.

Nezákonná položka cesty: `https://domena.pripona/slozka/soubor.php`

Podobné funkcie
--------------------------

- <a href="/file-get-contents">file_get_contents()</a>

Príklad
--------------------------

```php
include 'file.php';
```

Vloží do stránky skript `file.php` a spustí ho.

`vars.php`

```php
$color = 'zelená';
$fruit = 'jablko';
```

`test.php`

```php
$color = '';
$fruit = '';

echo 'A' . $color . '' . $fruit; // A

include 'vars.php';

echo 'A' . $color . '' . $fruit; // Zelené jablko
```

> VAROVANIE: Nasledujúci zápis nie je možný, prenášajte obsah premenných ich definovaním!

```php
include 'file.php?parameter=neco';
```

Vrátené hodnoty
--------------------------

Žiadne, len vloží súbor.

**POZNÁMKA:** Umožňuje porovnať obsah súborov, ale predstavuje to bezpečnostné riziko. Na poradí zátvoriek záleží! Príklad:

```php
if ((include 'file.php') == 'OK') {
    echo 'Hodnota je "OK"';
}
```

> Ide o potenciálne bezpečnostné riziko!
>
> Možno vyriešiť pomocou **file_get_contents()**, **readfile()** alebo **fopen()**. Funkcia Fopen() by sa mala používať len na súbory .txt a .html.
>
> Bezpečnejšie čítanie súboru možno vyriešiť definovaním vlastnej funkcie.

Príklad:

```php
$string = get_include_contents('somefile.php');

function get_include_contents(string $filename): ?string
{
    if (is_file($filename)) {
        ob_start();
        include (string) $filename;
        return ob_get_clean();
    }

    return null;
}
```

Poznámky a tipy od iných vývojárov
--------------------------

**Jakub Vrána** mi poslal e-mail:

```php
include 'články/' . $_GET['Článok'] . '.html';
```

Je to mimoriadne nebezpečné.

Útočník môže odovzdať odkaz na iný adresár pomocou `../` alebo niečoho podobného ako názov článku a niekedy je možné zbaviť sa koncovky odovzdaním nulového bajtu na konci.

Mali by ste použiť aspoň funkciu `basename()`, ale lepšie je povoliť len hodnoty z bieleho zoznamu.
