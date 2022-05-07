Methoden der PSA und des Input-Transfers
========================================

> id: '843fbfb4-daf2-4c2e-9d94-28d494025b2e'
> slug:
> 	cs: metody-a-predavani-vstupu
> 	de: methoden-der-psa-und-des-input-transfers
> 
> publicationDate: '2020-02-16 20:49:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3e1bca690ed70479ef9807a1f2a1f23

Methoden stellen das Verhalten eines Objekts dar, weil sie es ermöglichen, mit seinem internen Zustand zu arbeiten und Objekte untereinander zu beeinflussen.

Darstellung von Methoden in der realen Welt
----------------------------------

Nehmen wir ein beliebiges reales Objekt, zum Beispiel eine Katze. Die Katze hat bestimmte Eigenschaften (Name, Farbe, Gewicht, ...), die wir mit Eigenschaften beschreiben und dann auch Verhalten (miauen, laufen, schlafen, ...) und das beschreiben wir mit Methoden. Da es viele Katzen auf der Welt gibt ("Und so belle ich wie der Donner, es soll eine Million Katzen geben"), ist es wichtig, sich daran zu erinnern, dass eine Methode etwas Allgemeines ist, das für alle Objekte eines bestimmten Typs gleichermaßen gilt.

Praktische Umsetzung einer Methode
-----------------------------

Was die Sprachsyntax betrifft, so definieren wir eine Klasse für eine Katze:

```php
class Cat
{
    public string $name;

    public string $sound = 'Miau';
}
```

Nachdem wir eine Instanz einer bestimmten Katze erstellt haben, können wir einfach eine Liste erstellen, zum Beispiel den Sound:

```php
$cat = new Cat;

echo $cat->sound; // sagt "Miau"
```

Was aber, wenn wir den Ton auf eine bestimmte Art und Weise formatieren wollen, wenn wir ihn ausschreiben? Dann kommen Methoden ins Spiel:

```php
class Cat
{
    public string $name;

    public string $sound = 'Miau';

    public function getFormattedSound(): string
    {
        return 'Ich mache Geräusche "' . $this->sound . '"!';
    }
}
```

Sie müssen das Prinzip der Funktionen bereits aus der Vergangenheit kennen. Klassen ermöglichen es, eine Funktion direkt in ihren Körper mit definierter Zugänglichkeit (wie bei Eigenschaften) zu schreiben und werden Methoden genannt.

Die Variable "$this" verhält sich ein wenig "magisch". Sie speichert die aktuelle Instanz des Objekts, in dem wir uns gerade befinden. Wenn wir einen Wert aus einer Eigenschaft lesen oder eine andere Methode innerhalb einer Methode aufrufen wollen, müssen wir dies nur über die Variable "$this" tun.

Die Methode wird dann innerhalb des Objekts als klassische Funktion aufgerufen (mit einer Klammer am Ende), um zu verdeutlichen, dass es sich nicht um eine Eigenschaft handelt:

```php
$cat = new Cat;

echo $cat->sound; // sagt "Miau"

echo $cat->getFormattedSound(); // druckt 'Ich mache ein "Miau"-Geräusch!'
```

Konstruktor - Methode, die bei der Erstellung einer Instanz aufgerufen wird
--------------------------------------------------

Bei der Erstellung einer Objektinstanz muss häufig festgelegt werden, wie der Grundzustand des Objekts eingestellt werden soll und welche Parameter (Eingabedaten) obligatorisch sind.

In der OOP gibt es zur Lösung dieses Problems eine spezielle öffentliche Methode `__construct`, die wir freiwillig implementieren können und die immer und nur bei der Erzeugung einer Instanz aufgerufen wird.

Praktisches Beispiel:

```php
class Cat
{
    public string $name;

    public string $sound;

    public function __construct(string $name, string $sound)
    {
        $this->name = $name;
        $this->sound = $sound;
    }

    public function getFormattedSound(): string
    {
        return 'Ich mache Geräusche "' . $this->sound . '"!';
    }
}
```

Durch die Definition des Konstruktors haben wir sichergestellt, dass wir beim Erstellen einer Instanz immer 2 obligatorische Parameter (`Name` und `Sound`) übergeben müssen und das Objekt direkt gesetzt wird.

Da der Konstruktor eine klassische Methode ist, kann er mit den eingefügten Daten etwas anderes tun. Formatieren Sie zum Beispiel die Zeichenfolge entsprechend um, aber dazu später mehr.

