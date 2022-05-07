Variablen in PHP
================

> id: b89774e7-143c-4a8c-8dc6-3b3d2c78d5b7
> slug:
> 	cs: promenna
> 	de: variablen-in-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '23e4d537f18a3b2e1946c79be1aacf52'

Diese Seite ist eine vollständige Zusammenfassung der Funktionsweise von Variablen in PHP. Der Text ist in einem etwas technischen Stil geschrieben und für Anfänger vielleicht nicht ganz verständlich. Wenn Sie an den kompletten Grundlagen interessiert sind, lesen Sie <a href="/erstes-skript">das Einsteiger-Tutorial</a> und <a href="/grundsätze-der-schrift">Grundsätze zum Schreiben von Variablen</a>.

Beschreibung
-----

Eine Variable ist ein virtueller Speicherplatz im Arbeitsspeicher, der durch `Name` und `Datentyp` definiert ist. Innerhalb des Datentyps hat die Variable dann einen "Inhalt".

Intern stellt PHP die Variablen als so genannte Hash-Tabelle dar, d.h. alle Variablen werden in einer Tabelle gespeichert, die sehr schnell zu durchsuchen ist, so dass die für den Zugriff auf jede Variable benötigte Zeit *fast* konstant ist.

Schreibbeispiele:

```php
$a = 10;
$b = 'Katze';
$c = true;
```

Jede Zeile in der Stichprobe steht für die Definition einer Variablen. Der Name jeder Variablen beginnt mit dem Dollarzeichen "$", gefolgt von dem Namen selbst. Das Gleichheitszeichen kann verwendet werden, um der Variablen einen Wert zuzuweisen.

Intern werden die Variablen in Form einer Hashtabelle im Speicher abgelegt:

| Name | Typ | Abkürzung | Wert |
|-------|---------|---------|---------|
| "$a" | Ganzzahl | int | "10" |
| "$b" | Zeichenkette | Zeichenkette | "Cats" |
| "$c" | boolean | bool | "wahr" |

Variable Typen
---------------

Die Variablen werden je nach Zugriffsrechten und Verwendung klassifiziert:

- <a href="/local-variable">lokale Variablen</a> (gültig im Kontext, d.h. innerhalb von Funktionen und Methoden),
- <a href="/globale-variable">globale Variablen</a> (gültig für das gesamte Skript),
- <a href="/superglobale-variable">Superglobale Variablen</a> (gültig innerhalb der gesamten Serverumgebung - typischerweise Benutzerdaten),
- <a href="/promenna-variable">Variablen</a> (eine spezielle Variable, die einen Verweis (Link) auf eine andere Variable enthält).

* Globale Variablen und variable Variablen sollten nicht verwendet werden, da sie zur Unlesbarkeit des Codes und zu "magischem" (unerwartetem) Verhalten der Anwendung beitragen.

Erlaubte Inhalte von Variablen
--------------------------

Variablen können alles enthalten, was ihr aktueller Datentyp zulässt. Wenn der Datentyp nicht angegeben wird, wird er automatisch anhand des Inhalts bestimmt (dies wird nicht empfohlen, da es zu Fehlern führen kann).

Datentypen arbeiten unabhängig voneinander, so dass wir fast jeden Typ verwenden können. Wenn wir jedoch einen Zusammenführungsvorgang durchführen wollen, müssen wir immer die Konvertierung in ein Format sicherstellen.

Ein gutes Beispiel ist die Addition und Multiplikation von Zahlen:

```php
$x = 5;       // Ganzzahl
$y = 3;       // Ganzzahl
$z = $y + $y; // Die Variable $z wird aus mehreren Variablen zusammengesetzt
```

In diesem Fall stellt sich für PHP die Frage, welchen Datentyp die neu erstellte Variable `$z` haben soll. Wenn sie den gleichen Datentyp haben und die Operation möglich ist, wird der Datentyp vererbt.

Manchmal können wir jedoch eine Operation mit mehreren Datentypen durchführen:

```php
$x = 1;       // Ganzzahl
$y = 3.14159; // Schwimmer
$z = $y + $y; // Schwimmer
```

