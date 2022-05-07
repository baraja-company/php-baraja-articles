Mathematik in PHP
=================

> id: '69a432ee-7635-46e2-bb21-8492cb62c4e6'
> slug:
> 	cs: matematika
> 	de: mathematik-in-php
> 
> perex:
> 	- 'Matematika a matematické funkce v PHP. Definice čísel, operátorů a vhodné typy pro výpočty a ukládání čísel.'
> 	- 'Mathematik und mathematische Funktionen in PHP. Definitionen von Zahlen, Operatoren und geeigneten Typen für Berechnungen und die Speicherung von Zahlen.'
> 
> publicationDate: '2020-02-16 18:24:10'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '1070f11c80eff1faf3388e47721b01a2'

Wie in anderen Sprachen werden die Zahlen in PHP im Binärsystem (dem System aus Einsen und Nullen) dargestellt, so dass einige mathematische Operationen etwas andere Ergebnisse liefern können, als wir erwarten würden. Diese Seite gibt einen Überblick über das Verhalten von Berechnungen in verschiedenen Kontexten. Zum Verständnis sind zumindest Grundkenntnisse der **Numerischen Techniken** und **Zahlensysteme** erforderlich.

Darstellung von Zahlen im Speicher
---------------------------

Die Art und Weise, wie Zahlen dargestellt werden, wird durch den Datentyp charakterisiert, z. B. werden ganze Zahlen direkt aus dem Quellcode von dezimal nach binär konvertiert. Wir müssen uns überhaupt nicht um die Umrechnung von Zahlen kümmern, alles wird automatisch erledigt.

Diese Umrechnung hat zur Folge, dass der maximale Wert einer ganzen Zahl durch die Anzahl der im Speicher verfügbaren Bits bestimmt wird. Bei 32-Bit-PHP reicht der Bereich von "2.147.483.648" bis "2.147.483.647" (~ ± 2 Milliarden), bei 64-Bit-PHP von "9.223.372.036.854.775.808" bis "9.223.372.036.854.775.807" (~ ± 9 Quintillionen). Der Maximalwert kann immer durch Aufruf der Konstante `PHP_INT_MAX` ermittelt werden.

Dezimalzahlen werden im Datentyp **Fließkommazahl** gespeichert, der eine begrenzte Genauigkeit aufweist. Daher wird er intern als Zahlenpaar gespeichert, das der mathematischen Operation "Mantisse * (2^Exponent)" unterliegt. Die praktische Konsequenz ist, dass wir in der Lage sind, eine relativ große Zahl (in der Größenordnung von Milliarden) in einer kleinen Anzahl von Bits zu speichern, nur nicht sehr präzise.

Die Verwendung des Datentyps **float** hat zur Folge, dass das Ergebnis der Berechnung von "1 - 0,9" nicht genau "0,1" ist.

Beispiel aus <a href="https://diskuse.jakpsatweb.cz/?action=vthread&forum=9&topic=97912">Webforen</a>:

```php
$x = 0.3;
$y = 0.4;
echo 0.7 - $x - $y; // druckt -5,5511151231258E-17
```

Wenn wir die Zahlen leicht anpassen:

```php
$x = 6.5;
$y = 7.5;
echo 14.0 - $x - $y; // druckt 0
```

Im Allgemeinen wird dieses Thema <a href="https://vtm.zive.cz/clanky/proc-pocitacum-delaji-problemy-desetinna-cisla/sc-870-a-185672/default.aspx">in einem separaten Artikel auf VTM.e15.cz: Warum Computer Probleme mit Dezimalzahlen haben</a>, oder allgemein über <a href="https://cs.wikipedia.org/wiki/Pohyblivá_řádová_čárka">Gleitkomma auf Wikipedia</a> diskutiert.

> Im Allgemeinen ist es ratsam, das Ergebnis nach der Berechnung zu runden oder Dezimalstellen ganz zu vermeiden. Speichern Sie z. B. den Preis nicht in Kronen (z. B. 14,90), sondern in Pfennigen (z. B. 1490) und arbeiten Sie dann genau damit. Die Rundung wird im nächsten Abschnitt dieses Artikels behandelt.

