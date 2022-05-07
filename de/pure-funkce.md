Reine Funktionen in PHP
=======================

> id: d94c18a9-8bc4-4377-8042-e7c8b48320a2
> slug:
> 	cs: pure-funkce
> 	de: reine-funktionen-in-php
> 
> publicationDate: '2021-10-27 10:30:00'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: e47a3507ddb189a218ad8e3a38703e5c

In der funktionalen Programmierung gibt es das Konzept der **reinen Funktion**, das sich auf eine Funktion bezieht, die immer dieselbe Ausgabe auf dieselbe Eingabe zurückgibt (d.h. deterministisch ist) und gleichzeitig keine Nebeneffekte hat (d.h. ihre Umgebung nicht beeinflusst).

So sieht eine reine Funktion aus
----------------------

Beispiel für eine reine Funktion:

```php
// Dies ist eine reine Funktion
function add(int $a, int $b): int
{
	return $a + $b;
}
```

Dies ist eine reine Funktion, da die Ausgabe auf der Grundlage der Eingabeargumente immer die gleiche ist.

Was keine reine Funktion ist
-------------------

```php
// Dies ist eine unsaubere Funktion
function add(int $a, int $b): int
{
	echo 'Hinzufügen...';
	file_put_contents('file.txt', 'Wert:' . $a);
	return $a + $b;
}
```

Diese Art von Funktion ist nicht rein, weil die Funktion das Dateisystem verändert. Eine andere Art von unsauberen Funktionen ist die Interaktion mit der Datenbank, die Ausgabe auf dem Bildschirm usw.
