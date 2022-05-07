Test der Nette-Grundkenntnisse
==============================

> id: d137c177-503c-4e84-8709-0e65b0ce6060
> slug:
> 	cs: test-nette-zaklady
> 	de: test-der-nette-grundkenntnisse
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '66fb122b33aa8ce41158345bb01769ab'

Erfolgsschwelle: 15 Punkte

*Für jede richtig beantwortete Frage erhalten Sie 1 Punkt. Für jede falsch beantwortete Frage erhalten Sie nichts. Wenn die Antwort nur teilweise ist (und es nicht möglich wäre, die Sache darauf basierend zu programmieren), gilt die Frage als falsch (es ist nicht möglich, einen halben Punkt zu bekommen). Wenn die Lösung einen Sicherheitsfehler oder einen Tippfehler im Code enthält, wird die Antwort als falsch angesehen, da sie nicht ausgeführt werden kann.

-----------

1 Erkläre den Unterschied zwischen den Schleifen "for", "while" und "foreach". Nennen Sie für jedes Produkt ein konkretes Anwendungsbeispiel, das seinen Hauptvorteil deutlich macht.


2. wir haben eine Variable, über die wir fast nichts wissen (wir kennen nur ihren Namen). Wie können wir seinen Inhalt sehen? Sie heißt zum Beispiel `$data`.


3 Schreiben Sie die folgenden Befehle, um mit dem Git-Repository zu arbeiten:
- Laden Sie die neuesten Änderungen vom Server herunter
- die Datei `Statistic.php` markieren
- alle Dateien im Projekt markieren
- alle Dateien im Verzeichnis `cron` markieren
- Übertragen von Änderungen mit der Meldung "Meine Übertragung"
- Senden der Übergabe an den Server


4. eine Textzeichenfolge in die Variable einfügen. Nennen Sie ein Beispiel für eine Funktion zur Berechnung der Prüfsumme.


Schreiben Sie ein Codefragment, das eine Aktion "Löschen" in "Präsentator" erstellt, die die Element-ID als Ganzzahl akzeptiert und eine Zeile aus der Tabelle "Frage" entsprechend der angegebenen ID löscht. Nach erfolgreicher Löschung wird die Meldung "Frage gelöscht" ausgegeben und zur Aktion "Liste" weitergeleitet.

Unter Frage für einen zusätzlichen Punkt: Wenn der Löschvorgang aus irgendeinem Grund fehlschlägt, wird kein schwerwiegender Fehler ausgegeben, sondern der Benutzer mit einer Meldung darüber informiert (Flash-Meldung).

6. wenn ich ein Nette-Formular erstelle, wird es zu einer Komponente. Was ist eine Nette-Komponente?

7 Ich muss ein einfaches Nette-Formular erstellen, um einen Datensatz in eine "Frage"-Tabelle einzufügen, die eine Liste von Fragen enthält. Die Struktur der Tabelle ist wie folgt:

| Spalte | Eigenschaften |
|-----------|----------------------------------|
| id | int(8), unsigned, auto increment |
| Frage | varchar(255) |
| is_active | tinyint(1), ohne Vorzeichen, Standardwert: 1 |

Erstellen Sie die entsprechenden Formularfelder, um eine neue Zeile in diese Tabelle einzufügen. Nach dem Einfügen des Datensatzes muss eine FlashMessage abgefeuert werden, die über das erfolgreiche Einfügen des Datensatzes und die Weiterleitung zur Bearbeitung des Datensatzes (Aktion "Bearbeiten") informiert.

- Validierung, dass das Formularfeld ausgefüllt wurde
- Überprüfung, ob der Fragentext gemäß der Tabellenstruktur in den varchar passt
- Bestätigung, dass eine Frage mit diesem Text nicht mehr existiert
- Definieren Sie eine weitere benutzerdefinierte "Gruppentabelle", die Informationen über die Gruppen enthält. Bei der Erstellung einer Frage kann dann festgelegt werden, zu welcher Gruppe die Frage gehört. Sie müssen eine Sitzung zwischen den Tischen einrichten (beschreiben Sie, wie dies geschieht und wie die Sitzung eingerichtet wird).
- Welches Latte-Makro verwende ich zum Rendern des Formulars in der Vorlage (Standard-Rendering)?