Mathematische Operationen
-------------------

```php
$a = 5;
$b = 3;

echo $a + $b;
```

Für die Operationen können gängige mathematische Konventionen verwendet werden, die den Vorrang der Operatoren nach den Regeln der Mathematik beachten (Multiplikation hat Vorrang vor Addition usw.). Werte können direkt geschrieben oder über Variablen gelesen werden.

> Es ist vielleicht nicht auf den ersten Blick ersichtlich, aber im mathematischen Beispiel wird das Gleichheitszeichen (`=`) nicht geschrieben. Daraus folgt, dass nur einfache `Binäroperationen` gelöst werden können, die immer nur mit `zwei Elementen` arbeiten (Gleichungen nicht mitzählen, dafür muss man eine spezielle Bibliothek verwenden).

Übersicht über die grundlegenden Operatoren
----------------------------

> In der Spalte "Ergebnis" führe ich auf, was gedruckt wird, wenn ich davon ausgehe:

```php
$a = 5;
$b = 3;
$c = 10;
```

| Operation | Markierung | Markierung in Worten | Beispiel für Notation | Ergebnis
|-------------------|-----------|---------------|-----------------------|----------
| Addition | `+` | Plus | `echo $a + $b;` | 8
| Subtraktion | `-` | Minus | `echo $a - $b;` | 2
| Multiplikation | `*` | Sternchen | `echo $a * $b;` | 15
| Division | `/` | Schrägstrich | `echo $a / $b;` | 1.6666666666667
| Klammern | `( )` | Klammern | `echo $a + ($b * $c);`| 35
| Zeichenfolgenverkettung | `.` | Punkt | `echo $a . $b . $c;` | 5310
| Rest nach Division | `%` | Prozent | `echo $a % $b;` | 2
| Hinzufügen von einem | `++` | zwei Pluspunkten | `echo $a++;` | 6
| Eins subtrahieren | `--` | zwei minus | `echo $a--;` | 4
| Macht | `**` | zwei Sternchen | `echo $a ** $b;` | 125

> Der Potenzoperator (doppelter Stern) ist erst ab PHP 7.1 verfügbar, in anderen PHP-Versionen müssen Sie die universelle Funktion `pow($a, $b)` verwenden.

Überblick über grundlegende mathematische Funktionen
---------------------------------------

Wenn Sie eine gewöhnliche Rechenaufgabe lösen wollen, gibt es höchstwahrscheinlich bereits eine Funktion direkt in PHP, hier ist eine kommentierte Übersicht über sie.

Rundung
--------------

Normales Runden wird mit der <a href="/function-round">Runden()</a>-Funktion durchgeführt, sie hat 3 Parameter:

- Gerundete Zahl
- Optional: Genauigkeit (auf wie viele Dezimalstellen)
- Optional: Halbierung des Wertes (ab welchem Wert aufgerundet werden soll)

```php
round(5);               // 5
round(5.1);             // 5
round(5.4);             // 5
round(5.5);             // 6
round(5.8);             // 6
round(5483.47621, 2);   // 5483.47
round(5483.47621, -2);  // 5500
```

Außerdem gibt es eine <a href="function-floor">floor()</a>-Funktion zum Abrunden und eine <a href="/function-ceil">ceil()</a>-Funktion zum Aufrunden.

> **Achtung auf Datentypen!**
>
> Alle Rundungsfunktionen geben das Ergebnis als "Fließkomma" zurück, auch wenn das Ergebnis eine ganze Zahl ist.
>
> > Wenn Sie das Ergebnis als Ganzzahl behandeln wollen, ist es wichtig, das Ergebnis zu überschreiben. Zum Beispiel: `echo (int) round(3.14);`

