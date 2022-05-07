Das Konstrukt umfasst
=====================

> id: '881cc6ef-b1dc-4a71-ab22-d1943ce8095b'
> slug:
> 	cs: include-function
> 	de: das-konstrukt-umfasst
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '53294e511809a77c08883100fd7416c6'

| Unterstützung | PHP 4, PHP 5, PHP 7
|---------------|---------
| Kurzbeschreibung | Hängt eine weitere Textdatei oder ein Skript an das Skript an.
| Anforderungen | Andere Textdateien oder Skripte, die eingefügt werden sollen.
| Hinweis | Externe Dateien können nicht geladen werden.

Beschreibung
--------------------------

Fügt eine andere Textdatei oder ein Skript in die Seite ein. Unterstützt Plain/Text-Dateien.

Eingebettete Dateien verhalten sich so, als befänden sie sich direkt auf der Seite.

Eingebettete Skripte werden automatisch ausgeführt.

Das eingebettete Skript überträgt den Wert von Variablen.

> Kann nicht von externem Speicher geladen werden. Kann nur einfachen unformatierten Text und PHP-Dateien lesen.

Erlaubte Pfadformate:

- `script.php` - Datei im gleichen Verzeichnis, wird ausgeführt
- `script.html` - Datei im gleichen Verzeichnis, wird nicht ausgeführt
- `./file.php` - Datei im gleichen Verzeichnis, wird ausgeführt
- `../page.html`
- file.php" - Schreiben in Windows
- Adresse/DalsiAdresse/Datei.php" - Schreiben auf Unix-Systemen

Schrägstriche des Typs `****` und `**/**` können miteinander konvertiert werden, so dass Sie sich damit nicht befassen müssen.

Unzulässiger Pfadeintrag: `https://domena.pripona/slozka/soubor.php`

Ähnliche Funktionen
--------------------------

- <a href="/file-get-contents">file_get_contents()</a>

Beispiel
--------------------------

```php
include 'file.php';
```

Es fügt das Skript "file.php" in die Seite ein und führt es aus.

'vars.php'

```php
$color = 'grün';
$fruit = 'Apfel';
```

test.php

```php
$color = '';
$fruit = '';

echo 'A' . $color . '' . $fruit; // A

include 'vars.php';

echo 'A' . $color . '' . $fruit; // Ein grüner Apfel
```

> WARNUNG: Die folgende Notation ist nicht möglich, übertragen Sie den Inhalt von Variablen, indem Sie sie definieren!

```php
include 'datei.php?parameter=neco';
```

Rückgabewerte
--------------------------

Keine, legt nur die Datei ab.

**HINWEIS:** Ermöglicht den Vergleich von Dateiinhalten, was jedoch ein Sicherheitsrisiko darstellt. Die Reihenfolge der Klammern ist wichtig! Beispiel:

```php
if ((include 'file.php') == 'OK') {
    echo 'Der Wert ist "OK".';
}
```

> Dies ist ein potenzielles Sicherheitsrisiko!
>
> Kann mit **file_get_contents()**, **readfile()** oder **fopen()** gelöst werden. Fopen() sollte nur für .txt- und .html-Dateien verwendet werden.
>
> Ein sichereres Lesen der Datei kann durch die Definition einer eigenen Funktion erreicht werden.

Beispiel:

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

Hinweise und Tipps von anderen Entwicklern
--------------------------

**Jakub Vrána** hat mir eine E-Mail geschickt:

```php
include 'Artikel/' . $_GET['Artikel'] . '.html';
```

Dies ist äußerst gefährlich.

Ein Angreifer kann einen Link zu einem anderen Verzeichnis übergeben, indem er `../` oder etwas Ähnliches als Artikelnamen verwendet, und manchmal ist es möglich, die Endung loszuwerden, indem ein Null-Byte am Ende übergeben wird.

Sie sollten zumindest die Funktion `basename()` verwenden, aber besser ist es, nur Werte aus der Whitelist zuzulassen.