8. ein Bearbeitungsformular im "Presenter", das als Komponente erstellt wird. Wir wollen die Standardwerte aus der Datenbank übernehmen, d.h. wir müssen die Daten auf eine bequeme Weise aus der Tabelle holen.
- Wie werden Sie vorgehen und welche Elemente des Codes benötigen wir?
- Wie werden Sie den Bezeichner des aktuell bearbeiteten Elements an das Formular übergeben?
- Wie setzen Sie das Formularelement auf seinen Standardwert?
- Wie können wir feststellen, dass ein Benutzer versucht, ein Element zu bearbeiten, das nicht in der Datenbank vorhanden ist, und wie können wir ihn angemessen darüber informieren?

9 Betrachten Sie die folgenden Daten, die aus einer Datenbank abgerufen wurden (unter Verwendung einer normalen Nette-Datenbank):

```php
$questions = $this->db->questions()->fetchAll();
```

- Wie können wir den Text aller Fragen als Aufzählung auflisten?
- Wie übergeben wir die Daten aus der Tabelle an die Latte-Vorlage?
- Welche Lattenmakros benötigen wir für die Auflistung der Artikel? Geben Sie eine spezifische Implementierung der Auflistung der Spalten "ID" und "Name" in dem Format an:

	*1024: Wie geht es Ihnen?
	*1025: Was hast du heute zu Mittag gegessen?

10 Nennen Sie ein Beispiel für mindestens 3 verschiedene Formularfelder, die in das Formular geschrieben werden:

```php
$form->add(tady bude příklad);
```

und erläutern Sie jeweils, wofür sie verwendet wird und welche Ausgabe sie liefert (Datentyp + Beispiel).


11. wir haben ein eingereichtes Nette-Formular.
- Wie bekommen wir alle Felder (Namen und Werte) übermittelt?
- Geben Sie ein Beispiel für die Ausgabe eines Feldes namens "Frage".
- Stellen Sie eine konkrete Code-Implementierung bereit, die das Array von Werten und Schlüsseln durchläuft und einen einzelnen String zurückgibt, der die gesamte Liste der Schlüssel und Werte enthält, so dass wir diesen String beispielsweise in einer Datenbank speichern oder per E-Mail versenden können (das Speichern und Versenden ist nicht Gegenstand der Aufgabe und wird nicht bewertet).


