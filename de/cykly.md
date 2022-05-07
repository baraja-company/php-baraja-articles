Zyklen und ihre Typen in PHP
============================

> id: e2927790-d0fb-4de5-9848-01fdd088464b
> slug:
> 	cs: cykly
> 	de: zyklen-und-ihre-typen-in-php
> 
> perex:
> 	- 'Cykl for, while, foreach a rekurze v PHP + vysvětlení rozdílů mezi cykly. Možnosti iterativního zpracování úloh.'
> 	- 'Zyklus for, while, foreach und Rekursion in PHP + Erklärung der Unterschiede zwischen den Zyklen. Möglichkeiten der iterativen Bearbeitung von Aufgaben.'
> 
> publicationDate: '2020-04-11 18:56:34'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '297931d6e7b8c9e011dbbe06b414cdde'

Schleifen werden im Allgemeinen verwendet, um denselben Codeabschnitt zu wiederholen (in der Regel über einen Datensatz). Für jeden Aufgabentyp eignet sich ein anderer Schleifentyp, der sich hauptsächlich in Bedeutung und Syntax unterscheidet. Es ist auch wichtig zu beachten, dass alle Arten von Schleifen untereinander konvertierbar sind, es lohnt sich nur nicht immer in Bezug auf die Komplexität des Codes und die Berechnungszeit.

Die grundsätzliche Einteilung der Schleifen nach der Art der Elemente:

- `for`: Eine allgemeine Schleife, bei der die Anzahl der Wiederholungen im Voraus bekannt ist oder einfach im Voraus berechnet werden kann,
- `while` und `until-while`: Eine allgemeine Schleife, bei der wir die Anzahl der Wiederholungen nicht im Voraus kennen,
- `foreach`: Durchsuchen von Arrays und Objekten,
- Rekursion": Zerlegung eines komplexen Problems in eine Reihe kleinerer Probleme.

Allgemeine Eigenschaften von Schleifen
-----------------------

Zyklen funktionieren im Allgemeinen so, dass ein markiertes Codestück **immer wieder** wiederholt wird, **solange die Abbruchbedingung** erfüllt ist. Die Schleife wird entweder gestoppt, wenn die Abbruchbedingung nicht erfüllt wird, oder durch manuellen Abbruch durch den Aufruf von `break;`.

Wenn die Schleife nicht gestoppt wird, kann sie unbegrenzt weiterlaufen, bis die Anwendung manuell beendet wird oder das Zeitlimit für die Verarbeitung des PHP-Skripts abläuft (normalerweise 30 Sekunden).

**Wichtige Begriffe:**

- Zyklus" - ein Sprachausdruck, mit dem Sie einen bestimmten Teil des Codes wiederholen können
- Iteration" - eine bestimmte Ausführung des Schleifenkörpers
- Initialisierung" - Beginn der Schleifenausführung (bevor die erste Iteration beginnt)
- Inkrement" - Erhöhen einer Iterationsvariablen um eins
- Ausstiegsbedingung" - Eine Bedingung, die bei jeder Iteration überprüft wird (der Ort der Überprüfung variiert je nach Schleifentyp). Die Schleife läuft so lange, wie die Bedingung erfüllt ist

For-Schleife
----------

Die Option "Für" ist in Fällen nützlich, in denen die Anzahl der Wiederholungen im Voraus bekannt ist (oder leicht berechnet werden kann). Sie eignet sich zum Beispiel perfekt für die lineare Durchquerung von Intervallen.

Sie ist in der Regel in 3 Teile gegliedert:

```php
for (inicializace; výraz; inkrementace) {
    // der Hauptteil des Zyklus
}
```

- Initialisierung": Definition des Anfangszustands der Schleife (welche Variablen wir brauchen),
- Ausdruck": Bedingung. Solange sie gültig ist, wird die Schleife ausgeführt,
- `increment`: Ändert den Zustand der Schleife vom vorherigen Durchlauf (`increment` - um eins erhöhen).

