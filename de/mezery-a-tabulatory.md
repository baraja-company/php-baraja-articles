Code mit Leerzeichen und Tabulatoren einrücken
==============================================

> id: '116f19ed-3753-498d-bb9e-e0f93b88c347'
> slug:
> 	cs: mezery-a-tabulatory
> 	de: code-mit-leerzeichen-und-tabulatoren-einruecken
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c3af36d7df67cf36b940c9702cb71549

Um den Code für andere Programmierer leicht lesbar und elegant zu halten, müssen wir lernen, ihn einheitlich zu formatieren. Dieser Artikel befasst sich mit der Verwendung von Leerzeichen und Tabulatoren.

Sind Leerzeichen oder Tabulatoren besser für die Einrückung von Code? Dies ist oft ein endloses Diskussionsthema. Wenn Sie eine schnelle und eindeutige Antwort suchen, bevorzugen die meisten guten Programmierer die Verwendung von Tabulatoren, aber lassen Sie uns das Ganze etwas genauer betrachten.

Räume
----------------------

Jeder Programmierer und jeder Editor verwendet eine andere Anzahl von Leerzeichen für die Einrückung (meistens jedoch 4), was zu inkonsistentem Code führt, der schwerer zu lesen ist, wenn man den Code eines anderen Programmierers liest. Außerdem werden mehr Zeichen für die Einrückung benötigt (was die Datengröße erhöht).

Leerzeichen haben jedoch einen Vorteil bei der Darstellung von Code in einem Webbrowser (wo die HTML-Entität `&nbsp;` für die Einrückung verwendet wird), so dass es sich um ein relativ einfach zu portierendes Format handelt, das nur als stabile und zuverlässige Darstellungsmethode einen Vorteil bietet (4 Leerzeichen werden immer als 4 Leerzeichen angezeigt).

Tabulatoren
----------------------

Sie sind so breit, wie der Programmierer sie im Editor einstellt (wenn der Editor das kann). Wenn Sie also eine bestimmte Einrückung bevorzugen, ist das kein Problem - wir können uns denselben Code mit unterschiedlichen Tabulatorbreiten ansehen. Gleichzeitig ist es ein sehr sparsames Zeichen, das nicht so oft wiederholt werden muss wie einfache Leerzeichen.

Bei der Wiedergabe von Code mit Tabulatoren in einer HTML-Seite ist es üblich, Tabulatoren durch feste Leerzeichen zu ersetzen, um eine korrekte Anzeige in allen Browsern zu gewährleisten:

```php
$code = '<?php
    $a = 5+3;
    $b = 4;
    if ($a > $b) {
        echo $a . " > " . $b;
    } sonst {
        echo $b . " <= " . $a;
    }
?>';

echo str_replace("\t", '&nbsp;', $code);
```
