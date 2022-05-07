Konstruktionen omfattar
=======================

> id: '881cc6ef-b1dc-4a71-ab22-d1943ce8095b'
> slug:
> 	cs: include-function
> 	sv: konstruktionen-omfattar
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '53294e511809a77c08883100fd7416c6'

| Stöd | PHP 4, PHP 5, PHP 7
|---------------|---------
| Kort beskrivning | Lägger till en annan textfil eller ett annat skript till skriptet.
| Krav | Annan textfil eller manus som ska infogas.
| Obs | Kan inte ladda externa filer.

Beskrivning
--------------------------

Infogar en annan textfil eller ett annat skript på sidan. Stöder vanliga textfiler.

Inbäddade filer beter sig som om de fanns direkt på sidan.

Inbäddade skript utförs automatiskt.

Det inbäddade skriptet överför värdet på variablerna.

> Kan inte laddas från externt lagringsutrymme. Kan endast läsa vanlig oformaterad text och PHP-filer.

Tillåtna format för sökvägar:

- `script.php` - fil i samma katalog, kommer att köras
- `script.html` - fil i samma katalog, kommer inte att exekveras
- `./file.php` - fil i samma katalog, kommer att köras
- `../page.html`
- `folder\file\file.php` - skriva i Windows
- `address/DalsiAddress/file.php` - skrivning på Unix-system

Snedstreck av typen `****` och `**/**` kan interkonverteras, så du behöver inte ta hänsyn till det.

Felaktig sökväg: `https://domena.pripona/slozka/soubor.php`.

Liknande funktioner
--------------------------

- <a href="/file-get-contents">file_get_contents()</a>

Exempel
--------------------------

```php
include 'file.php';
```

Den infogar skriptet `file.php` i sidan och kör det.

`vars.php`

```php
$color = 'grönt';
$fruit = 'äpple';
```

`test.php`

```php
$color = '';
$fruit = '';

echo 'A' . $color . '' . $fruit; // A

include 'vars.php';

echo 'A' . $color . '' . $fruit; // Ett grönt äpple
```

> VARNING: Följande notation är inte möjlig, överför innehållet i variabler genom att definiera dem!

```php
include 'file.php?parameter=neco';
```

Returvärden
--------------------------

Ingen, lägger bara in filen.

**NOTAT:** Gör det möjligt att jämföra filinnehåll, men det är en säkerhetsrisk. Ordningen på parenteserna spelar roll! Exempel:

```php
if ((include 'file.php') == 'OK') {
    echo 'Värdet är "OK".';
}
```

> Detta är en potentiell säkerhetsrisk!
>
> Kan lösas med **file_get_contents()**, **readfile()** eller **fopen()**. Fopen() bör endast användas för .txt- och .html-filer.
>
> Säkrare läsning av filen kan lösas genom att definiera en egen funktion.

Exempel:

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

Anteckningar och tips från andra utvecklare
--------------------------

**Jakub Vrána** mejlade mig:

```php
include 'artiklar/' . $_GET['Artikel'] . '.html';
```

Detta är extremt farligt.

En angripare kan skicka en länk till en annan katalog med `../` eller något liknande som artikelnamn, och ibland är det möjligt att bli av med ändelsen genom att skicka en null-byte i slutet.

Du bör åtminstone använda funktionen `basename()`, men det är bättre att endast tillåta värden från whitelistan.
