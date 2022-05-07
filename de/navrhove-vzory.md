Entwurfsmuster in PHP
=====================

> id: c22b3e54-bb66-4196-a998-f4b62b9308b2
> slug:
> 	cs: navrhove-vzory
> 	de: entwurfsmuster-in-php
> 
> publicationDate: '2020-02-13 10:32:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '9ee32f0a3ce55bad52725f45fcd1c041'

Entwurfsmuster sind eine Art, über Programmierung nachzudenken.

Sie bieten eine Sammlung von Ratschlägen, vorgefertigten Praktiken, Best Practices und Einblicken in die Entwicklung. Für jedes Programmierparadigma und jeden Aufgabentyp gibt es bestimmte Entwurfsmuster, die am besten geeignet sind.

Vorteile - warum Entwurfsmuster verwenden?
---------------------------------------

Beim Programmieren ist es sinnvoll, eine Methode zur Lösung bestimmter Probleme zu wählen und diese Methode immer wieder zu wiederholen.

Der Hauptvorteil ergibt sich vor allem bei der Entwicklung im Team, wenn jeder weiß, wie die Anwendung entwickelt wird (nach welchem Entwurfsmuster) und sie es einfach anwenden. Damit entfallen die vielen Stunden, die man mit dem Debuggen von seltsam geschriebenem Code und dem Versuch, die vom Autor beabsichtigten Prinzipien zu verstehen, verbringen muss.

MVC - ein Beispiel für die Verwendung eines Entwurfsmusters
--------------------------------------

Mein bevorzugtes Entwurfsmuster ist `MVC` (von `Model View Controller`), das besagt, dass die Anwendung in 3 unabhängige Schichten unterteilt ist, die sich gegenseitig nacheinander aufrufen und Daten aneinander weitergeben.

Wenn beispielsweise eine Seite gerendert wird, könnte es so aussehen, als ob zuerst entschieden wird, um welche Art von Seite es sich handelt (z.B. ein Kategorie-Detail), also wird `CategoryController` mit der Methode `Detail` aufgerufen.

Ein konkretes Beispiel (ich vereinfache hier sehr):

```php
class CategoryController
{
    public CategoryManager $categoryManager;

    public function actionDetail(string $id): void
    {
        $this->template->id = $id;
        $this->template->category = $this->categoryManager->getById($id);
    }
}
```

> **Anmerkung:**
>
> Dies ist nur ein Beispielcode, der das Prinzip des "MVC"-Entwurfsmusters erklärt.
>
> In einer realen Implementierung müssten wir weiter herausfinden, wie wir zum Beispiel eine Instanz von `CategoryManager` bekommen und wie wir sie an property übergeben können. In der Regel wird für diese Art von Aufgaben die "Dependency Injection" verwendet.

Bevor die Seite für das Kategoriedetail gerendert wird, wird zunächst der `CategoryController` aufgerufen, um die eigentliche Anfrage zu erhalten (d.h. wir rendern das Kategoriedetail mit einer bestimmten ID, die z.B. vom Router in der URL erhalten wird), die Daten zu erhalten (Abfrage des entsprechenden `Model`) und die endgültigen Daten an die Vorlage für das Rendering zu übergeben.

Der große Vorteil dieses Prinzips besteht darin, dass wir viele Modelle (Anwendungslogik) schreiben können, die unabhängig von der Art und Weise sind, wie die Daten präsentiert werden (die Vorlage), was zu wiederverwendbarem Code führt. Wenn wir den "CategoryManager" in einem anderen Projekt verwenden wollen, leiten wir die Daten einfach auf eine bestimmte Weise durch den "Controller", der entsprechend der vom Projekt selbst definierten Vorlage gerendert wird, während die Anwendungslogik dieselbe bleibt und sich niemand darum kümmert, weil die Softwareschicht ihre vereinbarte Schnittstelle und Verantwortung erfüllt hat.

> **Praktische Anmerkung:**
>
> Das "MVC"-Designmuster wird von den meisten modernen Frameworks wie Nette, Symfony, Laravel und anderen verwendet.
>
> Wir können auch "MVC" in der Entwicklung von mobilen Anwendungen und anderen Arten von Software, wo wir brauchen, um Daten zu erhalten und machen es in einer Vorlage nach der Seite oder Ansicht Typ.

Wichtige Entwurfsmuster für die Webentwicklung
---------------------------------------

Im Allgemeinen gibt es in der Programmierung viele Entwurfsmuster, die für die Webentwicklung nicht geeignet sind. Diese Liste beschreibt die wichtigsten Muster, die ich selbst verwende und die Sie kennen sollten.

Eine <a href="/kategorien-design-patterns">vollständige Liste aller Design Patterns</a>, Beispiele für ihre Verwendung und ausführliche Erklärungen finden Sie auf einer eigenen Seite.

- **MVC** - Das Prinzip der Unterteilung einer Anwendung in `Model` (Anwendungslogik und Daten), `View` (Template und Datenansicht) und `Controller` (Verbindung von `Model` und `View`).
- **Dependency Injection** - Anstatt Klasseninstanzen zu erstellen, definieren wir so genannte Dienste, die wir automatisch injizieren lassen. Der Code lässt alle Abhängigkeiten zu, einzelne Teile können ausgetauscht werden und wir bauen die Anwendung dynamisch auf.
- **Singleton (Singleton)** - Jede Klasse hat nur eine Instanz (ich persönlich verwende dies in Kombination mit `Dependency Injection`, wo jeder Dienst nur eine Instanz hat, die über die Anwendung weitergegeben wird).
- **Faule Initialisierung (verzögerte Initialisierung)** - Wir erstellen eine Instanz eines Objekts, wenn es zum ersten Mal benötigt wird.
- **Adapter** - Wenn wir die Kommunikation zwischen zwei inkompatiblen Klassen ermöglichen müssen, konvertiert der `Adapter` Daten von einem Typ in den anderen (typischerweise konvertiert er native PHP-Datentypen in Datenbank-Datentypen und wieder zurück).
- **Befehl** - Anstatt eine bestimmte Aufgabe direkt auszuführen (z. B. das Versenden einer E-Mail), erinnern wir uns einfach daran, dass dies geschehen sollte. Ein anderer oder derselbe Prozess nimmt dann die Anfrage auf und bearbeitet sie. Der Hauptvorteil ist, dass wir die Anwendung träge (und daher sehr schnell) verarbeiten, fehlgeschlagene Aktionen wiederholen und einfach die Parameter des Aufrufs speichern können. Gleichzeitig können wir so Anfragen von verschiedenen Kunden, über APIs usw. erhalten. Gesendete Aktionen können auch in eine Warteschlange gestellt, nach Prioritäten geordnet und möglicherweise vor der Auswertung abgebrochen werden.
- **Strategie** - kapselt eine Art von Algorithmen oder Objekten, die geändert werden sollen, so dass sie für den Kunden austauschbar sind.

Es gibt noch viele weitere Entwurfsmuster, dies waren die wichtigsten, die Sie kennen sollten.

Anti-Muster
------------

Einige Programmiertechniken werden als "Anti-Pattern" bezeichnet, was das genaue Gegenteil eines Entwurfsmusters ist. Es handelt sich in der Regel um eine Technik, die seltsamen Code erzeugt, der nicht leicht zu debuggen und zu warten ist und sich "magisch" verhält.

Ein typisches Beispiel ist die Verwendung von <a href="/global-variable">globalen Variablen</a>.
