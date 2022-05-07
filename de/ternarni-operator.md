Ternäre Operatoren in PHP (?:) - Bedingung in einer Zeile
=========================================================

> id: '1191be9b-ff9d-4725-b0b4-17b60de2b935'
> slug:
> 	cs: ternarni-operator
> 	de: ternaere-operatoren-in-php---bedingung-in-einer-zeile
> 
> perex:
> 	- Ternární operátor umožňuje zapsat jednoduchou podmínku do jednoho řádku jako výraz.
> 	- 'Der ternäre Operator ermöglicht es Ihnen, eine einfache Bedingung in einer Zeile als Ausdruck zu schreiben.'
> 
> publicationDate: '2019-11-26 11:59:18'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: '491dbbbaeceba356a030e2501f5c0e2b'

Mit dem ternären Operator können Sie eine einfache Bedingung an einer Stelle, an der das Parsing unnötig, kompliziert oder schlichtweg unpassend ist, auf eine einzige Zeile verkürzen.

TL;DR
------

Die ternären Operatoren sind eine Abkürzung für die Bedingungen `if` und `else`, so dass Sie sie nicht verwenden müssen. Sie sind nützlich für sich ständig wiederholende Teile der Verifikationslogik. Verwenden Sie immer das Format `(Bedingung ? positive Logik : negative Logik)` einschließlich Klammern. Für kurze Überprüfungen verwenden, um den Code klarer und kürzer zu machen.

Grundlegende Definitionen
------------------

Oft haben wir eine Bedingung, zum Beispiel in der Form:

```php
$a = 5;
$b = 3;

if ($a > $b) {
     echo 'Es ist größer';
} else {
     echo 'Es ist kleiner';
}
```

Wenn wir also nur einen einfachen Satz schreiben wollen, müssen wir 4 Codezeilen verwenden, die auf eine reduziert werden könnten.

```php
$a = 5;
$b = 3;

echo 'Sie ist' . ($a > $b ? 'größer' : 'kleiner');
```

Im Allgemeinen wird der ternäre Operator mit 3 Teilen geschrieben (daher der Name "ternär"):

```php
(podmínka ? pokud je pravda : pokud není pravda)
```

Ternäre Operatoren werden in der Praxis sehr häufig verwendet, zum Beispiel um gerade Zeilen in einer Tabelle zu kennzeichnen:

```php
$pole = [3, 1, 4, 1, 5, 9, 2];

for ($i = 0; $pole[$i]; $i++) {
     echo '<tr class=""' . ($i % 2 ? 'von' : 'sogar') . '">';

          // Das funktioniert irgendwie mit einer Tabellenkalkulation...
          // zum Beispiel: echo '<td>' . $field[$i] . '</td>';

     echo '</tr>';
}
```

Beispiel für die Verwendung des ternären Operators
------------------------------------

Oft müssen wir das Vorhandensein eines Variablenwerts überprüfen und ihn bei Bedarf sofort verwenden. Wenn sie nicht vorhanden ist, soll der Standardwert zurückgegeben werden.

Der klassische Ansatz ist dieser:

```php
$a = 5;
$b = 8;

$c = $a ? $a : $b;
```

Wenn die Variable "$a" existiert, wird "$a" als Wert verwendet, ansonsten "$b".

Manchmal erhalten wir jedoch den Wert aus einer Funktion:

```php
$a = 5;
$b = 3;
$default = 42;

$c = funkce($a, $b) ? funkce($a, $b) : $default;
```

Diese Art des Aufrufs ist sehr ineffizient in Bezug auf die Systemressourcen. Zuerst muss die Funktion aufgerufen werden, und wenn sie existiert, wird sie erneut aufgerufen, um den Wert zu erhalten, der in der Variablen "$c" gespeichert wird.

Dies könnte besser über eine Hilfsvariable gehandhabt werden:

```php
$a = 5;
$b = 3;
$pomocnaPromenna = funkce($a, $b);
$default = 42;

$c = $pomocnaPromenna ? $pomocnaPromenna : $default;
```

Ungeeignete Verwendung
------------------

Der ternäre Operator ist nicht immer sinnvoll, da er beim Schreiben komplizierter und verschachtelter Bedingungen zu Verwirrung führen kann.

Sehen Sie sich selbst ein Beispiel an:

```php
$valid = true;
$lang = 'Französisch';

$x = $valid
     ? ($lang === 'Französisch' ? 'oui' : 'ja')
     : ($lang === 'Französisch' ? 'nicht' : 'von');
```

Würden Sie auf den ersten Blick erkennen, dass die Variable `$x` den Wert `oui` enthalten würde?

Mit ein wenig Übung vielleicht, aber das ist nicht die richtige Antwort. :)

Überprüfung des (Nicht-)Vorhandenseins des Wertes und Verwendung des Standardwertes
--------------------

Die ternären Operatoren sind am leistungsfähigsten bei der routinemäßigen Überprüfung des (Nicht-)Vorhandenseins von Werten und der Verwendung anderer Standardwerte.

Wir wollen z. B. prüfen, ob für einen Artikel eine Hauptkategorie existiert, und wenn nicht, eine Ersatzmeldung ausgeben:

```php
echo $mainCategory ?? 'Die Kategorie existiert nicht';
```

Der Operator `??` (zwei Fragezeichen) prüft, ob die Variable `$mainCategory` existiert und nicht `null` ist. Sie funktioniert auf die gleiche Weise wie die Funktion `isset()`.

Validierung der Leere eines Wertes
-----------------------------

Ein sehr oft nützliches Konstrukt, um zu überprüfen, ob der Ausgabewert nicht leer ist (d.h. nicht `null`, `0`, `false` oder `''*(Leerstring)*).

Dies kann wie folgt geschrieben werden:

```php
$a = 5;
$b = 3;
$default = 42;

$c = funkce($a, $b) ?: $default;
```

Zuerst wird die `Funktion($a, $b)` aufgerufen, dann wird ihr Wert getestet und wenn er nicht leer ist, wird er sofort an die Variable `$c` übergeben, andernfalls wird die Variable `$default` verwendet.

Der `?:`-Operator funktioniert wie der `!=`-Operator in einer Bedingung (falsch, unabhängig vom Datentyp) und kann z. B. durch ein Smiley-Gesicht mit Elvis-Haar in Erinnerung bleiben.

Unterstützung für ältere PHP-Versionen
----------------------------

Der `?:` Operator funktioniert seit PHP 7. In älteren Versionen müssen wir uns mit der `if (...)` Bedingung begnügen, die das gleiche Verhalten erreichen kann. Denken Sie daran, dass ternäre Operatoren eigentlich nur eine Abkürzung sind, um das Gleiche zu schreiben, was durch Bedingungen gehandhabt wird.

Das Nichtvorhandensein eines Wertes kann mit der Funktion `isset()` überprüft werden, die prüft, ob die Variable existiert und nicht leer ist (`null`).

Anstelle von Code:

```php
$a = 5;
$b = 3;

$c = $a ?? $b;
```

Dann schreiben wir die ältere Alternative auf:

```php
$a = 5;
$b = 3;

$c = isset($a) && $a ?? $b;
```

> **Warnung:** Die Reihenfolge von `isset()` und der Variablen selbst ist wichtig. Wenn wir die Reihenfolge umkehren und die Variable nicht existiert, würde ein Zugriffsfehler auf die nicht existierende Variable auftreten.
