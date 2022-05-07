Befehle, Schlüsselwörter und Funktionen in PHP
==============================================

> id: '96a00928-4410-479d-ada0-612de21cdacd'
> slug:
> 	cs: prikazy-a-funkce
> 	de: befehle-schluesselwoerter-und-funktionen-in-php
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '4534cc03d2ae66454b9f398139f232a4'

Jedes PHP-Skript besteht aus Befehlen und Funktionen, zusammen werden diese **Konstrukte** genannt. Bei der Verarbeitung des Skripts werden diese Sprachausdrücke verfolgt (dieser Vorgang wird **Tokenisierung** genannt), und auf der Grundlage der *Schlüsselwörter* entscheidet der Interpreter, wie sich der Prozessor verhalten soll.

Befehle
--------------------------

Befehle (<a href="https://www.php.net/manual/en/reserved.keywords.php">im Englischen werden sie **Schlüsselwörter** genannt</a>) sind bereits vorprogrammierte Teile der Sprache, sie sind speziell für ihren Zweck reserviert, können in jeder Situation sofort verwendet werden (sie benötigen keine zusätzlichen speziellen Bibliotheken oder Installationen) und bilden die Grundlage aller Skripte.

Zum Beispiel <a href="/echo">Extrahieren einer Zeichenkette in HTML-Code</a>:

```php
echo 'Echo ist ein Sprachbefehl zum Auflisten des Inhalts';
```

Bei Befehlen ist zu beachten, dass sie keinen Wert zurückgeben und daher nicht als Variable verwendet werden können. Befehle lassen sich auch daran erkennen, dass sie keine Klammern enthalten.

Beispiele:

```php
echo 'Hallo, Welt!';
echo ('Hallo, Welt!');
```

Wenn ich den Inhalt des Befehls in Klammern setze, ändert sich das Verhalten nicht und er wird als **Ausdruck** betrachtet. Das ist dasselbe, als wenn wir in Mathematik schreiben würden:

```php
5 + 3

// oder:

(5 + 3)
```

Technisch gesehen gibt es einen Unterschied, aber in der Praxis ändert sich nichts.

Funktionen
--------------------------

Wenn es nur Befehle gäbe, wäre es ziemlich langweilig. Funktionen sind eine Sammlung von mehreren Befehlen.

Oft wollen wir denselben Code an mehreren Stellen wiederholt ausführen. In diesem Fall wäre ein ständiges Kopieren zu viel Arbeit und könnte zu Fehlern führen. Deshalb verpacken wir solchen Code in eine Funktion, die wir benennen und rufen sie dann einfach mit ihrem Namen auf.

Wenn wir die Funktion aufrufen, übergeben wir ihr die Parameter als Variablen, sie führt den Code aus und gibt das Ergebnis zurück, das wir weiter verwenden können.

```php
$text = 'PHP ist meine Lieblingssprache!';

echo 'Text "' . $text . '" ist eine lange' . strlen($text) . 'Zeichen.';
```

PHP hat viele Funktionen direkt vordefiniert (<a href="/documentation">Siehe die Dokumentation für eine vollständige Liste</a>), aber es ist oft bequem, eigene zu definieren:

```php
function mojeFunkce(int $x, int $y): int
{
   $z = $x + $y;   // fügt die Eingabeparameter hinzu und speichert sie in einer Variablen

   return $z;      // gibt die Variable $z zurück
}

echo mojeFunkce(5, 3); // gibt die Zahl 8 aus, da die Zahlen von der Funktion verarbeitet wurden
```

Variablen in Funktionen
--------------------------

Lokale Variablen werden innerhalb von Funktionen verwendet, was bedeutet, dass sie nur innerhalb der Funktion verwendet werden können und nicht außerhalb der Funktion manipuliert (oder definiert) werden können. Sie erhalten ihre Anfangswerte aus den Funktionsparametern direkt in der Funktionsdefinition.

Beispiel:

```php
$z = 5;

function prvniFunkce(int $x, int $y): int
{
   return $x - $y; // dies würde die Differenz der Zahlen zurückgeben
}

function druhaFunkce(): mixed
{
   return $z; // Dies ergibt einen Fehler, da die Variable $z
              // nicht innerhalb der Funktion definiert
}
```

Manchmal ist es sinnvoll, einige der Parameter als optional einzustellen, indem ein alternativer (Standard-)Wert definiert wird:

```php
echo prictiCislo(5);    // ergibt 6
echo prictiCislo(5, 7); // ergibt 12

function prictiCislo(int $x, int $y = 1): int
{
   return $x + $y;
}
```

Die Funktion "prictiCislo()" addiert den Wert der Variablen "$y" zu der Variablen "$x". Wenn die Variable `$y` nicht definiert ist (als Parameter beim Aufruf der Funktion angegeben), wird ihr Standardwert aus der Funktionsdefinition verwendet. Der zweite Funktionsparameter (die Variable `$y`) ist daher optional.

> Jede Funktion kann nur eine Ausgabe (eine Rückgabe) haben. Wenn Sie mehrere Ausgaben in einer Funktion angeben, wird die zuerst angegebene zurückgegeben. Wenn Sie mehrere Werte zurückgeben wollen, müssen Sie ein Array verwenden.
>
> In PHP müssen wir (im Gegensatz zu anderen Sprachen) überhaupt keinen Rückgabewert angeben. In diesem Fall gibt die Funktion bei jeder Eingabe nichts zurück (void). Solche Funktionen werden meist zum Speichern von Daten oder zur Ausgabe in Quellcode verwendet. Im Allgemeinen ist es jedoch empfehlenswert, immer einen Wert zurückzugeben, zumindest eine Bestätigung, dass alles gut gelaufen ist.
