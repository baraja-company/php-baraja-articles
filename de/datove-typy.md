Datentypen in PHP
=================

> id: cd63818c-259a-484e-b006-86c35b362019
> slug:
> 	cs: datove-typy
> 	de: datentypen-in-php
> 
> perex:
> 	- 'Datové typy v PHP: String, integer, float, boolean, pole (array), objekt a další.'
> 	- 'Datentypen in PHP: String, Integer, Float, Boolean, Array, Objekt und mehr.'
> 
> publicationDate: '2019-08-23 15:44:28'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: fcb2c647973c5f94e601c789b0a882f1

Alle in PHP verarbeiteten Daten sind von einem bestimmten Typ. Zum Beispiel eine ganze Zahl, eine Zeichenkette oder ein Boolean (wahr/falsch).

Grundlegende Datentypen
--------------------

Basistypen werden auch **primitive Datentypen** oder <a href="/function-is-scalar">Skalartypen</a> genannt.

| Typ | Name | Beschreibung |
|---------|-----------------|-------|
| `int` | Integer | (`integer`) Enthält nur Ganzzahlen. Der Höchstwert wird durch die Anzahl der Bits bestimmt. Bei 32-Bit-PHP reicht der Bereich von "2.147.483.648" bis "2.147.483.647" (~ ± 2 Milliarden), bei 64-Bit-PHP von "9.223.372.036.854.775.808" bis "9.223.372.036.854.775.807" (~ ± 9 Quintillionen). Der Maximalwert kann immer durch Aufruf der Konstante `PHP_INT_MAX` ermittelt werden. Wenn der Maximalwert einer Ganzzahl überschritten wird, stellt PHP die Zahl als "Float" dar und überschreibt sie automatisch.
| Dies ist eine Variante der Fließkommazahl, für die die Regel *"je kleiner, desto genauer "* gilt. Die Zahl wird intern als so genannte **Mantisse** und **Exponent** gespeichert, d.h. es werden eigentlich 2 Zahlen gespeichert, zwischen denen die Operation `Mantisse * (2^Exponent)` durchgeführt wird, wodurch es möglich ist, einen wirklich großen Zahlenbereich zu speichern. Dies beruht auf dem Prinzip, dass wir bei großen Zahlen nicht immer ihren genauen Wert kennen müssen, aber so viel Speicher wie möglich sparen wollen. Zahlen vom Typ `Float` müssen nicht exakt gespeichert werden und sollten nicht zur Geldberechnung verwendet werden.
| `String` | String | Enthält eine Folge von Zeichen, die durch Anführungszeichen oder Apostrophe begrenzt sind. Die maximale Länge ist nur durch die Kapazität des Arbeitsspeichers begrenzt. Eine Zeichenkette kann in beliebiger Kodierung gespeichert werden, Emoji oder Binärdaten enthalten.
| `bool` | Boolean | (`boolean`) Boolescher Wert aus der booleschen Algebra, kann nur `true` oder `false` enthalten.
| Der leere Wert "null" ist nützlich für Fälle, in denen wir ausdrücken wollen, dass etwas nicht existiert. Zum Beispiel hat ein Artikel keine Kategorie. Manchmal wird `null` fälschlicherweise durch null (`0`) oder leere Zeichenketten (`''`) ersetzt, aber dies ist keine gute Lösung, um Nicht-Existenz auszudrücken.

**Warnung:** Der Typ `null` ist nicht skalar.

Null (0) vs. Null
----------------

Vielen Entwicklern fällt es zu Beginn der Entwicklung schwer, den Unterschied zwischen `0` (Null) und `null` (ein nicht vorhandener Wert) zu verstehen.

Diese Unterscheidung lässt sich sehr gut und humorvoll anhand des folgenden Bildes erklären:

<img src="{$baseUrl}/images/0-vs-null.jpg" alt="0 vs. null" class="w-100 mb-3">

Manuelles Umpacken
--------------------

Einige Typen können ineinander umgewandelt werden. Der Zieldatentyp wird in runden Klammern angegeben und kann an beliebiger Stelle in der Anwendung angegeben werden, z. B. bei der Auflistung eines Wertes.

Zum Beispiel:

```php
$pi = 3.14;

echo $pi;       // druckt 3,14

echo (int) $pi; // druckt 3
```

Dynamisches Nachfüllen
---------------------

Betrachten Sie die folgenden 2 Variablen:

```php
$x = 10;
$y = '10';
```

Was ist der Unterschied zwischen dem Inhalt der Variablen "$x" und "$y"?

Die Variable `$x` ist eine Zahl, `$y` ist eine Zeichenkette (die eine "1" und eine "0" enthält), wenn wir also die Variable nur im Speicher ablegen und keine Operation durchführen, die den Wert beeinflusst. Zum Beispiel liefern die folgenden 2 Einträge das gleiche Ergebnis:

```php
echo $x + 5;	// druckt 15
echo $y + 5;	// druckt 15
```

Im zweiten Fall kommt es zum so genannten **dynamischen Überschreiben**, d. h. die Variable wandelt ihren Datentyp um, so dass eine Rechenoperation mit ihr durchgeführt werden kann. Auf dieses Verhalten kann man sich nicht immer verlassen, es ist eher ein korrigierendes Verhalten, um schlecht geschriebene Anfängerskripte zu korrigieren. Schreiben Sie Zahlen nach Möglichkeit immer mit einem Datentyp, um die Zahlen zu speichern, da dies ihre Genauigkeit erhöht und sie in Zukunft leichter zu verwenden sind.

> **Hinweis:** Es ist wichtig zu beachten, dass wir Datentypen nicht völlig willkürlich konvertieren können, daher ist dies nicht immer möglich. Wenn Sie einen Datentyp mit einem anderen (inkompatiblen) Datentyp überschreiben, kann es sein, dass die Konvertierung gar nicht stattfindet oder dass der ursprüngliche Inhalt beschädigt oder vollständig zerstört und durch einen anderen ersetzt wird. Wenn Sie zum Beispiel eine Zeichenkette in eine ganze Zahl umschreiben (und ein Text, der keine Zahl ist, in der Variablen gespeichert wird), wird der Wert "1" anstelle des numerischen Wertes in der Variablen gespeichert.

Vergleich von Typen
----------------

Wenn Sie Werte vergleichen, müssen Sie verschiedene Datentypen berücksichtigen.

Im Allgemeinen wird der Operator `==` für den allgemeinen Vergleich von zwei Werten (unabhängig vom Typ) verwendet, während `===` sowohl Werte als auch Datentyp vergleicht.

Zum Beispiel:

```php
$a = '';
$b = null;

if ($a == $b) {
    // Es wird als TRUE ausgewertet, weil
    // der Datentyp wird konvertiert.
}

if ($a === $b) {
    // Führt eine viel strengere Validierung durch
    // und es wird nicht passieren, weil es eine andere
    // Inhalt und einen anderen Datentyp, daher
    // Dieser Code wird nie ausgeführt.
}
```
