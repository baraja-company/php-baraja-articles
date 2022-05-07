Wie man PHP-Code versteht
=========================

> id: d8d452da-37b4-4502-a573-a27afe7ea37a
> slug:
> 	cs: jak-se-vyznat
> 	de: wie-man-php-code-versteht
> 
> publicationDate: '2020-04-11 18:45:23'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c3012a19e6dd01770fad9ab456a5b6b3

Die meisten Sprachen können auf verschiedene Arten geschrieben werden, mit dem gleichen Ergebnis. Wenn Sie den Code einmal geschrieben haben, werden Sie ihn wahrscheinlich irgendwann einmal lesen und korrigieren oder neue Funktionen hinzufügen.

Um zu vermeiden, dass man die ganze Zeit über den Code nachdenken muss, und um sich gut darin zurechtzufinden, gibt es eine Reihe von Werkzeugen und Möglichkeiten, den Code direkt in PHP "richtig" zu schreiben, oder den Code so zu erstellen, dass seine zukünftige Lesbarkeit (auch durch einen anderen Menschen) direkt unterstützt wird.

> **Anmerkung des Autors:**
>
> Die Erfahrung zeigt, dass Code so schnell veraltet, dass sogar der Autor der Anwendung selbst seinen eigenen Code nach einem halben Jahr als fremd empfindet. Wenn wir es also von Anfang an richtig schreiben, wird es seine zukünftige Erweiterbarkeit nicht verhindern.
>
> In der realen Entwicklung hat sich heimlich gezeigt, dass eine einheitliche Codeformatierung und die Einführung von Regeln im Allgemeinen eine Reihe von Fehlern verhindert.

TL;DR
-----

Die Lesbarkeit von Code hängt oft mit Formatierungs- und Schreibregeln zusammen.

In Entwicklungsteams ist es sinnvoll, formale Regeln für die Formatierung und Pflege von Code aufzustellen.

