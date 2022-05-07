Taschenrechner in PHP: Verarbeitung eines mathematischen Ausdrucks als String
=============================================================================

> id: e6798758-031d-4e7c-b24e-1f77cf61558d
> slug:
> 	cs: pokrocila-kalkulacka
> 	de: taschenrechner-in-php-verarbeitung-eines-mathematischen-ausdrucks-als-string
> 
> publicationDate: '2020-02-16 17:07:38'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '63f5336bf2bbabe5312121b57e1ce34e'

Stellen Sie sich vor, Sie stehen vor der Aufgabe, ein einfaches mathematisches Beispiel zu verarbeiten, das ein Benutzer als Textzeichenfolge in ein Suchfeld eingibt. Normalerweise möchte der Benutzer eine einfache numerische Operation mit Zahlen durchführen. In diesem Artikel werden der Gedankengang und die konkreten Anweisungen dazu beschrieben.

Naive Umsetzung
-------------------

Lange Zeit habe ich mich gefragt, ob ein einfacher mathematischer Ausdruck durch einen Trick verarbeitet werden könnte, um den Code so kurz wie möglich zu machen... und nach vielen Jahren habe ich tatsächlich die Lösung.

Betrachten Sie die angegebene Lösung **nur als Beispiel**, denn sie ist **extrem gefährlich** und ein unehrlicher Benutzer kann die Zeichenfolge leicht unterstreichen, was zum Beispiel die gesamte Anwendung löschen oder die Datenbank stehlen kann!

```php
// Benutzerabfrage
$query = '5 + 3 * 2';

// Verarbeitung des Ausdrucks als regulärer PHP-Code
eval('$result = @(' . $query . ');');

// Auflistung der Variablen mit Ausdruckslösung
echo $result; // druckt 11
```

Der Trick besteht darin, dass die Funktion <a href="/function-eval">eval()</a> die Zeichenkette so ausführt, als befände sie sich im Kontext von PHP-Code. Es ist verrückt, aber es funktioniert. Der Wrapper unterdrückt Fehlermeldungen.

Umgang mit komplexeren Eingaben
--------------------------

Abgesehen davon, dass die **Verarbeitung von Ausdrücken mit eval() extrem gefährlich** ist, bietet sie auch keine Syntax, die eloquent genug ist, um allen gerecht zu werden. Wenn der Benutzer auch nur einen einzigen Syntaxfehler begeht, kann der gesamte Ausdruck nicht verarbeitet werden.

Daher besteht die Lösung darin, die Benutzeranfrage zunächst zu **verstehen** und nach dem formalen Aspekt zu korrigieren (Normalisierung auf die kanonische Form genannt) und sie dann weiterzuleiten und zu verarbeiten.

