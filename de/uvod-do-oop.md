Einführung in die objektorientierte Programmierung in PHP
=========================================================

> id: '44a79461-dcfd-4dd0-b6fd-ba5db76db6de'
> slug:
> 	cs: uvod-do-oop
> 	de: einfuehrung-in-die-objektorientierte-programmierung-in-php
> 
> publicationDate: '2020-01-01 19:54:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '888db2cf35845892eebade9fdcc18871'

Willkommen zum ersten Artikel des Online-Kurses OOP in PHP. Eine vollständige Liste der Artikel finden Sie auf der <a href="/oop">Übersichtsseite</a>.

**Inhaltliche Hinweise:**
>
> Das Ziel dieser Serie ist es, das Wesen** der objektorientierten Programmierung so gut wie möglich zu erklären, damit Sie nicht Hunderte von Stunden damit verbringen müssen, mit Dingen zu experimentieren, die keinen Sinn ergeben. Alle Techniken und Anleitungen sind aus der Praxis und werden so erklärt, wie ich sie selbst gerne gelesen hätte, als ich programmieren lernte. Einige **Konzepte sind stark vereinfacht**, was auf Kosten der 100%igen sachlichen Korrektheit geht, denn ich glaube, dass es immer **besser ist, hauptsächlich die Prinzipien zu verstehen** und zumindest eine Ahnung von den Problemen zu haben, als sich in der Programmierung zu verlieren und alles als ein großes Durcheinander zu sehen.
>
> Wie Sie bald sehen werden, hat die Programmierung in OOP enorme Vorteile für Ihren Code. Es bringt eine neue Abstraktionsebene über Ihre Anwendung und ermöglicht Ihnen, sehr komplexe Probleme zu lösen, die auf andere Weise nicht möglich wären.

Was sind Objekte
------------------

Im Grundkonzept der Programmierung sind Objekte spezielle Datentypen, die zusätzlich zu ihrem eigenen Wert einen detaillierten Zustand, Methoden und Verhalten definieren können und die Durchführung verschiedener Operationen ermöglichen, wie in der realen Welt.

Ein Objekt in der Programmierung ist so etwas wie ein "Ding" aus der realen Welt, das wir benennen können (z. B. durch ein Substantiv) und das die gleichen Eigenschaften hat wie ein Ding aus der realen Welt. Ein Objekt kann zum Beispiel ein Artikel sein, der einige Werte (Titel, Inhalt, Autor, Veröffentlichungsdatum, ...) und Methoden (neuen Titel einfügen, Inhalt lesen, Anzahl der Tage seit Veröffentlichung berechnen, ...) hat. In diesem Artikel werden wir uns hauptsächlich damit beschäftigen, wie man das Objekt auf einer grundlegenden Ebene erstellt und damit arbeitet.

Objekte und Klassen
---------------

Wie wir bereits festgestellt haben, ist ein **Objekt etwas, das existiert**. Das Wort "existiert" ist in diesem Stadium sehr wichtig, denn wie wir gleich sehen werden, ist noch eine praktische Umsetzung in der Programmierung zu bewältigen.

Da wir beim Programmieren Code schreiben und ein Objekt etwas "Lebendiges" ist, werden wir von diesem Punkt an zwischen zwei verschiedenen Konzepten unterscheiden, bei denen es wichtig ist, die Bedeutung im Detail zu verstehen und diese strikt zu unterscheiden:

- Ein **Objekt** ist etwas "Lebendiges", das in diesem Moment existiert (eine Instanz hat) und etwas ausführen oder auf andere Objekte reagieren kann.
- Eine **Klasse** ist statisch geschriebener Code, der noch ausgewertet werden muss und immer gleich bleibt.

Da wir beim Programmieren Code immer als statische Zeichenkette (Text) schreiben, werden wir Programme in `Klassen` (statische Definition des gesamten Objekts) und `Objekte` (eine `lebende` Instanz der Klasse) unterscheiden.

Beispiel für eine Klassendefinition:

```php
class Article
{
    public string $title;

    public ?string $autor = null;
}
```

> **Praktische Hinweise:**
>
> In einer realen Anwendung wird jede Klasse normalerweise in eine separate PHP-Datei geschrieben, die den gleichen Namen wie die Klasse trägt.
>
> Für die Klasse "Artikel" erstellen wir zum Beispiel "src/Artikel.php". Diese Konvention ist in PHP nicht erforderlich, aber sie trägt dazu bei, die Anwendung lesbarer zu machen, damit Sie nicht zu viel Code an einer Stelle haben.
>
> Jede Klasse kann in der gesamten Anwendung höchstens einmal vorkommen (einen eindeutigen Namen haben), aber es kann (fast) unendlich viele Instanzen geben (je nach RAM-Kapazität). Außerdem kann eine einzelne Klasse nicht in mehrere Dateien aufgeteilt werden und muss an einer einzigen Stelle definiert werden. Wenn Sie mehrere Klassen mit demselben Namen erstellen müssen, verwenden Sie **Namensräume**.
>
> Wir werden später noch zeigen, dass gut strukturierter Code in vielen Projekten wiederverwendet werden kann.

