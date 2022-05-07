File_get_contents
=================

> id: '6c8889f1-95e7-4540-9c3a-0225c6383954'
> slug:
> 	cs: file-get-contents
> 	de: file-get-contents
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: f8b180a5f7208dab0d37761a5acdc9e3

Die Funktion **file_get_contents** wird verwendet, um eine Datei zu lesen und ihren Inhalt in eine Variable zu schreiben. Diese Funktion ähnelt der Funktion <a href="/include">include</a>, aber im Gegensatz zu include kann sie entfernte Dateien im Internet abrufen und deren Inhalt über Variablen übertragen.

Muster
------

Beide Funktionen können verwendet werden, um eine lokale Datei von der Festplatte zu laden:

```php
$news = file_get_contents('nachrichten.html');

echo 'Neueste Nachrichten:<br>' . $news;
```

Oder von einer entfernten URL:

```php
$page = file_get_contents('https://www.google.com');

echo $page;
```

Beim Abrufen einer URL kann eine beliebige Adresse heruntergeladen und ihr Inhalt als Zeichenkette in einer Variablen abgerufen werden. Im Falle von HTML ist dies der Quellcode.

Die Seite wird nicht korrekt wiedergegeben
----------------------------

Das liegt daran, dass der HTML-Code genau so übergeben wird, wie er in der URL steht.

Wenn der Pfad zum Bild z.B. `<img src="kocka.png">` lautet, dann kann es sein, dass diese Datei im Kontext unseres Servers nicht existiert, so dass wir den Pfad z.B. korrigieren müssen auf: `<img src="https://server.cz/kocka.png">`.
