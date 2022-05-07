Bedingungen in PHP - IF() {...} - Verzweigungsoptionen
======================================================

> id: '3e4b81bb-a8bb-4e99-8842-e76f1658a371'
> slug:
> 	cs: if
> 	de: bedingungen-in-php---if-...---verzweigungsoptionen
> 
> perex:
> 	- 'Podmínky, logické výrazy, booleovská logika a možnosti větvení PHP scriptů. Vyhodnocování logických výrazů a operátory. Možnosti skládání výrazu.'
> 	- 'Bedingungen, boolesche Logik, boolesche Logik und PHP-Skriptverzweigungsoptionen. Boolesche Ausdrücke und Operatoren auswerten. Optionen zur Ausdrucksfaltung.'
> 
> publicationDate: '2019-11-26 11:55:09'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '759a244b4fe2dc119c475aee42259578'

> **Hinweis:** Dieser Artikel könnte für einige Anfänger etwas unübersichtlich sein, da er ein grundlegendes Verständnis von PHP voraussetzt. Wenn Sie sich dafür interessieren, wie Bedingungen funktionieren, lesen Sie <a href="/bedingungen">über Bedingungen im Einsteigerkurs</a>.

| Unterstützung: | Alle Versionen: PHP 4, PHP 5, PHP 7
|-------------------|----------|
| Kurzbeschreibung: | Validierung von einer oder mehreren Aussagen
| Typ: <a href="/statements-and-functions">Statement, Konstrukt</a> (keine Funktion)

Beschreibung
-----

Oft muss man feststellen, ob eine Gleichheit gilt oder ob eine Aussage wahr ist, dafür sind Bedingungen da. PHP verwendet die folgende Syntax wie viele andere Sprachen (insbesondere C):

```php
if (logický výrok) {
    // konstruieren
}
```

Jeder logische Ausdruck hat einen Wert von `TRUE` (wahr) oder `FALSE` (falsch), es gibt keine anderen Optionen.

Beispiel für den Vergleich, ob die Variable "$x" größer ist als die Variable "$y":

```php
$x = 10;
$y = 5;

if ($x > $y) {
    // Dieser Teil des Skripts wird ausgeführt, wenn die Bedingung erfüllt ist
} else {
    // Dieser Teil des Skripts wird ausgeführt, wenn die Bedingung nicht zutrifft
}
```

Das Bedingungskonstrukt hat einen obligatorischen Inhalt in runden Klammern, in dem der zu prüfende Ausdruck angegeben wird, zusammengesetzt aus Operatoren (Übersicht unten), mehrere Ausdrücke können mit logischen Operatoren verknüpft werden (Übersicht unten).

Darüber hinaus enthält die Bedingung zwei optionale Anweisungsblöcke.

- Wenn die Bedingung erfüllt ist
- Wenn die Bedingung nicht erfüllt ist

Aus praktischen Gründen sollte immer mindestens der erste Anweisungsblock enthalten sein, wenn die Bedingung wahr ist, da sonst die Prüfung des Ausdrucks keinen Sinn macht.

Verwendung von Semikolon und Klammern
--------------------------

Generell:
- **Runde** Klammern werden verwendet, um logische Ausdrücke zu trennen (andere runde Klammern können eingesetzt werden, um komplexere Ausdrücke zu erhalten).
- Die **komplexe** Klammer wird verwendet, um einen Block von Befehlen und Funktionen abzugrenzen.
- Die **Mitte** wird nicht verwendet, um eine Bedingung anzugeben (der Block von Befehlen wird durch eine zusammengesetzte Klammer abgegrenzt), sondern um die einzelnen Befehle innerhalb der Bedingung zu trennen).

Die einzig mögliche Schreibweise mit einem Semikolon (außer bei Verwendung des **endif**-Konstrukts):

```php
if ($x > $y);
```

Eine solche Bedingung ist jedoch sinnlos, da in beiden Fällen das Ergebnis des Vergleichs verworfen wird und keine zur Bedingung gehörende Anweisung ausgeführt wird.

Alternative Schreibweise
--------------------------

Unter bestimmten Umständen kann die "if"-Konstruktion unter Weglassung der zusammengesetzten Klammern verwendet werden. Dies kann nur in den folgenden Fällen erreicht werden:

- Wenn wir nur eine Anweisung in der Bedingung ausführen.
- Wenn wir einen Doppelpunkt und ein endif anstelle von zusammengesetzten Klammern verwenden;
- Wenn wir die "in-line" Notation verwenden.