Ein Beispiel wäre die Ausgabe einer Reihe von Zahlen von "0" bis "10":

```php
for ($i = 0; $i <= 10; $i++) {
    echo $i . '<br>';
}
```

Oder Vielfache von zwei von "2" bis "100":

```php
for ($i = 2; $i <= 100; $i += 2) {
    echo $i . '<br>';
}
```

Die for-Schleife ist für verschiedene Tricks nützlich, z. B. <a href="/abeceda-cisla-intervals">Abfrage des Alphabets, eines Arrays von Zahlen und Intervallen</a>.

Während einer "do-while"-Schleife
-----------------------

Eine "While"-Schleife läuft so lange, wie die Bedingung erfüllt ist. Der Unterschied zwischen `while` und `do-while` besteht darin, wann die Bedingung ausgewertet wird.

```php
while (výraz) {
    // der Hauptteil des Zyklus
}
```

Alternativ dazu:

```php
do {
    // der Hauptteil des Zyklus
} while(výraz);
```

- Das "while" wertet zunächst die Bedingung aus und führt dann die innere Schleife aus,
- do-while" verarbeitet zunächst die innere Schleife und wertet dann die Bedingung aus.

Dies ist besonders vorteilhaft, wenn wir nicht im Voraus wissen können, ob die Schleife jemals laufen wird und wir garantieren wollen, dass sie immer mindestens einmal läuft (dann verwenden wir `do-while`).

Beispiel:

> Wir haben eine Zahl in der Variablen "$x = 2" gespeichert. Wir wollen auch eine Zahl im Intervall `<0; 10>` in der Variablen `$y` erzeugen, so dass sie sich von der Zahl in der Variablen `$x` unterscheidet. Einfach gesagt: Erzeugen Sie eine Zufallszahl zwischen `0` und `10`, die nicht `2` ist.

In diesem Fall ist es praktisch, eine "Do-While"-Schleife zu verwenden. Wir wissen nicht im Voraus, wie oft die Schleife laufen muss (vielleicht haben wir Pech und treffen immer wieder auf dieselbe Zahl, dann läuft die Schleife sehr lange), und wir wollen auch sicherstellen, dass sie mindestens einmal läuft, bevor die Bedingung ausgewertet wird, da wir die Zahl erst generieren müssen.

```php
$x = 2;
$y = $x;

do {
   $y = rand(0, 10);
} while($x == $y);

echo 'X :' . $x . '<br>';
echo 'Y:' . $y;
```

Foreach
-------

foreach" ist ideal zum Durchsuchen von Feldern und Objekten. Wir können nicht nur bestimmte Werte, sondern auch Schlüssel abrufen.

Wenn wir nur bestimmte Werte erhalten wollen:

```php
foreach ($data as $value) {
    // Datenverarbeitung
}
```

Alternativ können wir auch die Schlüssel besorgen:

```php
foreach ($data as $key => $value) {
    // Datenverarbeitung
}
```

foreach" eignet sich hervorragend zum Durchsuchen echter Daten - zum Beispiel Ergebnisse aus einer Datenbank oder Felder mit Schlüsseln:

```php
$countries = [
    'en' => 'Tschechische Republik',
    'Siehe' => 'Slowakei',
];

foreach ($countries as $shortcut => $name) {
    echo $shortcut . ':' . $name . '<br>';
}
```

Rekursion
-------

Eine Rekursion liegt vor, wenn eine Funktion oder Methode sich selbst aufruft. Sie dient dazu, ein komplexes Problem in eine Gruppe kleinerer Probleme zu zerlegen.

> Das Verständnis der Rekursion kann für Anfänger schwierig sein, da sie auf der Idee der Weitergabe von Verantwortung beruht.
>
> Die Funktion sagt tatsächlich etwas in der Art von "Ich kann dieses Problem nicht lösen, aber ich kenne jemanden, der es kann...", also ruft sie ihn an, er ruft jemanden an, ... bis das letzte Glied aufgerufen wird, das das Problem löst.

