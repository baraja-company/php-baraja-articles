Besondere Steuerzeichen in PHP
==============================

> id: '888e6ee4-2ff3-4f0d-8f5a-4741e62e11d0'
> slug:
> 	cs: specialni-ridici-znaky
> 	de: besondere-steuerzeichen-in-php
> 
> publicationDate: '2021-11-24 14:05:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '2711968227a253040a0a6ed1464aa447'

PHP-Strings können spezielle Steuerzeichen enthalten, die in einem bestimmten Kontext unterschiedliche Bedeutungen haben und sich nicht unbedingt wie normale Zeichen verhalten.

Viele davon werden Ihnen bereits intuitiv bekannt sein. Einige sind für besondere Verwendungszwecke reserviert, andere sind beispielsweise für Tastaturzeichen vorgesehen.

Schreiben von Sonderzeichen
-----------------------

Sonderzeichen werden in doppelte Anführungszeichen gesetzt.

Es ist also ganz einfach:

```php
$message = "Hallo\nworld.";
```

Der vorangehende Code enthält einen Zeilenumbruch zwischen "Hallo" und "Welt".

Tabelle der Sonderzeichen
-------------------------

Wenn der String in doppelte Anführungszeichen (") eingeschlossen ist, interpretiert PHP die folgenden Escape-Sequenzen als Sonderzeichen:

| Reihenfolge | Bedeutung |
|----------|--------|
| Zeilenumbruch (LF oder 0x0A (10) in ASCII)
| "r" | Wagenrücklauf ("CR" oder "0x0D (13)" in ASCII) |
| ``t`` | horizontaler Tabulator (`HT` oder `0x09 (9)` in ASCII) |
| ``v`` | vertikaler Tabulator (`VT` oder `0x0B (11)` in ASCII) |
| Escape (ESC oder 0x1B (27) in ASCII)
| Formfeed (FF oder 0x0C (12) in ASCII)
| Schrägstrich...
| "$" | Dollarzeichen |
| Anführungszeichen |
| `[0-7]{1,3}` | Die einem regulären Ausdruck entsprechende Zeichenfolge ist ein Zeichen in oktaler Notation, das stillschweigend in ein Byte überläuft. (z.B. `"\400" === "\000"`) |
| "x[0-9A-Fa-f]{1,2}" | Die Zeichenfolge, die einem regulären Ausdruck entspricht, ist ein Zeichen in hexadezimaler Schreibweise.
| Die Zeichenfolge, die mit dem regulären Ausdruck übereinstimmt, ist ein Unicode-Codepunkt, der in der Zeichenkette als UTF-8-Darstellung dieses Codepunkts ausgegeben wird.

Wie bei Zeichenketten in einfachen Anführungszeichen wird ein Backslash ausgegeben, wenn ein anderes Zeichen maskiert wird.

Wenn Sie Strings mit Anführungszeichen abgrenzen, denken Sie daran, dass die enthaltenen Variablen erweitert werden (die Werte der Variablen werden direkt in den String geschrieben). Dieses Verhalten kann extrem gefährlich sein.
