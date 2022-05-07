PHP Include - Einfügen einer Datei in eine Seite
================================================

> id: '7a53145c-8552-425d-b864-283f73a7a7de'
> slug:
> 	cs: include
> 	de: php-include---einfuegen-einer-datei-in-eine-seite
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '6e18e67ec03b3a7592473230559fc364'

Das `include`-Konstrukt fügt automatisch zusätzliche Dateien/Skripte in die aktuelle Seite ein.

Was PHP betrifft, so wird es automatisch ausgeführt und so verstanden, als hätte es sich schon immer an dieser Stelle befunden.

Beispiel:

```php
include 'nachrichten.html';
```

Praktische Anwendung
-----------------

- Gemeinsame Menüs und Menüs,
- Nachrichten, Aktualisierungen, Nachrichten und derselbe Inhalt,
- Kopfzeile oder Fußzeile, ...

Dynamisches Laden von Dateien
--------------------------

Oft müssen Dateien dynamisch geladen werden, zum Beispiel auf der Grundlage einer Variablen.

Zum Beispiel:

```php
$clanek = 'neco';

include $clanek . '.html';
```

Oder wir können Artikel dynamisch auf die Seite laden:

```php
include 'Artikel/' . $_GET['Seite'] . '.html';
```

Wenn Sie eine URL mit dem Parameter `page` aufrufen, wird der Artikel automatisch eingefügt, z.B.: `https://example.com/?page=novinky`.