Es ist sicherlich erwähnenswert, dass jeder rekursive Algorithmus umgeschrieben werden kann, um Schleifen zu verwenden, wo keine Rekursion benötigt wird (das gilt auch umgekehrt). Rekursion ist ein guter Diener, aber ein schlechter Meister. Damit lassen sich bestimmte Probleme einfach und sehr effizient lösen, während das Durchlaufen von Zyklen für andere Dinge nützlich ist.

Im Allgemeinen ist die Rekursion speicherintensiv, da sie ständig neue Instanzen und Kontexte von Funktionen und Methoden erzeugt. Für das Traversieren von Baumstrukturen beispielsweise ist dies jedoch der einzig sinnvolle Weg, um das Problem zu lösen.

Beispiel:

Wir müssen die Fakultät der Zahl `5` berechnen, so wird es in der Mathematik gemacht:

`5! = 5 * 4 * 3 * 2 * 1`

Das heißt, es gibt eine sukzessive Multiplikation einer Reihe von Zahlen von "1" bis zu der Zahl, deren Faktor wir interessiert sind.

Rekursiv kann dies sehr elegant gelöst werden:

```php
function factorial(int $n): int
{
   if ($n === 0) {
      return 1;
   }

   return $n * factorial($n - 1);
}
```

Alternativ ist eine noch kürzere Implementierung mit dem ternären Operator möglich:

```php
function factorial(int $n): int
{
    return $n === 0 ? 1 : $n * factorial($n - 1);
}
```

Das Gleiche kann auch für die Verwendung von Zyklen umgeschrieben werden:

```php
function factorial(int $n): int
{
    $result = 1;

    for ($i = $n; $i > 0; $i--) {
        $result *= $i;
    }

    return $result;
 }
```

Das Schreiben mit einem Zyklus dauert etwas länger, hat aber wiederum einen deutlich geringeren Speicherbedarf. Wir verwenden nur eine Variable `$result` für die Berechnung, wobei wir den Wert ständig ändern und keine neuen Instanzen von `factorial()` erzeugen müssen.

Schreiben Sie Endlosschleifen sehr sorgfältig
-----------------------------------

Es kann sehr leicht passieren, dass eine Schleife unendlich wird (wir erreichen dies, indem wir die Abbruchbedingung nie erfüllen). Sie sollten dies berücksichtigen und die Schleife eventuell selbst mit `break;` beenden.

Würfeln Sie zum Beispiel so lange, bis die Zahl "6" gewürfelt wird. Bei der Implementierung ist man versucht, die "while"-Schleife zu verwenden:

```php
while (true) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'Der Wert ist gesunken' . $value;
      break;
   }
}
```

Indem wir `while(true)` schreiben, haben wir eine unendliche Anzahl von Wiederholungen sichergestellt, weil `true` immer wahr sein wird. Wir müssen also die Schleife selbst mit dem Konstrukt "break" stoppen, aber was ist, wenn der Wert "6" nie fällt und wir die Schleife nie stoppen?

Ich persönlich zähle immer die Anzahl der Wiederholungen, und wenn sie eine kritische Grenze überschreitet, breche ich die Schleife zwangsweise ab. Zum Beispiel, wenn wir mehr als "1000" Versuche haben. In diesem Fall ist es besser, "for" statt "while" zu verwenden und die Anzahl der Durchläufe zu zählen.

Wenn wir die Anzahl der Durchläufe überschreiten, ist es höflich, den Entwickler zu informieren, indem eine Ausnahme ausgelöst wird.

```php
for ($i = 0; $i <= 1000; $i++) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'Der Wert ist gesunken' . $value;
      break;
   }
}

if ($i === 1000) {
   throw new \Exception('Die maximale Anzahl von Würfen wurde überschritten.');
}
```