12. entscheiden Sie für jede Bedingung, ob das Ergebnis WAHR oder FALSCH ist:
- `1 > 0`
- `1 == 1`
- `1 == "1"`
- `1 === "1"`
- 1 == wahr
- "1 === wahr
- `1 === false`
- 1 == "1" && 1=== wahr`


13. welche Datentypen kennen wir in PHP?
- Nennen Sie mindestens 5 Beispiele für Datentypnamen
- Führen Sie für jedes Beispiel mindestens 3 mögliche Werte auf, die in dem Datentyp gespeichert werden können (wenn der Datentyp nicht so viele mögliche Werte speichern kann, schreiben Sie das)
- Geben Sie für jeden Datentyp ein typisches Beispiel für seine Verwendung an (was wird in der Praxis darin gespeichert)
- Was ist der Unterschied zwischen `==` (zwei Gleichen) und `===` (drei Gleichen)?
- Erläutern Sie den Nachteil der Verwendung von `==` in Bedingungen und wie speziell `==` dieses Problem löst (Beispiel, bei dem `==` versagen kann und `==` die Situation rettet)


14. eine Koordinierungstabelle (Koordinierungstabelle), die alle Koordinierungen zwischen 2 Personen auflistet. Einer von ihnen organisiert die Koordination, der andere ist ein Gast. Schreiben Sie eine Datenbankauswahl, die alle Zeilen mit Koordinierungen liefert, an denen ich beteiligt bin (bin ich der Organisator der Koordinierung oder bin ich der Gast der Koordinierung). Die Tabelle hat die Spalten `id`, `id_user_organizer` (Organisator-ID), `id_user_quest` (Gast-ID). Meine ID wird auf die übliche Weise in `Presenter` gespeichert.


15. eine Gruppe von Fragen über Latte:
- Was ist eine Latte?
- Was ist der Unterschied zwischen `Variable`, `Makro`, `Filter` und `n:Attribut`? Was wird wo verwendet?
- Wie erstelle ich einen Verweis von "ashboardPresenter" auf eine "standardmäßige" Aktion?
- Wie generiere ich bei der Auflistung einer Fragentabelle einen Link zu einer bestimmten Bearbeitung (`QuestionPresenter`, `edit` Aktion) einer Frage, um die ID der aktuell aufgelisteten Frage zu übergeben? Schreiben Sie einen speziellen Latte-Code.

Symbolisch geschrieben (Beispiel in PHP, in Latte übersetzen):

```php
foreach ($questions as $question) {
   echo $question->id; // Frage-ID
   echo $question->question; // Fragetext
}
```

- Wie schreibt man einen festen HTML-Raum?


16. wofür werden die Dienste in Nette verwendet?
- Wie wird sie instanziiert?
- Was muss ein Dienst tun, um genutzt zu werden?
- Wie lade ich einen Dienst in Presenter?
- Nehmen wir zum Beispiel den Dienst `StatisticManager`, der eine öffentliche Methode `getStatistics()` hat, die keine Parameter annimmt. Wie lade ich diesen Dienst in Presenter und rufe die öffentliche Methode "getStatistics()" in der Standardaktion auf und übergebe das Ergebnis an die Vorlage?
- Was ist der Unterschied zwischen `Objekt`, `Klasse` und `Dienst`?
- Was ist ein "Modell", eine "Einheit" und ein "Wertobjekt"?
- Alle Dienste haben einen Hauptmanager, der sie kennt und in diesem Manager abgeholt werden kann. Wie lautet der Name dieses Managers? Wie registriere ich einen neuen Dienst in der Datenbank?


17. neonfarben
- Was sind Neon-Dateien?
- Welche Arten gibt es und nach welchen Kriterien sind sie sortiert?
- Was enthalten die Neon-Dateien? Welche Daten sind darin gespeichert?
- Geben Sie ein Beispiel dafür, wie man das folgende Feld registriert (das Rezept ist in PHP, es muss übersetzt werden), damit es als Parameter verwendet werden kann:

```php
$imageGenerator = [
   "Punkte" => [
      480: [910, 30, 1845, 1150],
      600: [875, 95, 1710, 910],
      768: [975, 130, 1743, 660]
   ]
];
```

- Geben Sie ein Beispiel für die Registrierung eines Dienstes in Neon, dem wir den Parameter `imageGenerator` übergeben, den wir in der vorherigen Aufgabe registriert haben, so dass der Dienst ihn im Konstruktor erhält und ihn im Dienst verwenden kann (im Sinne der Konfiguration). Stellen Sie für den Dienst eine Beispielimplementierung des Konstruktors bereit, so dass der erste Eingabeparameter als Datentyp für das Array behandelt wird.


18. allgemeiner Gegenstand
- Was sind `Methode`, `Eigenschaften` und `Konstanten`? Worin besteht der Unterschied zwischen ihnen?
- Sowohl für Methoden als auch für Eigenschaften gibt es 3 grundsätzliche Zugänglichkeitsstufen (`public`, `private`, `protected`). Erläutern Sie den Unterschied und ein konkretes Beispiel für die Verwendung und wer was wann sehen kann.
- Ich habe eine Klasse `Kurs`, in der es eine private Eigenschaft `currentCourse` gibt, in der der aktuelle Kurs gespeichert ist. Wie kann man die Eigenschaft nur lesbar machen und nicht von außen schreiben?


Wenn ich in einer Datenbank Tabellen anlege, die logisch zusammenhängen (z. B. eine Tabelle für einen Benutzer und dann eine Tabelle für seine Artikel), muss ich dafür sorgen, dass die Daten korrekt verknüpft werden.
- Wie wird sichergestellt, dass die Daten in den Tabellen in der Datenbank korrekt verknüpft sind?
- Was ist referentielle Integrität und welche Rolle spielt sie in der Datenbank?
- Welche Arten von Sitzungen haben wir? Welchen Zweck erfüllen die einzelnen Typen?
- Welche Parameter setzen wir für Sitzungen, um das Löschen oder Ändern von Daten auf unterschiedliche Weise zu handhaben? Nennen Sie 3 Beispiele und spezifische Verwendungszwecke + eine Beschreibung der Funktionsweise.


20 Was ist der Zweck von Fabriken (OOP-Entwurfsmuster)?
- Nennen Sie ein Beispiel für eine Verwendung.
- Sind Nette-Dienste Fabriken?
- Was ist der Zweck von Dependency Injection?
- Was ist der Unterschied zwischen "DI" und "DIC"?
