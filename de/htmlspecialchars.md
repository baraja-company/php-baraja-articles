Htmlspecialchars
================

> id: '46f04729-3956-4889-bb40-58362cb46b2a'
> slug:
> 	cs: htmlspecialchars
> 	de: htmlspecialchars
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0743dd02-12d0-4766-ba42-8fd7e9c4ae8a'
> sourceContentHash: '566e10c57527a45f32906beb6c21430f'

Htmlspecialchars() ist eine Funktion zur Umwandlung von Sonderzeichen in HTML-Entities.

Beschreibung
-----

```php
$promenna = htmlspecialchars($text);
```

Einige Sonderzeichen haben eine besondere Bedeutung für die Browser, so dass sie in Entitäten umgewandelt werden sollten. Dadurch wird die allgemeine Skriptsicherheit verhindert und eine fehlerhafte Darstellung der Seite vermieden.

Sie wird meist zum Schutz von Formularen und allen Stellen verwendet, an denen der Benutzer Text einfügt und die Gefahr besteht, dass er HTML-Tags einfügt.

| Zeichen | Hinweis | Änderungen an
|------|-------------------------|-----------
| `&` | kaufmännisches Und | `&amp;`
| "`"| doppelte Anführungszeichen (ändert sich, wenn `ENT_NOQUOTES` deaktiviert ist) | `&quot;`
| `'` | Hochkomma (ändert sich, wenn `ENT_QUOTES` aktiviert ist) | `&#039;`
| "<" | kleiner als, HTML-Klammer | "&lt;`
| ">" | größer als, HTML-Klammer | "&gt

Parameter
--------

**String** zu konvertieren

**Flaggen** Unterschiedliche Verhaltenseinstellungen

**charset** Gibt den Zeichensatz (Kodierung) an. Der Standardzeichensatz ist "ISO-8859-1".

Sie können `ISO-8859-1`, `ISO-8859-15`, `UTF-8`, `cp866`, `CP1251`, `CP1252`, und `KOI8-R` verwenden.

> Hinweis: Unterstützung erst ab PHP 4.3.0 und höher. Alle anderen Zeichensätze werden nicht erkannt und unterstützt.

**double_encode** Wenn `double_encode` deaktiviert ist, kodiert PHP keine existierenden HTML-Entities, standardmäßig wird alles konvertiert.

Rückgabewerte
-----------------

Konvertiert die Zeichenkette.

Enthält die Zeichenkette ungültige Einheiten innerhalb des angegebenen Zeichensatzes in `ENT_IGNORE` (nicht gesetzt), wird eine leere Zeichenkette zurückgegeben.

Änderungen in den Versionen
----------------

| Version | Hinweis
|-------|---------
| 5.4.0 | Hinzufügen der Konstanten `ENT_SUBSTITUTE`, `ENT_DISALLOWED`, `ENT_HTML401`, `ENT_XML1`, `ENT_XHTML` und `ENT_HTML5`.
| 5.3.0 | Hinzufügen der Konstante `ENT_IGNORE`.
| 5.2.3 | Hinzufügen des Parameters "double_encode".
| 4.1.0 | Hinzufügen des Parameters "Zeichensatz".

Beispiel
-------

```php
$new = htmlspecialchars(
	'<a href="test">Test</a>',
	ENT_QUOTES
);

echo $new; <a href="test">Test</a>
```
