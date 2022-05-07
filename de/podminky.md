Bedingungen und Verzweigungen
=============================

> id: '2cea5541-6879-4763-a518-cb21bf9021dd'
> slug:
> 	cs: podminky
> 	de: bedingungen-und-verzweigungen
> 
> publicationDate: '2019-09-07 20:25:57'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: bc4140fc49413be3f2f2b2264b8ea5e6

**Warnung:** Dieser Artikel wurde vor vielen Jahren geschrieben und einige Informationen sind möglicherweise veraltet oder falsch. Bitte beachten Sie dies beim Lesen.

Keine linearen Programme mehr! Das Grundprinzip eines jeden Programms lautet: "Was passiert, wenn....". Eine Bedingung kann als logische Anweisung geschrieben werden, die gültig (die Bedingung ist erfüllt) oder ungültig (dann wird sie nicht ausgeführt oder ihr genaues Gegenteil wird ausgeführt) sein kann. Beide sind leicht zu definieren.

Allgemeine Notation
------------

Im Allgemeinen kann eine Bedingung als logische Aussage geschrieben werden. Die Bedingung kann erfüllt oder nicht erfüllt sein. Es ist ratsam, möglichst beide Optionen zu berücksichtigen. Wenn es mehrere Alternativen gibt, spricht man von einer **geschachtelten Bedingung**.

Beispiel:

```php
if (hodnota   operace   hodnota) {
	// Dies wird ausgelöst, wenn die Bedingung erfüllt ist
} else {
	// Dies wird ausgelöst, wenn die Bedingung nicht zutrifft
}
```

Wir müssen nicht immer beide Optionen definieren (manchmal ist das auch gar nicht nötig). In der Tat können wir die Situation definieren, wenn nur die Bedingung erfüllt ist. Dies geschieht wie folgt:

```php
if (hodnota   operace   hodnota) {
	// Dies wird ausgelöst, wenn die Bedingung erfüllt ist
}
```

Logische Operatoren
--------------------------

| Operator | Bedeutung
|----------|---------
| `==` | Gleich
| `===` | Ist gleich und hat denselben Datentyp (*alles kann mit allem verglichen werden, aber die Bedingung ist nur erfüllt, wenn es sich um einen Wert desselben Datentyps handelt (z. B. Zahl, Text, ...)*)
| `!=` | Ist nicht gleich
| `<=` | Gleich oder größer als
| `>=` | Gleich oder kleiner als
| `<` | Größer
|>` | Weniger

Reales Beispiel
--------------------------

```php
$a = 5;
$b = 3;
if ($a === $b) {
	// Block, der gedruckt wird, wenn $a gleich $b ist
} else {
	// Block, der gedruckt wird, wenn $a NICHT gleich $b ist
}
```

Verschachtelte Bedingungen
--------------------------

Leider ist die Ausgabe nur `true` (gültig) und `false` (ungültig). Wenn wir also mehrere Möglichkeiten in Betracht ziehen wollen, müssen wir mehrere Bedingungen ineinander verschachteln. Dies wird als **verschachtelte Bedingung** bezeichnet. Sie ist verschachtelt, weil eine der Lösungen der Bedingung nur eine weitere Bedingung ist.

```php
$a = 5;         // linke Tasche
$b = 3;         // rechte Tasche
$kapsa = true;  // Habe ich eine Tasche?

if ($kapsa === true) {

	if ($a > $b) {
		echo 'In der linken Tasche befindet sich mehr';
	} else {
		echo 'In der rechten Tasche befindet sich mehr';
	}

} else {
	echo 'Sie haben keine Tasche';
}
```
