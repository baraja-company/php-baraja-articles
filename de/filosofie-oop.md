Grundlegende Philosophie der objektorientierten Programmierung
==============================================================

> id: c38214d9-8c92-4484-9c31-239d716f2545
> slug:
> 	cs: filosofie-oop
> 	de: grundlegende-philosophie-der-objektorientierten-programmierung
> 
> publicationDate: '2020-01-02 22:21:56'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3841046b40a039413bacd045f275c68

Objektorientierte Programmierung ist ein Paradigma, eine Sichtweise, wie man programmiert. Sie werden bald selbst sehen, dass OOP eine ziemlich grundlegende Vereinfachung für alle gängigen Probleme und Schwierigkeiten bringt, die in der realen Programmierung immer wieder gelöst werden müssen.

Der Grundgedanke
-----------------

Die Grundidee der objektorientierten Programmierung besteht darin, eine große Anwendung (eine komplexe Aufgabe) in viele kleine Teile zu zerlegen, die wir elegant und unabhängig voneinander lösen können.

Wenn wir zum Beispiel ein Reservierungssystem für Flugtickets programmieren, ist das ein sehr komplexes Projekt, das Tausende von Fällen löst. Wenn wir die gesamte komplexe Logik in viele Schichten und Teile zerlegen können, können wir das gesamte komplexe System leicht verstehen und die einzelnen Teilaufgaben unabhängig voneinander programmieren.

Praktische Vorteile von OOP und der Aufteilung des Codes in Objekte
------------------------------------------------

Abgesehen von der akademischen Sichtweise gibt es viele praktische Gründe, OOP zu verwenden:

- Die Anwendung wird eine <a href="/encapsulation">Encapsulation</a> erstellen, was bedeutet, dass die Daten immer in einem lokalen Kontext verarbeitet werden, was nur wenige sind, so dass wir nicht an die gesamte Anwendung auf einmal denken müssen.
- Wir können eine Anwendung in Hunderttausende kleiner Teile aufteilen, die von verschiedenen Personen parallel entwickelt werden können, und lediglich eine gemeinsame Schnittstelle einrichten.
- Wir können die Anwendung aus der Perspektive verschiedener Schichten betrachten (abstrakt auf ganze Module oder lokal auf bestimmte Funktionen, die einen bestimmten Algorithmus lösen).
- Bei der Fehlersuche in der Anwendung (Debugging) können wir die Anwendung schrittweise verändern, einige Teile weglassen oder ersetzen.
- Wir können **Entwurfsmuster** besser anwenden und so Fehler und Komplexitäten im Code vermeiden, bevor sie auftreten.

Ich persönlich kann mir nicht vorstellen, dass Teams mit mehr als einer Person anders programmieren.

> **Anmerkung:**
>
> Die Verwendung von Objekten belastet den Computer mit etwas mehr Speicher und CPU-Overhead, so dass die Leistung der Anwendung etwas geringer ist. In einer realen Umgebung spielt dies jedoch keine Rolle, da die Neuprogrammierung der Anwendung auf Objekte zwar einen gewissen Leistungsverlust bedeutet (in der Regel im Prozentbereich), aber den Programmierern Zeit spart (in der Regel Dutzende bis Hunderte von Prozent). Menschliche Zeit ist immer viel teurer (und sehr begrenzt) als Computerzeit.
>
> > Vergessen Sie auch nicht, dass OOP eine große Vereinfachung der gesamten Anwendung mit sich bringt und es ermöglicht, große Anwendungen in einer angemessenen Zeitspanne fertigzustellen. Eine große Anzahl komplexer Anwendungen wäre ohne Objekte kaum zu programmieren.

Beziehung zur realen Welt
-------------------------

Das grundlegende Ziel von OOP bei der Softwareentwicklung ist es, die Eigenschaften, Verhaltensweisen und Prinzipien der realen Welt so weit wie möglich zu simulieren. Objekte in OOP stellen reale Entitäten dar. Diese Denkweise ermöglicht es uns, riesige komplexe Systeme zu bauen, die man gut verstehen kann, die reale Probleme intern so lösen, wie sie auch ohne Computer gelöst werden könnten, und deren Prinzipien man echten Menschen erklären kann.

Wenn wir beispielsweise eine Anwendung zur Verwaltung von Inhalten implementieren, ist es sinnvoll, die gesamte interne Logik in vielen realen Entitäten (Artikel, Autor, Kategorie) anzulegen und Sitzungen nicht nach künstlich erzeugten Datensatz-IDs (wie in Datenbanken üblich), sondern nach realen Beziehungen aufzubauen.

Beispiel für eine konkrete Umsetzung:

```php
class Article
{
    private Author $author;

    /** @var Kategorie[] */
    private array $categories;

    private string $title;
}

class Author
{
    private string $name;
}

class Category
{
    private string $name;
}
```

Wie wir sehen können, enthält die Klasse "Artikel" nicht nur technische Parameter wie die ID des Datensatzes mit dem Autor, sondern sie ist eine echte Bindung an die Entität "Autor", die wiederum ihre eigenen Eigenschaften hat.

Eine Erläuterung der spezifischen Implementierung und Syntax finden Sie im <a href="/uvod-do-oop">Einführungslehrgang</a>.
