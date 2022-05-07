Datei_Inhalt_ausgeben
=====================

> id: '7ec781c6-e3db-484d-8a49-0b93ab8252b1'
> slug:
> 	cs: file-put-contents
> 	de: datei-inhalt-ausgeben
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '78211a3696ebba56d559045f8a0ab9e4'

Die Funktion **file_put_contents** ist für das automatische Schreiben in eine Datei geeignet. Alternativ können Sie auch <a href="/fopen">fopen()</a> verwenden, was ich Anfängern jedoch nicht empfehle.

Muster
--------------------------

```php
$file = 'file.txt';
$content = 'Inhalt, der in einer Datei gespeichert werden soll.';

file_put_contents($file, $content);
```

file_put_contents hat 2 Parameter:

- `Dateiname`, wohin geschrieben werden soll,
- Der `Inhalt der Datei`, die wir schreiben werden.

> Hinweis: `file_put_contents()` überschreibt die Datei mit dem neuesten Inhalt.

Achten Sie auf das Überschreiben
--------------------------

Wenn Sie mit file_put_contents speichern, achten Sie darauf, die Daten zu überschreiben. Die Funktion löscht den gesamten aktuellen Inhalt und ersetzt ihn durch den neuen Inhalt. Wenn Sie also nur den Text anhängen wollen, können Sie ihn mit einem eigenen Skript entweder an den Anfang oder an das Ende setzen:

```php
$file = 'file.txt';
$content = 'Neuer Inhalt.';

$oldContent = file_get_contents($file);
file_put_contents($file, $content . $oldContent);
```

Zuerst wird also die Datei geöffnet, dann wird der neue Inhalt geschrieben, und danach wird der ursprüngliche Inhalt geschrieben...

Wenn wir den alten Inhalt vor dem neuen einfügen wollen, müssen wir das Skript nur leicht abändern:

```php
$file = 'file.txt';
$content = Nový obsah.';

$oldContent = file_get_contents($soubor);
file_put_contents($file, $oldContent . $content);
```
