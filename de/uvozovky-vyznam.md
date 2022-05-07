Besondere Bedeutung der Anführungszeichen
=========================================

> id: '2649942d-94d6-48c3-9c78-5a303165bf72'
> slug:
> 	cs: uvozovky-vyznam
> 	de: besondere-bedeutung-der-anfuehrungszeichen
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c1b63b98a3e9d9c642247c363747616a

In PHP können Sie Anführungszeichen oft durch Apostrophe ersetzen und so den gleichen Effekt erzielen. Manchmal verwenden wir sogar absichtlich eine Kombination aus Anführungszeichen und Apostrophen, um eine andere Ausgabe zu erreichen, ohne dass wir die Escape-Funktion verwenden müssen.

**TLDR: Die Verwendung von Anführungszeichen kann in manchen Kontexten gefährlich sein, verwenden Sie daher stattdessen überall Apostrophe. Anführungszeichen sind nur für Sonderfälle geeignet.**

| Zeichen | Name |
|------|-----------
| "| Anführungszeichen |
| `'` | Apostroph |

Beispiel eins - gleiche Ausgabe
-----------------------------

```php
echo "Hallo, Welt!"; // das ist dasselbe
echo 'Hallo, Welt!'; // wie diese
```

In diesem Fall spielt die Verwendung von Anführungszeichen oder Apostrophen keine Rolle. Oberflächlich betrachtet mag es also so aussehen, als gäbe es keinen Unterschied zwischen Anführungszeichen und Apostrophen.

Kombination von Anführungszeichen und Apostrophen
------------------------------

Betrachten Sie den folgenden Code:

```php
echo 'Meine Lieblingsfarbe ist "rot"!';
```

Wenn ich "Zeilenumbrüche" zur Begrenzung der ausgeschriebenen Zeichenkette verwenden würde, wäre es **mehrdeutig**, wo die Zeichenkette **anfängt** und wo sie endet**, also habe ich "Apostrophe" zur Begrenzung der Zeichenkette verwendet, was dieses Problem löst.

Einfügen von Anführungszeichen in eine durch Anführungszeichen begrenzte Zeichenfolge
---------------------------------------------------

Gelegentlich kann es Situationen geben, in denen ich sowohl "Anführungszeichen" als auch "Apostrophe" in derselben Zeichenfolge aufführen muss. Sie können nicht nur die Verkettung von zwei Zeichenketten verwenden, sondern auch das so genannte **Escaping** von Zeichen.

```php
echo "Diese Zeichenfolge "enthält" Anführungszeichen.";
```

Der Backslash hat in diesem Fall die Bedeutung "genau dieses Zeichen" und daher wird das Anführungszeichen nicht als Ende einer Zeichenkette (oder irgendetwas anderes) angesehen und ist daher immer ein Anführungszeichen. Andere seltsame Zeichen können auf diese Weise gekennzeichnet werden, wenn ihre Haltbarkeit nicht garantiert werden kann und die richtige Bedeutung missverstanden werden könnte.

Variable innerhalb einer Zeichenkette
-----------------------

Anführungszeichen können extrem knifflig sein, da sie es ermöglichen, Variablen direkt in eine Zeichenkette einzufügen:

```php
$color = 'Rot';

echo "Moje oblíbená barva je {$color}, a tvoje?";
```

Viel sicherer sind Apostrophe, bei denen dies nicht möglich ist und man die Zeichenfolge falten muss:

```php
$color = 'Rot';

echo 'Meine Lieblingsfarbe ist' . $color . 'und Ihre?';
```

Gute Gewohnheiten
--------------------------

Im Allgemeinen empfehle ich die Verwendung von Apostrophen (wenn möglich), um irgendetwas abzugrenzen, da sie in Zeichenketten viel seltener vorkommen als einfache Anführungszeichen.

Außerdem ist PHP eine Websprache, d. h. sie wird zur Erstellung von HTML-Dokumenten verwendet, in denen Anführungszeichen sehr häufig vorkommen, weil sie auch zur Erstellung von Teilen des HTML-Codes verwendet werden. Ich persönlich empfehle, sich daran zu gewöhnen, überall Apostrophe zu verwenden, denn dann muss man sich nicht merken, was man einschließt.

Verhalten bei Anführungszeichen
--------------------------

Aufgepasst! Lassen Sie die Anführungszeichen nicht ganz weg! Sie haben einige besondere Vorteile, die für fortgeschrittene PHP-Arbeiten nützlich sein können - Anfänger betrachten sie jedoch als Fehler und verstehen sie nicht.

```php
$x = 10; // eine Variable setzen
echo "Hodnota proměnné je: $x, přesně."; // und eine Auflistung
```

Innerhalb der Anführungszeichen können Sie auch die Werte von Variablen auflisten, oder das Dollarzeichen macht alles, was danach kommt, zu einer Variablen. Wenn Sie also nicht den Wert einer Variablen, sondern das Dollarzeichen ausgeben wollen, müssen Sie escape verwenden.

```php
$price = 25; // Preis in Dollar
echo "Cena produktu: $price\$"; // druckt "Produktpreis: $25"
```

Der Vorteil von Anführungszeichen ist in diesem Fall fraglich, und es ist vielleicht besser, Apostrophe zu verwenden und die Zeichenfolgen einfach zu verbinden.

```php
$price = 25;
echo 'Produktpreis:' . $price . '$'; // druckt dasselbe wie das vorherige Beispiel
```

