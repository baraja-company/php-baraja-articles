Ausnahmen und deren Abfangen in PHP
===================================

> id: '61b0f178-bb1c-4166-9e8a-af49de2e2a8c'
> slug:
> 	cs: vyjimky
> 	de: ausnahmen-und-deren-abfangen-in-php
> 
> publicationDate: '2020-02-16 22:18:18'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: d843fe11a092943db429cb8a28384a31

Ausnahmen sind die Werkzeuge der objektorientierten Programmierung, die eine elegante Methode zum Auslösen und Behandeln von Anwendungsfehlern bietet (Behandlung).

Eine Ausnahme wird zunächst ausgelöst (`thrown`), behandelt (`try`) und abgefangen (`catch`). Nur das Werfen ist obligatorisch.

Philosophie der Ausnahmegenehmigung
-------------------------

Vor dem Aufkommen von Ausnahmen war die Fehlerbehandlung in der Programmierung sehr kompliziert, weil wir uns auf die Rückgabewerte von Funktionen verlassen und sie auf unsere eigene Weise abfangen und uns entsprechend verhalten mussten.

In der Tat erzwingen Funktionen selbst keine Fehlerbehandlung, was zu fatalen Problemen führen kann, aber David hat darüber in <a href="https://phpfashion.com/programatori-chyby-neignoruji">Programmierer ignorieren keine Fehler</a> geschrieben.

Ein Beispiel für eine vergessene Fehlerbehandlung:

```php
// Bewegung von Festplatte zu Festplatte
copy('c:/alteDatei', 'd:/neueDatei');
unlink('c:/alteDatei');
// wenn der erste Vorgang fehlschlägt, wird die Datei unwiederbringlich gelöscht
```

Dies liegt daran, dass die korrekte Art und Weise, die Ausgabe der Funktion `copy()` zu behandeln, darin besteht, nicht fortzufahren und einen Fehler zu werfen. Im Falle der guten alten Funktionen könnte es so aussehen:

```php
function backup(): bool
{
   if (copy('c:/alteDatei', 'd:/neueDatei')) {
      return unlink('c:/alteDatei');
   }

   return false;
}
```