Dieser Code definiert eine Klasse "Artikel" mit zwei Eigenschaften (Titel und Autor). Kommentare werden von PHP ignoriert und nur zur statischen Typüberprüfung verwendet (normalerweise von PhpStorm gelesen).

Code:

```php
public string $title;
```

bedeutet die Definition von "Eigenschaften", die eine Eigenschaft der Klasse "Artikel" ist. Wir können uns das so vorstellen, dass "Artikel" eine Art Feld ist und "Titel" sein Index, in den wir einen Wert schreiben können. Jede Eigenschaft muss eine definierte Zugänglichkeit haben (`public`, `protected` oder `private`), wir werden später erklären, was jede Einstellung bedeutet. Wenn Sie nicht wissen, welche Zugänglichkeit Sie in einem bestimmten Fall wählen sollen, geben Sie `public` ein.

Instanziierung einer Klasse und Erstellung eines Objekts
----------------------------------

Das Konzept der "Instanz" ist eines der am schwierigsten zu verstehenden Konzepte, daher ist es wichtig, dass Sie die folgenden Zeilen sehr sorgfältig lesen und ich empfehle Ihnen, alle Beispiele auszuprobieren.

Wenn unsere Anwendung bereits eine Klasse im Quellcode definiert hat, wird sie nirgendwo verwendet und PHP weiß nichts davon. Das liegt daran, dass OOP das "Datenkapselungsprinzip" einführt, was bedeutet, dass eine Klasse einen internen (lokalen) Kontext hat, der nur innerhalb der Klasse gültig ist. Das liegt daran, dass wir bei der objektlosen Entwicklung daran gewöhnt waren, den Code immer in seiner Gesamtheit zu evaluieren. Versuchen Sie auch, sich eine Klasse als eine Art Funktion oder extern aufgerufenes Programm vorzustellen.

Jetzt können wir unsere erste Instanz erstellen, indem wir den neuen Operator "new" verwenden.

```php
$myArticle = new Article;
$myArticle->title = 'Mein erster Artikel'; // schreibt den Wert

echo $myArticle->title; // listet "Mein erster Artikel" auf
```

Beachten Sie, dass wir das gesamte Objekt "Artikel", das durch den Operator "new" erstellt wurde, in die Variable "$myArticle" aufnehmen. Das bedeutet für uns, dass das Objekt eigentlich ein Datentyp ist, so dass wir es über Variablen hinweg verschieben und mit ihm weiterarbeiten können.

Der Pfeiloperator `->` wird verwendet, um das Objekt zu manipulieren. Wenn wir eine Instanz eines bestimmten Objekts haben (wir haben eine Variable mit seinem Inhalt), können wir ganz einfach Werte in die internen Eigenschaften schreiben oder sie lesen oder sie auf verschiedene Weise mit Hilfe von Methoden ändern (wir werden später sehen).

Die **Instanz eines Objekts ist die Erstellung einer lokalen "lebenden" Kopie der Klasse im Speicher (eine Variable).**

Gleichzeitiges Erstellen mehrerer Instanzen der gleichen Klasse
---------------------------------------------

Wie wir bereits besprochen haben, hat jedes Objekt einen lokalen Kontext, der in seinen Interna gilt. Dank dieses Prinzips können wir viele unabhängige Instanzen der gleichen Klasse erstellen und sie frei manipulieren. Wir können sogar eine dynamische Anzahl von Instanzen erstellen und sie zum Beispiel als Array-Elemente speichern. Die Möglichkeiten sind endlos.

> **Warnung:**
>
> Wenn Sie eine große Anzahl von Instanzen erstellen (Hunderte oder mehr), sollten Sie den Speicherbedarf im Auge behalten, da PHP Informationen über alle Instanzen im RAM halten muss. In der Praxis tritt das Problem des Speicherüberlaufs aufgrund der vielen Instanzen jedoch nicht auf.

Beispiel:

```php
$firstArticle = new Article;
$firstArticle->title = 'Mein erster Artikel';

$secondArticle = new Article;
$secondArticle->title = 'Über einen Hund und eine Katze';

echo $firstArticle->title;  // listet "Mein erster Artikel" auf
echo $secondArticle->title; // schreibt aus "Über einen Hund und eine Katze"
```

Beachten Sie, dass die Auflistung der Eigenschaft `title` (`->title`) davon abhängt, aus welcher Instanz wir gerade lesen.

Zusammenfassung
-------

Wir haben gezeigt, wie wir unsere erste Klasse definieren und eine Instanz dieser Klasse (ein Objekt) erstellen, in die wir mehrere Werte schreiben.

In der nächsten Folge werden wir das Konzept des <a href="/methods-and-passing-input">Konstruktors, der Methoden und der Übergabe von Eingaben</a> erklären.