In diesem Fall verschmelzen wir Integer und Float. Die Ausgabe wird eine Dezimalzahl sein, daher wird float verwendet. In diesem Fall führt PHP eine so genannte **dynamische Repartitionierung** durch.

Wir können uns jedoch nicht immer auf dieses Verhalten verlassen. Wie möchten Sie zum Beispiel eine Zahl und eine Zeichenkette zusammenführen?

```php
$x = 256;     // Ganzzahl
$y = 'Hey! Hey!'; // Schwimmer
$z = $y + $y; // ???
```

Datentypen (Überblick über die wichtigsten)
--------------------------------------

PHP ist eine interpretierte Sprache und hat daher einige Besonderheiten im Vergleich zu anderen Programmiersprachen. Eine davon ist, dass wir keine Datentypen für Variablen angeben müssen, d.h. jede Variable ändert dynamisch ihren Datentyp je nach ihrem Inhalt (sofern nicht anders angegeben). Dennoch ist es gut, zumindest die grundlegenden Datentypen zu kennen, was besonders bei der Optimierung von Anwendungen oder der Arbeit mit Datenbanken nützlich ist.

Die Notation kann wie folgt aussehen:

```php
$x = (int) 25; // erstellt eine Variable vom Typ Integer
```

<a href="/datove-typy">Datentyp-Übersicht</a>.

Vererbung von Datentypen
-----------------------

Welchen Datentyp wird die Variable `$x` haben, wenn wir nur diesen Teil des Quellcodes kennen?

```php
$x = $y;
```

Er hängt vom Datentyp der Variablen `$y` ab, von der sowohl der Wert als auch der Datentyp geerbt werden. In diesem Fall kennen wir die Variable `$x` nicht, so dass wir mit der Auswertung des Codes nicht fortfahren können und eine Fehlermeldung ausgeben würden.

Dynamische Übersteuerung
---------------------

Nehmen wir die folgenden 2 Variablen an:

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

Darstellung von Zeichenketten als Arrays
------------------------------

Alle Zeichenketten werden intern als ein Array von Zeichen gespeichert. Das heißt, jedes Zeichen hat seinen eigenen **Index** und kann referenziert werden. Wenn wir keinen Index angeben, wird die gesamte Zeichenkette verarbeitet.

```php
$x = 'Lassen Sie uns in PHP programmieren!';
$n = 3;

echo $x;		// Dies gibt den gesamten Inhalt der Variablen $x aus
echo $x[0];		// Dies gibt das Null-Zeichen der Variablen $x aus
echo $x[$n];	// Dies druckt das n-te Zeichen der Variablen $x
```

> **Anmerkung:** PHP-Zahlen von Null an, d. h. das Nullzeichen ist 'P' und das erste Zeichen ist 'r'.
>
> > Außerdem werden die Zeichen byteweise umgeschaltet. Das Zeichen "no" in der UTF-8-Kodierung ist beispielsweise 2 Byte lang, so dass der Zeichenindex in der Zeichenfolge beim Blättern nicht mit der tatsächlichen Position übereinstimmt und zwei Indizes zum Speichern des Zeichens verwendet werden.

Die Existenz eines Array-Elements sollte immer mit der Funktion `isset()` überprüft werden:

```php
if (isset($x[$n])) {
    echo $x[$n];
}
```

Alternativ können Sie es auch mit einem ternären Operator aufschreiben:

```php
echo $x[$n] ?? '';
```

Kopieren von Variablen
---------------------

Nehmen wir die folgende Variable an:

```php
$q = 'Lorem ipsum, ...';
```

Und kopieren Sie dann den Wert in die nächste Variable:

```php
$qi = $q;
```

Glücklicherweise wird kein Kopieren durchgeführt und PHP speichert lediglich einen Verweis auf den Wert in einer **Hash-Tabelle**. Der Wert wird nur dann tatsächlich kopiert, wenn sich der Wert einer der Variablen ändern sollte. Dieses Verhalten wird von einer Komponente gehandhabt, die allgemein **Garbridge-Kollektor** genannt wird.