Unsere Funktion `backup()` wird nur dann `true` zurückgeben, wenn die Funktion `copy()` nicht fehlgeschlagen ist und die Funktion `unlink()` ` `true` zurückgegeben hat. Andernfalls wird `false` zurückgegeben.

Aber ist das jetzt sicher für die Anwendung? Ist es nicht! Denn jetzt müssen wir die Ausgabe der Funktion `backup()` an dem Punkt behandeln, an dem wir sie aufrufen, und wenn sie fehlschlägt, wissen wir nicht einmal warum. Kurz gesagt, es wird `false` zurückgegeben und wir müssen den Fehler irgendwie selbst erkennen. Ich denke, in diesem Fall ist es gut zu sehen, dass Programmierer oft die Fehlerbehandlung aufgeben oder einfach vergessen, etwas zu behandeln, und die Anwendung deshalb schwer zu erkennende Fehler auslöst.

Die Lösung für dieses Problem ist die Verwendung von Ausnahmen, die die Behandlung erzwingen, und wenn sie nicht behandelt werden, stürzt die Anwendung vollständig ab und wir finden immer heraus, warum.

Grundlegende Definition der Ausnahme
--------------------------

In PHP ist eine Ausnahme eine spezielle Art von Schnittstelle, die von der nativen Klasse `Exception` implementiert wird, die wir verwenden werden.

Wenn die Verarbeitung eines Teils des Programms fehlschlägt, werfen wir einfach eine Ausnahme, die das Problem beschreibt:

```php
if (copy('c:/alteDatei', 'd:/neueDatei') === false) {
   throw new \Exception('Kann Datei "oldfile" nicht kopieren.');
}
```

Das Auslösen einer Ausnahme erfolgt mit dem Schlüsselwort `throw`, gefolgt von der Erzeugung einer Instanz der Klasse mit der Ausnahme. Wir können eine Instanz auch auf andere Weise erhalten (z. B. durch Übergabe aus einer Variablen), und die bloße Erstellung einer Ausnahmeinstanz führt nicht dazu, dass sie ausgelöst wird.

Das erste Argument des Konstruktors der Klasse "Exception" akzeptiert den Text der Ausnahme, der in knapper Form erklären soll, was gerade passiert ist. Es hat sich bewährt, auch Informationen über den durchgeführten Vorgang und einen Verweis auf die Daten anzugeben. Wenn z. B. eine Dateikopie fehlgeschlagen ist, empfiehlt es sich, den Dateinamen zu übergeben. Wenn die Ausführung der SQL-Abfrage fehlschlägt, übergeben wir die ausgeführte Abfrage erneut. Dies wird uns später bei der Behandlung von Fehlern sehr helfen, da wir genau sehen können, wo das Problem liegt.

Behandlung von Ausnahmen
-----------------

Nehmen wir zum Beispiel eine Funktion `backup()`, die Daten sichert und ein Paar Fehler auslösen kann:

```php
function backup(): void
{
   if (copy('c:/alteDatei', 'd:/neueDatei')) {
      if (unlink('c:/alteDatei') === false) {
         throw new \Exception('Kann alte Datei nicht entfernen.');
      }
   }

   throw new \Exception('Sicherungsdateien können nicht kopiert werden.');
}
```

Beachten Sie, dass die Funktion keine Ausgabe zurückgibt und wir in der Definition den Typ "void" angeben. Die Funktion muss nichts zurückgeben, da Erfolg als ein Zustand betrachtet wird, in dem kein Fehler ausgelöst wird, und wir ein positives Szenario nicht behandeln müssen.

Wenn wir die Funktion in einer Anwendung ohne Behandlung verwenden würden, zum Beispiel wie folgt:

```php
echo 'Dateien sichern...';
backup();
echo 'Sicherung abgeschlossen.';
```

Das ist der normale Weg, wie es funktioniert. Wenn jedoch ein Fehler auftritt, wird das Skript automatisch beendet und der Ausnahmetext in der Ausgabe angezeigt. Wichtig ist, dass der Code nicht weiter ausgeführt wird und wir wissen, dass keine Datenbeschädigung auftritt.

Wenn wir die Ausführung fortsetzen wollen, müssen wir den Fehler **bereinigen**, was wir mit Hilfe der Konstrukte `try` und `catch` tun:

```php
echo 'Dateien sichern...';
try {
   backup();
} catch (\Exception $e) {
   echo 'Sicherung fehlgeschlagen:' . $e->getMessage();
}
echo 'Sicherung abgeschlossen.';
```

Wenn eine Ausnahme ausgelöst wird, wird der Code im `catch()`-Bereich (der die Ausnahme akzeptiert, wenn sie mit ihrem Datentyp übereinstimmt) aufgerufen und der interne Code wird ausgeführt.

Wir erhalten immer eine Instanz der Ausnahmeklasse, die z. B. dazu verwendet werden kann, eine Fehlermeldung anzuzeigen, die von der Methode `getMessage()` behandelt wird. Es ist auch nützlich, die Methode `getFile()` zu kennen, die den Festplattenpfad zu der Datei zurückgibt, die den Fehler enthält, `getCode()`, die den Fehlerstatuscode zurückgibt, und `getLine()`, die die Zeilennummer zurückgibt, in der die Ausnahme ausgelöst wurde.

Vorbereitete Ausnahmen
------------------------

Zusätzlich zur grundlegenden Ausnahme "Exception" enthält PHP weitere vordefinierte Ausnahmetypen, die für verschiedene Anwendungsfälle geeignet sind.

| Datentyp | Erläuterung |
|------------|-----------|
| LogicException" | Logischer Fehler, vorhersehbar beim Programmentwurf |
| `BadFunctionCallException` | Funktionsaufruffehler; Funktion nicht gefunden; Aufruf nicht erlaubt |
| `BadMethodCallException` | Methodenaufruffehler |
| `InvalidArgumentException` | Schlechtes (ungültiges) Argument an Funktion oder Methode übergeben |
| `OutOfRangeException` | Wert außerhalb des Bereichs von Array oder Sammlung |
| `LengthException` | Wert überschreitet zulässige Länge |
| "DomainException" | Wert fällt nicht in den erforderlichen Bereich |
| `RuntimeException` | Fehler, der nur zur Laufzeit erkannt werden kann (z.B. Fehler beim Schreiben in eine Datei) |
| `OverflowException` | Überlauf des Puffers oder der arithmetischen Operation, oft verursacht durch die Verarbeitung von mehr Daten als erwartet |
| `UnderflowException` | Unterlauf eines Puffers oder einer arithmetischen Operation, es wurden weniger Daten als erwartet übergeben |
| `OutOfBoundsException` | Index außerhalb des Bereichs von Array oder Sammlung |
| `RangeException` | Wert nicht innerhalb des angeforderten Bereichs |
| `UnexpectedValueException` | Unerwarteter Wert (z.B. Rückgabewert einer Funktion) |

Die Ausnahmen "LogicException" und "RuntimeException" sollten durch einen geeigneten Programmentwurf verhindert werden. Ich persönlich verwende sie nur in Ausnahmesituationen, z. B. wenn ich nicht in eine Datei schreiben kann oder mit einem externen Dienst kommuniziere.

Ich empfehle, `RuntimeException` überhaupt nicht abzufangen und die Anwendung scheitern zu lassen. Dies ist in der Regel ein ernstes Problem, das so schnell wie möglich gemeldet werden sollte.