PHP arbeitet mit verschiedenen Datentypen, z.B. kann es bei **float** leicht passieren, dass das Ergebnis von `round()` keine Zahl mit endlicher Dezimalerweiterung ist. Für die Auflistung empfehle ich die Funktion `number_format()`, die ich weiter unten bespreche.

Goniometrische Funktionen
--------------------

Diese werden für viele verschiedene Berechnungen verwendet und basieren auf dem Einheitskreis. Sie werden z. B. beim Zeichnen von Kreisen, Ellipsen, Verschiebungen usw. verwendet.

- <a href="/funktion-sin">sin()</a>
- <a href="/funktion-cos">cos()</a>
- <a href="/funktion-tan">tan()</a>

Zyklometrische Funktionen
--------------------

Berechne den Winkel zwischen "x" und "y". Umrechnungen von kartesischen und polaren Koordinaten.

- <a href="/funktion-asin">asin()</a>
- <a href="/funktion-acos">acos()</a>
- <a href="/funktion-atan">atan()</a>

Hyperbolische Funktionen
-------------------

Ausgehend von der Einheitshyperbel. Ihre Definitionen lassen sich leicht mit Hilfe von Gleichnissen beschreiben:

- Ellipse, Kreis, Kreis mit Radius 1
- Hyperbel, äquiaxiale Hyperbel, Einheitshyperbel

Sie werden z. B. bei der Geländeerstellung und der physikalischen Simulation eingesetzt.

- <a href="/funktion-sinh">sinh()</a>
- <a href="/funktion-cosh">cosh()</a> (chainline)
- <a href="/funktion-tanh">tanh()</a>

Hyperbolometrische Funktionen
------------------------

Ausgehend von der Einheitshyperbel.

- <a href="/funktion-asinh">asinh()</a>
- <a href="/funktion-acosh">acosh()</a>
- <a href="/funktion-atanh">atanh()</a>

Beispiele für die Verwendung goniometrischer Funktionen
---------------------------------------

```php
echo sin(30);            // -0.98803162409286
echo sin(deg2rad(30));   // 0.5
echo cos(deg2rad(123));  // -0.54463903501503
echo 1/tan(deg2rad(45)); // 1 (Onen-Kotangens)
echo sin(deg2rad(500));  // 0.64278760968654
echo atan2(deg2rad(50), deg2rad(23)); // 1.1396575860761
```

Taschenrechner - Verarbeitung mathematischer Ausdrücke
--------------------------------------------

Manchmal kann es vorkommen, dass wir einen mathematischen Ausdruck als Benutzerzeichenfolge verarbeiten müssen, zum Beispiel `5+2^(1+3/2)`.

Es gibt keine einfache Möglichkeit, solche Eingaben in PHP zu verarbeiten, aber ich habe eine Reihe von Bibliotheken programmiert, die dieses Problem lösen.

Details finden Sie im separaten Artikel <a href="/pokrocila-calculator">Calculator in PHP: Verarbeitung eines mathematischen Ausdrucks als String</a>.

Operationen mit großen Zahlen und Genauigkeit der Berechnungen
------------------------------------------

Große Zahlen und Dezimalzahlen werden in PHP als Fließkommazahlen gespeichert und auf etwa 15 Dezimalstellen gerundet, was manchmal lästig sein kann. Aus diesem Grund wurde die **<a href="https://www.php.net/manual/en/book.bc.php">BCMath Arbitrary Precision Mathematics Bibliothek</a>** geschaffen.

Zahlen werden in dieser Bibliothek als **Strings** dargestellt, so dass sie nicht durch die Ungenauigkeit von **float** eingeschränkt sind, aber die durchgeführten Operationen sind um mehrere Größenordnungen langsamer (aber immer noch schneller, als wenn wir eine solche Funktion selbst programmieren würden).

Wenn wir zum Beispiel eine Quadratwurzel mit 2 bis 3 Dezimalstellen ermitteln wollen, rufen wir einfach auf:

```php
echo bcsqrt('2', 3); // 1.414
```
