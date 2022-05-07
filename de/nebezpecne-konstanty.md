Unsichere Verwendung von Konstanten in PHP
==========================================

> id: b4d0f45f-c059-4a47-9b63-8980d0d465ed
> slug:
> 	cs: nebezpecne-konstanty
> 	de: unsichere-verwendung-von-konstanten-in-php
> 
> publicationDate: '2021-06-22 10:49:46'
> mainCategoryId: '561b0614-e152-4a62-a634-1f2d605d39d9'
> sourceContentHash: '58d4ea2e4bf106402386506b023ba4d2'

Bei der Verwendung von Konstanten in PHP gibt es zwei knifflige Dinge zu beachten.

Dynamische und statische Konstanten
------------------------------

Eine Konstante kann in PHP entweder statisch direkt in der Klasse definiert werden (die beste Lösung), zum Beispiel wie folgt:

```php
class Region
{
	public const PREFIX = 420;
}
```

Und der Nutzen ist ganz klar. Bei der Kompilierung der Klasse wird der Wert der Konstante festgelegt und wir können darauf zugreifen, indem wir den Klassennamen und die Konstante selbst aufrufen. Meistens, indem man `Region::PREFIX` schreibt.

Die andere (viel schlechtere) Möglichkeit besteht darin, die Konstante dynamisch zur Laufzeit zu definieren (meist irgendwo in einem Konfigurationsskript), wo sie dann etwa so lautet:

```php
define('BASE_DIR', __DIR__ . '/../');
```

Der größte Nachteil der Definition einer Konstante über die Funktion `define` ist, dass das Skript, das die Konstante definiert, möglicherweise nicht aufgerufen wurde, so dass die Konstante nicht existiert, wenn Sie versuchen, sie zu lesen.

In Verbindung mit der Verwendung einer dynamischen Konstante innerhalb der Definition einer statischen Konstante in einer Klasse kann dies sogar zu einem fatalen Reflexionsfehler führen:

```php
class InvoiceGenerator
{
	// Das ist völlig falsch!
	public const DATA_DIR = BASE_DIR . '/Daten/Rechnung';
}
```

> **Erläuterung:**
>
> Die Verwendung einer dynamischen Konstante innerhalb einer statischen Konstante hat den großen Nachteil, dass der Wert der dynamischen Konstante zur Compiletime nicht gelesen werden kann. Dieses Skript muss daher bei jeder Anfrage erneut verarbeitet werden (d.h. es kann nicht zur Geschwindigkeitsoptimierung im OPCache gespeichert werden), und wenn die Konstante überhaupt nicht existiert, wird ein fataler Kompilierfehler ausgelöst und die Anwendung kann überhaupt nicht ausgeführt werden.

Wenn Sie PhpStan verwenden, kann es Sie automatisch vor diesem Problem warnen:

```txt
Reflection error: Could not locate constant "BASE_DIR" while
evaluating expression in InvoiceGenerator at line 6
```

> **Lernen:**
>
> Der Wert aller Konstanten sollte immer konstant sein.


Vererbung von Konstanten bei Verwendung von statischen
-------------------------------------

In manchen Fällen ist es sinnvoll, den Wert einer Konstante durch Vererbung zu überschreiben. Aber in diesem Fall kann der Vorfahre den Wert des Nachfahren nicht lesen (oder sollte es nicht).

Ein Beispiel dafür ist die Definition von Ländern und Regionen:

```php
abstract class Region
{
	public function getPrefix(): int
	{
		// Fataler Fehler!
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

Das Paradoxe ist, dass der obige Code nicht notwendigerweise einen Fehler auslöst, aber er kann durch unangemessene Verwendung der Vererbung ausgelöst werden.

Wenn wir die Methode `getPrefix()` bei einem Nachkommen von `CzechRepublic` aufrufen, ist alles korrekt, da der Wert der Konstante richtig gelesen wird. Wenn jedoch der Nachkomme den Wert der Konstante nicht gesetzt hat, wird ein fataler Fehler bei nicht vorhandener Konstante ausgelöst. Das Schlimmste an der ganzen Sache ist, dass es sich um eine **versteckte Abhängigkeit** handelt, die in der Implementierung der Methode entsteht, und der Entwickler, der die Klasse erbt, weiß möglicherweise nicht einmal von dieser Abhängigkeit.

Die beste Lösung in diesem Fall ist, entweder eine Konstante direkt im Vorgänger mit einem Standardwert zu definieren (so dass die Logik immer funktioniert) oder zumindest eine Ausnahme im Getter auszulösen.

```php
abstract class Region
{
	public const REGION = null;

	public function getPrefix(): int
	{
		if (static::REGION === null) {
			throw new \LogicException('Die Region wurde nicht definiert.');
		}
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

PhpStan antwortet auf diesen Fehler wie folgt:

```txt
Access to undefined constant static(Region):REGION.
```