Ausführlichere Informationen finden Sie in den folgenden 3 Kapiteln.

> **`1. nur ein Befehl ~ abgekürzte Syntax`**

Wenn Sie eine Bedingung erstellen, in der Sie nur ein Konstrukt (Anweisung) ausführen wollen, können Sie entweder die klassische Klammerschreibweise verwenden:

```php
if ($x > 10) { $y = $x; }
```

Oder wir können die Klammern weglassen:

```php
if ($x > 10) $y = $x;
```

Dieses Verhalten gilt jedoch nur für einen Befehl in der unmittelbaren Umgebung der Bedingung.

Ein besseres Beispiel (nur das Konstrukt `$y = $x` wird bedingt ausgeführt, der Rest wird immer ausgeführt):

```php
$x = 5;
$y = 3;
$z = 10;
if ($x > $y)
$y = $x;
$x = 3;
```

> **`2. Doppelpunkt und endif;`**

```php
if (výraz):
    konstrukt;
    konstrukt;
    konstrukt;
endif;
```

Diese Notation gilt jedoch seit langem als veraltet, da sie an Orientierung verliert, wenn mehrere Bedingungen ineinander übergehen.

> **Anmerkung:** Ich möchte anmerken, dass dieser Stil auch von einigen Leuten wie Yuh (<a href="https://www.jakpsatweb.cz/php/php-tahak.html#vetveni">Siehe seinen Artikel</a>) geschätzt wird. Gott bewahre, dass Sie das irgendwo verwenden.

> **`3. Ternärer Ausdruck ~ einzeilige "in-line"-Notation`**

Gelegentlich ist es sinnvoll, einen einfachen Inline-Vergleich mit einer anderen Aktion durchzuführen (z. B. zusammen mit der Definition einer neuen Variablen). Wenn nur eine Anweisung ausgeführt werden soll, kann die gesamte Prozedur auf eine einzige Zeile reduziert werden, auch wenn sie so einfach wie möglich gehalten wird.

```php
$x = 5;
$xJeVetsiNezDva = ($x > 2 ? TRUE : FALSE);

// oder noch kürzer:

$xJeVetsiNezDva = ($x > 2);
```

Operator-Tabellen
-----------------

Innerhalb der Bedingung werden zwei Arten von Operatoren verwendet:

- **Komparativ** ~ sie vergleichen eine bestimmte Beziehung,
- **Logisch** ~ Kombinieren Sie mehrere Ausdrücke, um komplexe Bedingungen zu erstellen.

Vergleichende Operatoren
-----------------------

| Operator | Bedeutung |
|----------|----------|
| `==` | Gleich
| `===` | Ist gleich und hat den gleichen Datentyp
| `!=` | Ist nicht gleich
| `>=` | ist gleich oder größer
| `<=` | ist gleich oder kleiner
|>` | Größer
| `<` | Weniger

Beispiel (gültig, wenn "$x nicht 5" ist):

```php
if ($x != 5) { ... }
```

Logische Operatoren
--------------------

| Operator | Alternative | Bedeutung | Wahr, wenn:
|-----------|--------------|----------------|--------------------------------------------
| `&&` | AND | und gleichzeitig | beide Werte sind wahr
| `||` | OR | oder | mindestens ein Wert ist wahr
| `^^` | XOR | exklusives ODER | mindestens eines ist wahr oder falsch, aber nie beides
| `!` | *nicht* | Negation des Ausdrucks | `wahr` wenn `falsch` und umgekehrt
| Die Negation eines Ausdrucks hängt von den jeweiligen Umständen ab.

Ein komplexeres Beispiel:

```php
$x = 5;
$y = 3;
$z = 8;
if ($x > 0 && !($y != 2 && $z == $x) || $z > $y) { ... }
```

Weglassen von logischen und Vergleichsoperatoren
---------------------------------------------

Oft können wir es uns leisten, einen der beiden Operatoren (oder sogar beide) wegzulassen, aber wir dürfen nie die Regeln der richtigen Verwendung vergessen, damit der resultierende Ausdruck funktioniert.

Im Allgemeinen wird beim Testen eines Ausdrucks ohne Operator geprüft, ob sein Wert `TRUE` oder nicht leer ist (z.B. ob er eine Zahl ungleich Null, eine nicht leere Zeichenkette, ... enthält).

Beispiele:

```php
$x = 5;
$y = 3;
$z = 8;

