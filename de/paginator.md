Paginator und Paginierung der Ergebnisse in PHP
===============================================

> id: a1450160-e320-414a-8266-128465632e94
> slug:
> 	cs: paginator
> 	de: paginator-und-paginierung-der-ergebnisse-in-php
> 
> perex:
> 	- Stránkování dlouhého seznamu položek. Jak řešit omezení počtu vypsaných položek a vypočítat stránkování.
> 	- Paginierung einer langen Liste von Einträgen. Wie löst man die Begrenzung der Anzahl der aufgelisteten Artikel und berechnet die Paginierung?
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: d0bc71ded032401875396b0fd263a820

Wenn wir eine große Menge an Daten zu löschen haben, ist es höflich, sie auf mehrere Seiten aufzuteilen. Dieser Artikel befasst sich nicht mit der praktischen Umsetzung der Weitergabe von Seitenzahlen und der Auflistung von Ergebnissen, sondern nur mit der theoretischen Extraktion von Werten und der Berechnung des optimalen Codebuchs, um das Durchsuchen einer großen Anzahl von Seiten so benutzerfreundlich wie möglich zu gestalten.

Wie viele Ergebnisse haben wir
----------------------

Zunächst einmal müssen wir herausfinden, wie viele Ergebnisse wir überhaupt haben. Wenn die Daten aus einer Datenbank stammen, können sie mit der folgenden SQL-Anweisung sehr effizient gezählt werden:

```sql
SELECT COUNT(*) FROM tabulka
```

Die Berechnung ist sehr schnell, da die Datenbank die Statistiken in einer Hilfsdatei speichert, so dass sie die Daten überhaupt nicht berührt.

Wenn die Daten von einem anderen Ort kommen (und wir sie z. B. in einem Array haben), können sie mit der Funktion count() gezählt werden:

```php
$cisla = [3, 1, 4, 1, 5, 9, 2];

echo 'Das Feld enthält' . count($cisla) . 'Zahlen.';
```

Begrenzung der Anzahl der Ergebnisse
----------------------

Ein weiteres Problem ist die Begrenzung der Anzahl der Ergebnisse. Wenn sich die Daten in der Datenbank befinden, fügen Sie einfach den Parameter `LIMIT` in die SQL-Anweisung ein:

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10
```

Dieser Befehl liefert immer maximal 10 Ergebnisse und beschleunigt die Abfrage, da die Datenbank nicht die gesamten Dateien durchgehen muss.

Wenn wir Daten aus einer anderen Quelle haben (wiederum ein Array), können wir die Ergebnisse auch auf PHP-Ebene mit der Hilfsvariablen "$iterator" einschränken:

```php
$pole = [...];

$iterator = 0;
$limit = 10;
foreach ($pole as $prvek) {
	// hier werden die Daten entsorgt

	$iterator++;
	if ($iterator >= $limit) {
	    break; // Stoppt den Zyklus, wenn er 10 Mal gelaufen ist
	}
}
```

Überspringen der ersten X Ergebnisse
----------------------

Wenn wir auf der ersten Seite sind, ist es ziemlich einfach, Sie müssen nur die Anzahl der Ergebnisse mit `LIMIT` begrenzen. Aber was ist, wenn ich auf der dritten Seite bin? Dann müssen wir die ersten "X" Ergebnisse auslassen.

In SQL gibt es dafür wiederum eine elegante Notation:

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10 OFFSET 20
```

Sie überspringt die ersten 20 Ergebnisse und beschränkt die nächste Ausgabe auf 10 Ergebnisse, so dass sie das Intervall `<21 - 30>` ausgibt.

In reinem PHP wird dies auf zwei Arten gehandhabt.

Wenn wir die Indizes des Arrays kennen, können wir ab einem bestimmten Punkt mit dem Lesen beginnen (was sehr schnell ist):

```php
$pole = [...];

$start = 20;
$limit = 10;
for ($i = $start; ($i <= $start + $limit && isset($pole[$i])); $i++) {
	// hier werden die Daten entsorgt
}
```

Bei einem unbekannten Feld müssen wir jedoch erneut den Iterator verwenden und die Elemente überspringen:

```php
$pole = [...];

$iterator = 0;
$start = 20;
$limit = 10;
foreach ($pole as $prvek) {
	if ($iterator < $start) {
		$iterator++;
		continue;
	}

	// irgendwie werden die Daten hier abgeladen

	$iterator++;
	if ($iterator >= $start + $limit) break;
}
```

Anzeige des optimalen Paginators/Steppers
----------------------

Angenommen, wir kennen die Gesamtzahl der Artikel, die Anzahl der Artikel auf der Seite und die aktuelle Seitenzahl. Jetzt wollen wir eine Leiste erstellen, die ein schnelles Blättern durch alle Seiten mit Suchergebnissen ermöglicht. Da es jedoch viele Seiten gibt (in der Größenordnung von Tausenden), können wir sie nicht alle auf einmal auflisten, also müssen wir auf intelligente Weise einige repräsentative Seiten auswählen, die die Bandbreite zwischen den Seiten am besten darstellen.

Das könnte so aussehen:

```php
1 | 15 | 30 | 36 | 45 | 60 | 72
```

Zuweisung:

Ich bin auf Seite 36 von 72, wie kann ich die Seitenzahlen optimal platzieren?
Nun, durch die Sequenz.

> **Tipp:** Durch praktische Beobachtung habe ich herausgefunden, dass der linke Teil des Paginators durch eine arithmetische Sequenz gezählt werden sollte (so kann ich mich linear um die gleiche Anzahl von Schritten bewegen) und der rechte Teil durch eine **geometrische Sequenz**, was es wiederum einfach macht, einen großen Schritt zu machen. Wenn ich also zu einer bestimmten Seite gelangen möchte, überspringe ich zunächst eine große Anzahl unnötiger Elemente und verfeinere dann die Auswahl, indem ich nach links zurückkehre.

Arithmetische Sequenztheorie (wir addieren immer wieder die gleiche Zahl):

```php
$d = 10;   // Schrittweite
$a[1] = 1; // erstes Element
$a[2] = $a[1] + $d; // zweites Element
$a[3] = $a[1] + 2 * $d;
$a[3] = $a[2] + $d;
$a[$n] = $a[1] + ($n - 1) * $d; // n-tes Element

function getAritmeticItem(int $start, int $step, int $n): int
{
	return $start + ($n - 1) * $step;
}
```

Geometrische Sequenztheorie (immer mit der gleichen Zahl multiplizieren):

```php
$q = 10;   // Schrittweite
$a[1] = 1; // erstes Element
$a[2] = $a[1] * $q; // zweites Element
$a[3] = $a[1] * $q * $q;
$a[3] = $a[1] * pow($q, 2);
$a[3] = $a[2] * $q;
$a[$n] = $a[1] * pow($q, $n - 1); // n-tes Element

function getGeometricItem(int $start, int $step, int $q): int
{
	return $start * pow($q, $step - 1);
}
```



```php
$start = 1;
$current = 36;
$end = 72;
```
