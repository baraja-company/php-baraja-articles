Datentyp Enum-Objekt in PHP
===========================

> id: '5b96765c-16c2-4f76-8e31-6de6e9f59365'
> slug:
> 	cs: datovy-typ-enum
> 	de: type-php
>
> publicationDate: '2022-05-29 15:30:00'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: '43735e926db87d2cc6c538ddb67e0a69'

Ab PHP 8.1 kann der Enum-Datentyp verwendet werden, um exakte Aufzählungswerte für eine Liste zu definieren. Dies ist nützlich für Fälle, in denen wir wissen, dass der Wert einer Variablen nur wenige bestimmte Werte annehmen kann.

So speichere ich zum Beispiel Meldungsarten:

```php
enum OrderNotificationType: string
{
    case Email = 'E-Mail';
    case Sms = 'Text';
}
```

In PHP ist der Datentyp Enum ein klassisches Objekt, das sich wie eine spezielle Art von Konstante verhält, aber auch eine Instanz hat, die weitergegeben werden kann. Im Gegensatz zu einem normalen Objekt unterliegt es jedoch einer Reihe von Einschränkungen.

Unterschiede zwischen Enum und Objekten
-----------------------

Obwohl Enums auf Klassen und Objekten aufbauen, unterstützen sie nicht alle objektbezogenen Funktionen. Insbesondere ist es Enum-Objekten untersagt, einen internen Zustand zu haben (sie müssen immer eine statische Klasse sein).

Eine spezifische Liste von Unterschieden:

- Konstruktoren und Destruktoren sind untersagt.
- Vererbung wird nicht unterstützt. Enums können nicht von einer anderen Klasse erweitert oder geerbt werden.
- Statische oder Objekteigenschaften sind nicht zulässig.
- Das Klonen von bestimmten Enum-Werten (Instanzen) wird nicht unterstützt, jede einzelne Instanz muss eine Singleton-Instanz sein.
- Magische Methoden sind, mit Ausnahme der unten genannten, verboten.

Die folgenden Objekteigenschaften sind verfügbar und verhalten sich wie jedes andere Objekt:

- Öffentliche, private und geschützte Methoden.
- Öffentliche, private und geschützte statische Methoden.
- Öffentliche, private und geschützte Konstanten.
- Enums können eine beliebige Anzahl von Schnittstellen implementieren.
- Attribute können an Enums und Cases angehängt werden. Der Zielfilter `TARGET_CLASS` beinhaltet die Enums selbst. Der Zielfilter `TARGET_CLASS_CONST` schließt Enum-Fälle ein.
- Magische Methoden `__call`, `__callStatic` und `__invoke`.
- Die Konstanten `__CLASS__` und `__FUNCTION__` verhalten sich wie normale Konstanten
- Die magische Konstante `::class` für den Enum-Typ wird als der vollständige Name des Datentyps ausgewertet, einschließlich des Namespace, genau wie bei einem Objekt. Die magische Konstante `::class` auf einer Instanz des Typs `Case` wird auch als Typ Enum ausgewertet, da sie eine Instanz eines anderen Typs ist.

Enum als Datentyp verwenden
-----------------------------

Stellen Sie sich vor, wir haben eine Aufzählung, die die Arten von Anzügen darstellt. In diesem Fall müssen wir nur den Typ "Suit" definieren und die einzelnen gültigen Werte speichern.

Wir erhalten dann eine Instanz der jeweiligen Option klassisch über eine quadratische, wie bei der Arbeit mit einer statischen Konstante.

Beispiel für die Definition einer Enum, deren Aufruf durch einen bestimmten Typ und Übergabe an eine Funktion:

```php
enum Suit
{
	case Hearts;
	case Diamonds;
	case Clubs;
	case Spades;
}

function doStuff(Suit $s)
{
	// ...
}

doStuff(Suit::Spades);
```

Vergleich von zwei Werten
---------------------

Der grundlegende Vorteil von Enums gegenüber Objekten und Konstanten besteht darin, dass sich ihre Werte leicht vergleichen lassen.

Der grundlegende Vergleich, dass wir mit einem bestimmten Wert arbeiten, kann wie folgt durchgeführt werden:

```php
$a = Suit::Spades;
$b = Suit::Spades;

$a === $b; // wahr
```

Sehr oft müssen wir auch entscheiden, ob ein bestimmter Wert in eine gültige Enum-Wert-Aufzählung gehört. Dies lässt sich wie folgt leicht überprüfen:

```php
$a = Suit::Spades;

$a instanceof Suit;  // wahr
```

Lesen des Wertes des Typs
---------------------

Wir können einen bestimmten Typwert entweder als Namen einer aufrufenden Konstante oder direkt als real definierten Wert (falls vorhanden) lesen:

```php
enum Colors
{
	case Red;
	case Blue;
	case Green;

	public function getColor(): string
	{
	    return $this->name;
	}
}

function paintColor(Colors $colors): void
{
	echo "Streichen:" . $colors->getColor();
}
```

Der Wert der aufrufenden Konstante wird über die Eigenschaft "Name" ausgelesen. Wichtig ist auch, dass eine benutzerdefinierte Funktion direkt in den Enum-Datentyp implementiert werden kann, die über jedes Enum aufgerufen werden kann.

Wenn ein bestimmtes Enum auch reale Werte implementiert (die unter jeder Konstanten versteckt sind), kann auch ihr Wert gelesen werden:

```php
enum OrderNotificationType: string
{
    case Email = 'E-Mail';
    case Sms = 'Text';
}

$type = OrderNotificationType::Email;

echo $type->value;
```

Alle gültigen Enum-Werte
-----------------------------

Oft müssen wir (z.B. für den Benutzer in einer Fehlermeldung) alle möglichen Werte auflisten, die Enum annehmen kann. Bei der Verwendung von Konstanten war dies nicht möglich, bei Enum ist dies problemlos möglich:

```php
Suit::cases();
```

Gibt `[Farbe::Herz, Farbe::Karo, Farbe::Kreuz, Farbe::Pik]` zurück.

Überprüfen Sie, ob die Variable vom Typ Enum ist
---------------------------------

Wir können leicht überprüfen, ob eine bestimmte unbekannte Variable eine Enum enthält, indem wir eine Bedingung angeben:

```php
if ($haystack instanceof \BackedEnum) {
```

Jedes Enum-Objekt ist automatisch ein Kind der generischen Schnittstelle "BackedEnum".

Weitere Informationen finden Sie in der Diskussion auf dem PhpStan GitHub: https://github.com/phpstan/phpstan/issues/7304