if ($x) { ... }         // funktioniert, weil $x nicht leer ist
if ($x && $y) { ... }   // funktioniert, da $x und $y nicht leer sind
if (!$x) { ... }        // schlägt fehl, weil TRUE negiert wird
if (isset($z)) { ... }  // funktioniert, weil die Variable $z existiert
```

Aber es können knifflige Situationen entstehen, insbesondere wenn:
- Ich frage nach `if ($x)` und die Variable `$x` enthält Null (`0`), dann ist die Bedingung nicht erfüllt.
- Oder die Variable `$x` enthält die Zeichenkette ``0`` (die Zahl Null), weil sie zu Null überläuft und der Ausdruck daher nicht wahr ist.
- Es gibt eine Funktion in der Bedingung, die immer eine nicht leere Zeichenkette zurückgibt, denn dann ist die Bedingung immer wahr.
- Oder wenn wir die Eingaben des Benutzers überprüfen und er `'false'` als Zeichenkette zurückgibt, dann ist die Bedingung wiederum wahr, weil die Zeichenkette nicht leer ist.

Ich empfehle eine einfache und effektive Lösung für dieses Problem: Fragen Sie nach der Anzahl der zurückgegebenen Zeichen. Wenn die Zeichenkette leer ist (oder die Variable nicht existiert), werden null Zeichen zurückgegeben und die Bedingung ist nicht erfüllt. Einfaches Beispiel:

```php
$x = '0';
if ($x) { ... }			// Bedingung trifft normalerweise nicht zu
if (strlen($x)) { ... }	// Bedingung ist gültig, da $x 1 Zeichen enthält
```

Als Nächstes können wir mit der Funktion `isset()` testen, ob eine Variable existiert.

String-Vergleich
-----------------

Es ist leicht herauszufinden, dass die Saiten identisch sind:

```php
$a = 'Katze';
$b = 'Katze';

if ($a === $b) {
    // Wenn die Zeichenketten gleich sind
} else {
    // Wenn die Zeichenketten unterschiedlich sind
}
```

Es ist wichtig, die Datentypen genau im Auge zu behalten, falls der Eintrag mit einem anderen gleichzusetzen ist.

Zum Beispiel ist die leere Zeichenkette `$a = '';` anders als die Zeichenkette `NULL`: `$b = NULL;`. Diese Unterscheidung ist z. B. bei Datenbanken notwendig, wo es einen Unterschied macht, ob ein Wert nicht vorhanden oder leer ist.

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

Es ist auch eine gute Idee, weiße (unsichtbare) Zeichen wie Leerzeichen, Tabulatoren und Zeilenumbrüche beim Vergleich von Zeichenketten zu ignorieren. Dies ist z. B. nützlich, wenn Sie ein Kennwort eingeben und es an eine Hash-Funktion weitergeben:

```php
$password = '81dc9bdb52d04dc20036dbd8313ed055'; // 1234
$userPassword = '1234';

if (md5(trim($userPassword)) === $password) {
    // Die Funktion trim() löscht automatisch Leerzeichen.
}
```

Unbekannter (nicht vorhandener) Wert?
--------------------------

Manchmal kann es vorkommen, dass der Wert nicht existiert (er ist weder `TRUE` noch `FALSE`), es handelt sich hauptsächlich um einen Wert, der aus der Datenbank bezogen wird (zum Beispiel fragen wir nach einer Spalte, die nicht existiert), in diesem Fall wird der Datentyp `NULL` zurückgegeben.

Im Allgemeinen wird `NULL` als `FALSE` ausgewertet, d.h. die Bedingung trifft nicht zu. Dieses Verhalten ist jedoch nicht immer zweckmäßig, da ein nicht vorhandener Wert nicht unbedingt bedeutet, dass es keinen Datensatz gibt.

> Beispiel aus der Praxis: Wir haben ein Benutzerprofil und fragen die Webseite des Benutzers ab. Nicht alle Benutzer müssen eine Webseite haben, daher wird in diesem Fall `NULL` zurückgegeben, aber der Benutzer existiert noch. In diesem Fall sollten wir also lieber die Funktion `isset()` verwenden, um auf das (Nicht-)Vorhandensein der Variablen zu testen und nicht auf der Grundlage eines bestimmten Wertes eine Schlussfolgerung zu ziehen.
