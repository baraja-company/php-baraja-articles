Unveränderlichkeit von Objekten - ein zentrales Entwurfskonzept
===============================================================

> id: '057467db-4e3b-4e18-9ea5-dfb25feb3800'
> slug:
> 	cs: immutabilita
> 	de: unveraenderlichkeit-von-objekten-ein-zentrales-entwurfskonzept
> 
> publicationDate: '2022-07-24 15:00:00'
> mainCategoryId: ae4c1c70-11b3-433e-b1d0-e590155bb8b9
> sourceContentHash: '88c26f35883c860426b4708f0be8761f'

Unveränderlichkeit ist eines der wichtigsten Designkonzepte für die Entwicklung stabiler Anwendungen. Das Grundprinzip besagt, dass ein einmal geschriebener Zustand später nur noch gelesen werden kann, ohne die Möglichkeit, ihn zu verändern. Wenn wir den Zustand ändern wollen, müssen wir eine neue Instanz erstellen und das gesamte Objekt durch ein anderes ersetzen.

Die Datentypen lassen sich daher ganz grob in zwei Kategorien einteilen:

- **Mutable** (veränderbarer Zustand innerhalb einer einzelnen Instanz)
- **Immutable** (unveränderlicher interner Zustand)

Veränderbare Objekte können intern geändert werden. Das heißt, sie stellen Operationen zur Verfügung, die, wenn sie in verschiedenen Kombinationen aufgerufen werden, zu unterschiedlichen Ergebnissen führen. Die Unveränderlichkeit versucht, dieses Verhalten zu verhindern.

Definition
--------

> Eine Klasse ist genau dann **unveränderlich**, wenn die Instanzdaten nach der Erstellung der Instanz in keiner Weise verändert werden können.

Alle Daten werden also im Konstruktor festgelegt. Alle skalaren Datentypen sind auch automatisch unveränderlich.

Ein großer Vorteil
--------------

Die Entwicklung von Anwendungen mit unveränderlichen Zuständen bietet einen grundlegenden Vorteil für die Sicherheit der Ausführung von Operationen. Wenn wir wissen, dass die einmal geschriebenen Daten später nicht mehr verändert werden können, können wir zum Beispiel sehr zuverlässig debuggen oder die Anwendung in Unterfunktionen aufteilen, ohne Gefahr zu laufen, einen der Zwischenzustände zu vergessen.

Die Idee der Unveränderlichkeit ist im Allgemeinen gegen das Prinzip der Speicherung von Zuständen in Eigenschaften von Objekten/Entitäten gerichtet und beschreibt eher einen funktionalen Ansatz, bei dem Daten einfach durch die Anwendung "fließen", wie es beispielsweise bei Javascript der Fall ist.

Unter dem Gesichtspunkt der Leistung können wir von unveränderlichen Objekten automatisch sagen, dass sie unbegrenzt zwischengespeichert werden können, da sie nie veraltet sein werden.

Ein reales Beispiel aus PHP
--------------------

Die bei weitem häufigste Verwendung von unveränderlichen Objekten in PHP ist das `DateTimeImmutable`-Objekt, das, sobald es erstellt wurde, nur von Formatierungsmethoden aufgerufen werden kann. Wenn wir die internen Einstellungen ändern, wird die Methode eine neue Instanz zurückgeben. Diese Funktion ist von entscheidender Bedeutung, wenn ein ORM verwendet wird, der das so genannte Identitätsmuster nutzt. So kann beispielsweise gewährleistet werden, dass das Erstellungsdatum einer Bestellung überall in der Anwendung gleich ist und die referentielle Integrität nicht beeinträchtigt wird.

Ein konkretes Beispiel für ein veränderbares Objekt:

```php
$date = new DateTime('2021-05-14');
$tomorrow = $date->modify('+1 Tag');
echo $date->format('Y-m-d'); // 2021-05-15
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

Es wurde dasselbe Datum gedruckt, weil die Methode `modify()` nur den internen Zustand des Objekts `DateTime` geändert und dieselbe Instanz zurückgegeben hat. Es gab also keine so genannte **Mutation des internen Zustands**, die ein grundlegendes Verhalten der objektorientierten Programmierung ist. Durch die Aktualisierung der Variable wurde auch die ursprüngliche Variable geändert.

Und nun ein Beispiel für ein unveränderliches Objekt:

```php
$date = new DateTimeImmutable('2021-05-14');
$tomorrow = $date->modify('+1 Tag');
echo $date->format('Y-m-d'); // 2021-05-14
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

Das Objekt "DateTimeImmutable" ist unveränderlich, was bedeutet, dass sich sein interner Zustand nie ändert. Eine neue geänderte Instanz (ebenfalls unveränderlich) wird in der Variablen gespeichert, nachdem die Methode `modify()` aufgerufen wurde. Wenn wir den neuen Wert nicht in der Variablen speichern würden, wäre er später nicht mehr verwendbar.

Der Originalwert wird nicht berührt und bleibt sicher aufbewahrt.

Wann sollte eine Klasse unveränderlich sein?
---------------------------

Wenn Sie keinen sehr guten Grund haben, sie veränderbar zu machen, schreiben Sie eine Klasse oder Funktion immer als unveränderbar. Dies wird Ihr Design in Zukunft vereinfachen.

Veränderliche Klassen sollten sich so wenig wie möglich ändern. Ich empfehle immer, das Verhalten der Unveränderlichkeit zu dokumentieren.

Der einzige Nachteil der Unveränderlichkeit ist vielleicht, dass bei jeder Attributänderung eine neue Instanz erstellt werden muss, was sich geringfügig auf die Leistung auswirkt. Wenn Ihre Anwendung (wie die meisten Anwendungen) dazu neigt, Daten anzuzeigen und weniger häufig zu ändern, ist dieser Nachteil bei der Leistung heutiger Computer eher unbedeutend.

Welche Arten von Daten sollten unveränderlich sein?
------------------------------------

- Alle Identifikatoren und eindeutigen Codes
- Die meisten ManyToOne- und OneToOne-Datenbanksitzungen
- Daten, Zeiten, Kalenderwerte
- Durchlaufen von Arrays, bei denen für jedes Element die gleiche Operation durchgeführt werden soll
- Intervalle, Paare, Dreiergruppen, ...
- Geometrische Figuren, Punkte, Linien, GPS-Koordinaten, physische Adressen, ...
- Protokolle und historische Aufzeichnungen
- Informationen über bearbeitete Aufträge und die meisten Finanzdaten
- Metadaten über die verbundene Entität

Was nicht unveränderlich sein sollte:

- Große Objekte mit vielen Eigenschaften
- Die meisten tabellarischen Ausgaben aus einer Datenbank, wie z.B. Doctrine Entities
- Nach und nach konstruierte Objekte aus kleineren Teilen