Ich persönlich verwende (im Jahr 2022) den Kodierungsstandard für das Nette-Framework, und die Regeln werden bei jeder Übertragung automatisch ausgewertet. Weitere Informationen finden Sie im Artikel [GitHub CI verwenden] (https://php.baraja.cz/github-actions-nejlepsi-ci-pro-rok-2021#hotove-priklady).

Die Installation des Codierungsstandardtests und seine Ausführung erfolgt mit zwei Befehlen:

```shell
composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
php temp/coding-standard/ecs check src
```

Hinweise im Code
---------------

Die Hinweise haben keinen Einfluss auf die Verarbeitung des Codes und sind nur für den Programmierer gedacht. Bei umfangreicheren, vollständigeren Code-Stücken ist es wichtig, eine Notiz zu schreiben, in der erklärt wird, wozu der Code dient und wie er im Prinzip funktioniert.

```php
// Definitionen der Variablen
$a = 5;
$b = 3;
$c = 2;

// Summe aller Zahlen
$sum = $a + $b + $c;

// Auflistung für Benutzer
echo $sum;
```

Die Notiz beginnt mit einem Paar Schrägstriche (`//`) und ist bis zum Ende der Zeile gültig. Es kann überall eingesetzt werden.

In dem Vermerk sollte nicht die konkrete Umsetzung des Algorithmus erläutert werden, sondern vielmehr seine allgemeinen Grundsätze. Der Grund dafür ist, dass sich der Code im Laufe der Zeit mehrmals ändern kann und wir in diesem Fall auch die Notiz korrigieren sollten.

> **Anmerkung des Autors:**
>
> Es kommt häufig vor, dass der Code nicht genau das tut, was in seiner Beschreibung steht. Das liegt vor allem daran, dass der Programmierer irgendwo einen Fehler gemacht hat. Der Vermerk sollte daher die allgemeinen Grundsätze beschreiben, damit wir den Code entsprechend anpassen können. Vergessen Sie aber nie, dass die einzige Wahrheit darüber, was in einer Anwendung tatsächlich passiert, nur durch den tatsächlichen Code beschrieben wird, und die Notiz hat keinen Einfluss darauf.

Grafische Trennung von Codeteilen
----------------------------

Beim Entwurf einer Anwendung ist es wichtig, die logischen Blöcke voneinander zu trennen. Normalerweise werden sie in Funktionen, Methoden oder im Falle von Basiscode zumindest durch Kommentare getrennt.

Bei einem längeren Algorithmus beschreibe ich in der Regel zunächst das gesamte Prinzip des Algorithmus und nummeriere dann die einzelnen Stellen im Code, damit der Entwickler die spezifische Funktionalität anhand dieser Stellen besser verstehen kann.

```php
/**
 * Die Funktion errechnet das arithmetische Mittel.
 *
 * 1. eine Liste von Nummern erhalten
 * 2. Summe und Anzahl der Zahlen ermitteln
 * 3) Berechnen und drucken Sie den Durchschnitt
 */

// 1.
$numbers = [1, 3, 8, 12];

// 2.
$sum = array_sum($numbers);
$count = count($numbers);

// 3.
echo 'Der Durchschnitt ist:' . ($sum / $count);
```

Mit den Zeichen `/**` beginnt ein mehrzeiliger Kommentar, der bis zur Markierung `*/` gilt. Um die Lesbarkeit zu gewährleisten, empfiehlt es sich, ein Sternchen an den Anfang jeder Zeile zu setzen.

Kommentare zur Dokumentation
----------------------

Dokumentationskommentare werden in der Regel verwendet, um Funktionen zu beschreiben und zu dokumentieren (Verhalten, Parameter, Rückgabewerte, Autor, usw.).

In früheren Versionen von PHP (vor `7.0`) wurden noch keine Datentypen verwendet, so dass der Typ einer bestimmten Variablen direkt im Kommentar beschrieben wurde.

```php
/**
 * @Autor Jan Barášek <jan@barasek.com>
 * @Lizenz MIT
 * @link https://php.baraja.cz
 * @param float[] $numbers
 */
function average(array $numbers): float
{
    $sum = array_sum($numbers);
    $count = count($numbers);

    return $sum / $count;
}
```

Dokumentationskommentare werden vor allem deshalb "Dokumentation" genannt, weil sie ein vorab vereinbartes Format haben, das von bestimmten Entwicklungsumgebungen (und Editoren), aber auch von automatischen Werkzeugen zur Dokumentationserstellung oder Codeprüfung verstanden wird.

Code auf Tschechisch oder Englisch schreiben?
-----------------------------

Ich schreibe den gesamten Code nur auf Englisch (einschließlich Funktionsnamen, Variablen, Kommentare, ...).

Dies hat eine Reihe von Vorteilen:

- Der Entwickler kann sein Englisch sofort proaktiv trainieren.
- Ein großer Teil der Anwendung verwendet Bibliotheken von Drittanbietern, die auf Englisch sind, so dass die Konsistenz automatisch gewahrt bleibt.
- Für die meisten fortgeschrittenen Themen gibt es überhaupt keine englische Übersetzung.
- Ich bin sicher, Ihnen fallen noch viele andere Beispiele ein

PHP erfordert nicht direkt Englisch und Sie können alles auf Englisch schreiben. Ich sehe die Verwendung der englischen Sprache eher als eine Art Investition in die Zukunft und als eine Möglichkeit, den Code durch andere Personen, die nicht englische Muttersprachler sind, leicht zu erweitern.

Vollständig lokalisierter Code in englischer Sprache wird auch in Unternehmen verwendet, daher ist es gut, von Anfang an Englisch zu üben.

Reihenfolge der numerischen Operationen
------------------------

Denken Sie immer daran, dass PHP Zahlen rundet, wenn Sie numerische Operationen durchführen. Dies ist oft lästig, da jedes Ergebnis mit Dezimalzahlen mit einer gewissen Ungenauigkeit verbunden ist.

Eine gute Lösung scheint es zu sein, zunächst die Zahlen zu erhöhen und dann mit den größtmöglichen Zahlen zu rechnen. Auf diese Weise kommt es statistisch gesehen zu weniger Verzerrungen.

Beispiel:

```php
echo 10 / 3; // Er schreibt 3.3333333333333
```

In manchen Fällen können Sie auch den Trick anwenden, überhaupt keine Dezimalstellen zu verwenden und alles als ganze Zahl zu berechnen. In diesem Fall gibt es keine solche Verzerrung:

```php
echo 1 / 2 * 2; // Dies ist schlechter, denn 1/2 = 0,5*2 = 1
echo 2 * 1 / 2; // Das ist besser, denn 2*1 = 2/2 = 1
```

Wenn Sie große, komplexe numerische Operationen lösen, verwenden Sie Brüche, um Zahlen zu schreiben.
