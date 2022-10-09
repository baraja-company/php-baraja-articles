Konstruktionen omfatter
=======================

> id: '881cc6ef-b1dc-4a71-ab22-d1943ce8095b'
> slug:
> 	cs: include-function
> 	da: konstruktionen-omfatter
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '53294e511809a77c08883100fd7416c6'

| Støtte | PHP 4, PHP 5, PHP 7
|---------------|---------
| Kort beskrivelse | Tilføjer en anden tekstfil eller et andet script til scriptet.
| Krav | Anden tekstfil eller script, der skal indsættes.
| Bemærk | Kan ikke indlæse eksterne filer.

Beskrivelse
--------------------------

Indsætter en anden tekstfil eller et script på siden. Understøtter almindelige/tekstfiler.

Indlejrede filer opfører sig, som om de var direkte på siden.

Indlejrede scripts udføres automatisk.

Det indlejrede script overfører værdien af variabler.

> Kan ikke indlæses fra eksternt lager. Kan kun læse almindelig uformateret tekst og PHP-filer.

Tilladte stiformater:

- `script.php` - fil i samme mappe, vil blive eksekveret
- `script.html` - fil i samme mappe, vil ikke blive eksekveret
- `./file.php` - fil i samme mappe, vil blive udført
- `../page.html`
- `folder\file\file.php` - skriv i Windows
- `address/DalsiAddress/file.php` - skrivning på Unix-systemer

Skråstreger af typen `****` og `**/**` kan interkonverteres, så du behøver ikke at tage dig af det.

Ulovlig stiangivelse: `https://domena.pripona/slozka/soubor.php`

Lignende funktioner
--------------------------

- <a href="/file-get-contents">file_get_contents()</a>

Eksempel
--------------------------

```php
include 'file.php';
```

Den indsætter scriptet `file.php` i siden og kører det.

`vars.php`

```php
$color = 'grøn';
$fruit = 'æble';
```

`test.php`

```php
$color = '';
$fruit = '';

echo 'A' . $color . '' . $fruit; // A

include 'vars.php';

echo 'A' . $color . '' . $fruit; // Et grønt æble
```

> ADVARSEL: Følgende notation er ikke mulig, overfør indholdet af variabler ved at definere dem!

```php
include 'file.php?parameter=neco';
```

Returneringsværdier
--------------------------

Ingen, lægger bare filen ind.

**BEMÆRK:** Giver dig mulighed for at sammenligne filindhold, men det er en sikkerhedsrisiko. Rækkefølgen af parenteserne er vigtig! Eksempel:

```php
if ((include 'file.php') == 'OK') {
    echo 'Værdien er "OK".';
}
```

> Dette er en potentiel sikkerhedsrisiko!
>
> Kan løses med **file_get_contents()**, **readfile()** eller **fopen()**. Fopen() bør kun bruges til .txt- og .html-filer.
>
> Sikker læsning af filen kan løses ved at definere en brugerdefineret funktion.

Eksempel:

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

Noter og tips fra andre udviklere
--------------------------

**Jakub Vrána** sendte mig en e-mail:

```php
include 'artikler/' . $_GET['Artikel'] . '.html';
```

Det er ekstremt farligt.

En angriber kan sende et link til en anden mappe ved at bruge `../` eller noget lignende som artikelnavn, og nogle gange er det muligt at slippe af med enden ved at sende en null-byte i slutningen.

Du bør i det mindste bruge funktionen `basename()`, men det er bedre kun at tillade whitelist-værdier.