Ich habe [QueryNormalizer](https://github.com/mathematicator-core/engine/blob/master/src/QueryNormalizer.php) in der Vergangenheit für genau diese Aufgabe programmiert.

Die Bearbeitung selbst ist eine sehr anspruchsvolle Aufgabe, denn man muss die verschiedenen Zusammenhänge richtig verstehen. Zum Beispiel, dass Klammern verschachtelte Blöcke bezeichnen und rekursiv ausgewertet werden müssen. Zum Beispiel kann der Ausdruck "5+2^(1+3/2)" nicht einfach gelöst werden, weil der Bruch zuerst gelöst, zur Zahl in Klammern addiert, dann für die ganzzahlige Potenz gelöst und schließlich auf der Basisebene addiert werden muss.

Um diese anspruchsvolle Anforderung überhaupt erfüllen zu können, kann der Ausdruck nicht mehr als gewöhnliche Zeichenkette behandelt werden, und wir müssen die **Abstraktionsebene** einnehmen. Im Grunde ist die Mathematik eine Art Sprache, die die Beziehungen zwischen Operationen und Zahlen beschreibt, denn wir haben es mit Operatorprioritäten, unterschiedlichen Bedeutungen, Kontexten, Rekursionen und sogar Datentypen zu tun. Hier kommt der **Abfrage-Tokenisierungsprozess** ins Spiel.

> Ich arbeite seit 2015 an dem Problem der mathematischen Tokenisierung und habe seitdem mehrere verschiedene Parser geschrieben.

Die beste davon, die derzeit den neuen Mathematicator antreibt, ist [verfügbar als OpenSource auf GitHub] (https://github.com/mathematicator-core/tokenizer).

Bei der Tokenisierung geht es darum, eine Zeichenkette zu **parsen**, sie in Gruppen kleinerer Zeichenketten bekannter Typen aufzuteilen und diese dann in Objekte (Datentypen) umzuwandeln. Das umgewandelte Array von Objekten wird dann durch **clevere Logik in einen binären Baum** umgewandelt, der Abhängigkeiten und Rekursionen beschreiben kann. Dies ist ein sehr anspruchsvoller Prozess, da es Hunderte von möglichen Szenarien gibt und die Benutzer bei ihren Abfragen sehr kreativ sein können.

Der Hauptvorteil des Token-Arrays besteht darin, dass es sehr einfach an die nächste Ebene weitergegeben werden kann, die zum Beispiel [die eigentliche Berechnung durchführt] (https://github.com/mathematicator-core/calculator) oder [den Baum in LaTeX neu zeichnet] (https://github.com/mathematicator-core/tokenizer/blob/master/src/TokensToLatex.php).

So elegant kann die Verwendung aussehen:

```php
$tokenizer = new Tokenizer(/* einige Abhängigkeiten */);

// Umwandlung der mathematischen Formel in ein Array von Token:
$tokens = $tokenizer->tokenize('(5+3)*(2/(7+3))');

// Jetzt können Sie Token in ein nützlicheres Format konvertieren:
$objectTokens = $tokenizer->tokensToObject($tokens);

dump($objectTokens); // Rückgabe von typisierten Token mit Metadaten

// In LaTeX rendern
echo $tokenizer->tokensToLatex($objectTokens);

// In den Debug-Baum rendern (extrem schnell):
echo $tokenizer->renderTokensTree($objectTokens);
```

Verfahren anzeigen
-----------------

Viele Benutzer werden es zu schätzen wissen, wenn das Programm **beim Rechnen den Vorgang** anzeigt, um zu zeigen, wie es ihn durchgeführt hat. Das ist auch für den Programmierer nützlich, denn so kann er zumindest leicht feststellen, wo ein Fehler in der Berechnung vorliegt und den Algorithmus entsprechend korrigieren. Wenn Sie all dies mit maschinellem Lernen auf der Grundlage automatisierter Tests kombinieren, erhalten Sie etwas Erstaunliches.

Schauen Sie sich an, wie `QueryNormalizer` Ihre Abfrage verstehen konnte, die Daten an den Tokenizer weitergab, dieser die Abfrage entsprechend in LaTeX renderte und dann den Objektbaum an den Rechner weitergab, der das Gesamtergebnis zurückgab.

Příklad: [5+2^(1+3/2)](https://mathematicator.com/search/5%2B2%5E%281%2B3/2%29).

Die Darstellung des Verfahrens erfolgt dadurch, dass der Rechner den Eingabebaum durchläuft und eine Regel nach der anderen entsprechend den darin enthaltenen Token und Regeln auswertet. Wenn eine Regel ausgewertet wird, werden die Schrittinformationen in ein Array geschrieben. Gelegentlich kann sich ein Schritt als falsch erweisen und wir müssen zurückgehen und einen anderen Weg in der Berechnung einschlagen, aber dahinter steckt eine ganze Menge Magie, die vorerst verborgen bleibt und die Sie in der Implementierung studieren können.

Schlussfolgerung
-----

Das obige Verfahren beschreibt, wie man elegant mit mathematischen Ausdrücken umgeht, die Zahlen, Operationen und Beziehungen zu ihnen enthalten. Mit diesem Ansatz können zum Beispiel keine Ausdrücke geändert oder Gleichungen gelöst werden, aber damit befassen wir uns beim nächsten Mal.

*Wenn Sie andere Ideen haben, wie man Mathe effizient bearbeiten kann, würde ich mich freuen, von Ihnen zu hören.