Das Erstellen einer Instanz ist dann wieder einfach, wir müssen nur die Anfangsparameter übergeben (sie werden in Klammern zum Klassennamen geschrieben, wo der Klassenname aufgerufen und die Instanz erstellt wird):

```php
$cat = new Cat('Minda', 'Vrr');

echo $cat->name; // Hier steht "Minda"
echo $cat->sound; // druckt "Vrr"

echo $cat->getFormattedSound(); // druckt 'Ich mache ein "Vrr"-Geräusch!'
```

Gettery und Settery
-----------------

Einige Methoden werden "Getter" und "Setter" genannt. Dies sind gängige Methoden, es ist nur eine Konvention, sie so zu nennen. Methoden werden zum Abrufen und Einfügen von Daten in ein Objekt verwendet. Der Hauptvorteil besteht darin, dass wir die Daten vor dem Einfügen validieren und möglicherweise korrigieren oder die Daten zurückweisen und einen Fehler ausgeben können.

Für jede Eigenschaft, die abgerufen werden kann (wir wollen den Datenabruf ermöglichen), ist es üblich, einen Getter zu erstellen, der mit dem Wort "get" beginnt. Wenn die Eigenschaft einen booleschen Wert zurückgibt (`true` oder `false`), ist es üblich, den Anfang des Methodennamens mit dem Wort `is` zu benennen.

Vereinfachtes Beispiel:

```php
class Cat
{
    public string $name;

    public function __construct(string $name)
    {
        $this->setName($name);
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function isEmpty(): bool
    {
        return $this->name === '';
    }

    public function setName(string $name): void
    {
        $this->name = trim($name);
    }
}
```

Bei der Erstellung einer Katzeninstanz übergeben wir ihren Namen an den Konstruktor (was immer obligatorisch ist). Da wir jedoch die Logik zum Einfügen des Namens sowohl für den Konstruktor als auch für den Setter (die Methode zum Einfügen eines neuen Namens) gemeinsam nutzen wollen, rufen wir die Methode `setName()` innerhalb des Konstruktors auf, um sicherzustellen, dass der Name eingefügt wird.

Innerhalb des Setters verwenden wir die <a href="/function-trim">trim()</a>-Funktion, die automatisch Leerzeichen auf beiden Seiten der eingefügten Zeichenkette entfernt.

Wir können die Methode "getName()" für die Ausgabe verwenden. Die Methode "isEmpty()" kann verwendet werden, um zu prüfen, ob eine Leere vorliegt.

> **Praktische Vorteile:**
>
> Ein großer Vorteil von Gettern und Settern sind **garantierte Datentypen**. Wenn ein Getter behauptet, eine Zeichenkette zurückzugeben, können wir uns immer darauf verlassen und erhalten immer eine Zeichenkette.
>
> Ein Setter in seiner Implementierung behauptet auch, einen "String" zu akzeptieren, was für die Anwendung bedeutet, dass die Eingabe des Benutzers entweder in einen "String" konvertiert wird (wenn er einen kompatiblen Typ wie "Integer" verwendet) oder eine Fehlermeldung erhält, dass der Typ nicht konvertiert werden kann (wie "Array").

Der größte Mehrwert von Gettern und Settern wird vor allem in der praktischen Anwendung geschätzt.

Die magische Methode `__toString()`
-----------------------------

Innerhalb einer Klasse können Sie eine magische Methode `__toString()` definieren, die automatisch aufgerufen wird, wenn Sie versuchen, ein Objekt als Zeichenkette auszuschreiben. Die Methode muss immer eine Zeichenkette zurückgeben.

Beispielhafte Umsetzung:

```php
class Cat
{
    public string $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function __toString(): string
    {
        return 'Hallo, ich bin' . $this->name . ';)';
    }
}
```

Die Verwendung ist dann wie folgt:

```php
$cat = new Cat('Minda');

echo $cat; // Es sagt "Hallo, ich bin Minda ;)"
```

Zusammenfassung
-------

Wir haben gezeigt, wie man Methoden definiert und sie dann innerhalb einer Objektinstanz aufruft.

Das nächste Mal werden wir uns mit dem <a href="/kapselung">Kapselungsprinzip</a> beschäftigen, das die Eigenschaften von Methoden voll ausnutzt.
