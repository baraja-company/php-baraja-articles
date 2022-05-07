Addcslashes
===========

> id: '48278937-b1c5-479b-bac2-9b1ec552df4c'
> slug:
> 	cs: addcslashes
> 	de: addcslashes
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1f53fe14dd5fbf79958c645d15c4d204'

Unterstützung von PHP4, PHP5

addcslashes" - Schrägstrich im C-Stil

Beschreibung
--------------------------

```php
string addcslashes (string $str, string $charlist)
```

Gibt eine Zeichenkette mit Backslashes vor den Zeichen zurück, die im Parameter charlist angegeben sind.

Parameter
--------------------------

**str** Textzeichenfolge

**charlist**

zu entfernende Zeichen. Wenn charlist die Zeichen `\n`, `\r` und andere enthält, werden sie in den C-Stil umgewandelt. Andere nicht-alphanumerische ASCI-Zeichen mit einer Länge von weniger als 32 und mehr als 126 ändern sich.

Wenn Sie eine Folge von Zeichen in einem charlist-Argument definieren, stellen Sie sicher, dass Sie wissen, welche Zeichen Sie als Anfang und Ende des Bereichs angeben.

```php
echo addcslashes('foo[ ]', 'A..z');

// Werte: \f\o\o\o[ \]
// Entfernt alle Klein- und Großbuchstaben
```

Rückgabewerte
--------------------------

Gibt die geänderte Zeichenkette zurück.

Beispiel
--------------------------

```php
$escaped = addcslashes($not_escaped, "\0..\37!@\177..\377");
```

charlist `\0..\37!@\177..\377`, entfernt alle Zeichen mit ASCII-Code zwischen 0 und 31.