> **Anmerkung:** Der Preis in Dollar wird korrekt im Format `$25` geschrieben, was noch mehr Verwirrung stiften würde, weil die Notation `$$price` eigentlich etwas aufruft, das `variable Variable` genannt wird (im Variablennamen nennen wir den Namen der aufzurufenden Variable - nur zur Verwirrung).

**TIP:** Das könnte Sie interessieren: <a href="/promenna-variable">Variable</a>.

String-Bezeichnung
--------------------------

Im Allgemeinen wird alles, was in Anführungszeichen oder Apostrophen steht, wie eine Zeichenkette behandelt. So:

```php
$x = 5;   // das ist etwas anderes,
$x = "5"; // als dies.
```

Im ersten Fall wird die **Zahl** 5 in der Variablen **$x** gespeichert, im zweiten Fall wird die **Zeichenkette** "5" in derselben Variablen gespeichert. Glücklicherweise spielt das in PHP keine Rolle, und Sie können mit beiden Varianten (fast) auf die gleiche Weise arbeiten, denn PHP kann Variablen automatisch **umwandeln**, je nach ihrem Inhalt. Es wird jedoch allgemein empfohlen, Zahlen nicht in Anführungszeichen zu schreiben, insbesondere bei Rechenoperationen, bei denen Rundungsfehler auftreten können.

Manchmal möchte ich die Ausgabe einer Funktion in einer Variablen speichern:

```php
$pi = 3.14159;
$zaokrouhleni = round($pi); // dies ist korrekt
$zaokrouhleni = "round($pi)"; // Das ist "falsch" (die Frage ist, welche Ausgabe ich erwarte).
```

Der Fehler im zweiten Fall liegt nur in den Anführungszeichen, wenn die Ausgabe der Funktion **rund()** nicht in der Variablen **$rund** gespeichert wird, sondern in der Zeichenkette, die diese Funktion aufruft.
> **TIP:** Der Wert `$pi` muss nicht direkt in das Skript eingegeben werden, sondern wir können die Funktion `pi()` verwenden, die für komplexere Berechnungen genauer ist.

Besondere Bedeutung von Escaping
--------------------------

> **Warnung:** Die folgenden Beispiele funktionieren nur in Anführungszeichen, wenn Sie sie in Apostrophe einschließen, verhalten sie sich wie normale Zeichen ohne besondere Bedeutung (außer `\'`, bei dem das Apostroph entfällt).
Escaping ist für den Fall gedacht, dass ich ein Sonderzeichen innerhalb von Anführungszeichen oder ein Apostroph ausgeben möchte, das als sprachlicher Ausdruck interpretiert und daher verarbeitet werden könnte, auch wenn der Programmierer dies nicht beabsichtigt hat. Wir haben bereits ein Beispiel gezeigt; in diesem Abschnitt werden mögliche Verhaltensausnahmen beschrieben.

Der Grund dafür ist, dass manchmal die Flucht selbst eine besondere Bedeutung hat. Beispiel:

```php
echo "Langer Text, aufgeteilt in zwei Zeilen.";
```

Das vorherige Beispiel wird gedruckt:

```php
Dlouhý text, rozdělený na
dva řádky.
```

Wenn wir also einen Schrägstrich ausschreiben wollen, müssen wir ihn auch escapen (das `n`-Zeichen zu escapen ist in diesem Fall nicht notwendig, da es wieder als Umbruch verstanden würde, oder wir müssen in diesem Fall überhaupt nicht escapen):

```php
echo "Langer Text, aufgeteilt in zwei Zeilen.";
```

Es gibt noch mehr solcher Sonderzeichen, z.B. "t" ergibt einen Tabulator. Die vollständige Liste finden Sie in der offiziellen Dokumentation.

Echte Verwendung von Sonderzeichen beim Escaping
-----------------------------------------------

Zunächst einmal möchte ich darauf hinweisen, dass fast alles in PHP auf verschiedene Arten erledigt werden kann, und die hier angeführten Beispiele sollen nur veranschaulichen, wie das Problem angegangen werden kann.

Wenn wir zum Beispiel einen Text zeilenweise analysieren wollen, können wir die Funktion <a href="/explode">explode</a> verwenden.

```php
$parser = explode("\n", $retezec); // analysiert den Text Zeile für Zeile
```

In diesem Fall ist die Verwendung des Sonderzeichens "n" sinnvoll, weil wir damit sehr effektiv sagen können, dass wir nach Zeilenumbrüchen analysieren wollen.

```php
$parser = explode('
', $retezec);
```

> **Warnung:** Auf Unix- und Windows-Systemen werden die Zeichen, die das Zeilenende markieren, verwechselt. Windows verwendet zum Beispiel "CRLF" (ein Paar von "r" Zeichen), während Linux nur "LF" (ein einzelnes "n" Zeichen) verwendet. Beim zeilenweisen Parsen sollte dies berücksichtigt werden. Normalerweise wird das Problem gelöst, indem die Zeichen nur auf "LF" normalisiert werden.

Zusammenfassung
-------

Wenn Sie können, verwenden Sie überall **Apostrophe**.

Es ist gut, die Verwendung von Anführungszeichen zu kennen und sie nur dort zu verwenden, wo sie notwendig (oder generell gut) sind. Wenn Sie Text ausgeben, der Anführungszeichen enthalten kann, schließen Sie ihn in Apostrophe ein (die sich dann vorhersehbarer verhalten). Ich persönlich verwende Anführungszeichen, um verschiedene Sonderzeichen auszudrücken, deren Eingabe in Apostrophe unnötig kompliziert ist und eine komplexe Escape-Funktion erfordern würde.
