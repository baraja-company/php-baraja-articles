Drucken
=======

> id: '23d672c5-b347-43b8-bd08-8ccb7ecca33f'
> slug:
> 	cs: print
> 	de: drucken
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7f7c3c32d9f3c1efd794ca38b7db384

print" - String-Ausgabe

Beschreibung
--------------------------

```php
print 'Hallo, Welt!';
```

`print()` ist eigentlich keine echte Funktion (es ist ein Sprachkonstrukt), daher brauchen Sie keine Klammern zu verwenden.

Parameter
--------------------------

- Ich weiß nicht.

Ausgangsparameter

Rückgabewerte
--------------------------

Gibt immer die Zahl 1 zurück.

Hinweis
--------------------------

Hinweis: Da dies ein **Sprachkonstrukt** (keine Funktion) ist, kann es nicht in eine Variable geladen werden.

Beispiel
--------------------------

```php
print "Hallo, Welt";

print print" kann mehrere Textzeilen ausgeben, aber achten Sie auf den HTML-Tag
weil sie nicht gedruckt wird. Das ist es, was die <a href="/nl2br">Nl2br</a>.";

// Beispiel für eine Verbindung mit einer Variablen
$a = 'php';

print 'Ich mag' . $a; // Ich mag php
```

**print** ist genau die gleiche Funktion wie **echo**. Wenn Sie weitere Informationen suchen, lesen Sie den Artikel <a href="/echo">echo</a>.
