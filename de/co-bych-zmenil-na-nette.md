Was ich an Nette ändern würde, wenn ich David Grudl wäre
========================================================

> id: bad8fe4f-92a4-4396-8698-7ec42bb2aef8
> slug:
> 	- null
> 	de: was-ich-an-nette-aendern-wuerde-wenn-ich-david-grudl-waere
> 
> cs: co-bych-zmenil-na-nette
> publicationDate: '2022-11-18 17:45:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '953e6c78b9dff185bbf8e3e9582d24c3'

Ich verwende Nette Framework seit fast 6 Jahren aktiv für die kommerzielle Softwareentwicklung. In den ersten Jahren war ich sehr begeistert, und das Framework deckte alle Bedürfnisse, die wir als Team hatten, perfekt ab, und es gab im Grunde keinen Grund, nach einem anderen Tool zu suchen.

Seit etwa 2019 fange ich an, den ursprünglichen Enthusiasmus für Nette zu verlieren, und ich fange an, einige der erweiterten Funktionen zu vermissen. Oft handelt es sich dabei um Dinge, die über das Framework selbst hinausgehen, so dass ich nicht einmal erwarte, dass sie jemals implementiert werden, aber andererseits muss ein Entwickler eine Entscheidung darüber treffen, wie er die Entwicklung in Zukunft fortsetzen will, und vielleicht eine andere Technologie wählen.

Datenbank-Ebene
-----------------

Ich halte Nette Database für einen der größten Fehler des Frameworks, leider.

Jede Woche haben wir Vorstellungsgespräche im Unternehmen, bei denen sich junge Leute, die gerade die Schule abgeschlossen haben, oder auch Quereinsteiger, die von einem anderen Framework kommen, bewerben und Nette Database als Teil ihrer Erkundung von Nette entdecken. Als Datenbankschicht ist sie gar nicht so schlecht, aber das Problem ist dann der praktische Einsatz in echten Projekten.

Das liegt daran, dass Nette Database keine Anstrengungen unternimmt, um Entwickler zu zwingen, von Anfang an strikte Datentypen zu verwenden, Spalten und Relationen zu benennen, Abfrage- (Repository-) Methoden von Modellen zu trennen und andere kleine Nuancen, die insgesamt viel bewirken.

Viele Jahre lang habe ich die Dibi-Bibliothek verwendet, die zu ihrer Zeit eine große Rolle spielte. Damit konnte man die Datenbank sehr gut abfragen und die Schnittstelle vereinheitlichen. Andererseits haben sich die Zeiten geändert, und wenn Datenbanken untypisierte Objekte zurückgeben, ist PHPStan schon aus Prinzip nicht glücklich.

Ich persönlich würde entweder native Unterstützung für Entitäten und Repositories in die Nette-Datenbank einbauen oder offizielle Unterstützung für Doctrine in Nette anbieten. Wenn Sie Anwendungen auf der Ebene eines großen Unternehmens programmieren, müssen Sie es mit der Entwicklung ernst meinen, und insbesondere Doctrine ist sehr sinnvoll. Gleichzeitig möchten Sie Neueinsteigern sofort eine bessere Technologie vermitteln und ihnen keine alten Gewohnheiten beibringen.

Tracy und Fehlerprotokollierung
---------------------

Ich halte Tracy immer noch für den besten Laufzeit-Debugger für PHP.

Als ich 2017 zu einer ungenannten Digitalagentur kam und den technischen Stand ihrer Nette-Projekte sah, war ich entsetzt. Wie oft hatten sie Websites, die in reinem PHP geschrieben waren, ohne jeden Versuch, Fehler zu protokollieren. Eine relativ einfache Lösung war es, Tracy auf dem Projekt zu installieren, `Debugger::enable()` aufzurufen und nichts weiter zu tun. Die Fehler wurden automatisch in einem Verzeichnis gespeichert, das nur alle paar Tage manuell abgeholt werden musste.

Was Tracy betrifft, so scheint es mir, dass es sich um ein fertiges Produkt handelt, an dem David kaum noch etwas zu verbessern hat. Andererseits sehe ich noch Raum für Verbesserungen in einer neuen Richtung.

Warum enthält Tracy zum Beispiel keine kostenpflichtigen Funktionen für Unternehmen oder Einzelpersonen, die es mit der Sicherheit ernst meinen?

Tracy könnte eine benutzerdefinierte Sentry-ähnliche Weboberfläche implementieren, bei der Sie ein Konto anlegen und Produktionsfehler automatisch über eine API gesendet werden, über die Sie sie projektübergreifend verfolgen können. Die App könnte auch einzelne Websites abfragen und automatisch erkennen, dass diese nicht verfügbar sind, und den Eigentümer auf andere Weise benachrichtigen (da Tracy einen Serverabsturz nicht logisch erkennen kann). Manchmal stoße ich auch auf das Problem, dass Tracy versehentlich auf einer Produktionsseite aktiviert ist, und Sie können über DIC oder anderswo herausfinden, wie Sie auf die Datenbank zugreifen können. Tracy sollte Sie auch darauf aufmerksam machen.

Tracy könnte auch andere kleine Dinge implementieren, die ich aus Node.js kenne. Für die Anzeige des Quellcodes könnte es beispielsweise möglich sein, das Zeichenintervall, in dem die Zeile hervorgehoben wird, auf besondere Weise zu wählen, weshalb die Möglichkeit besteht, ein bestimmtes Token innerhalb einer Zeile hervorzuheben. Gleichzeitig wäre es schön, wenn eine zweite (vielleicht blaue) Hervorhebungslinie hinzugefügt werden könnte, um den Kontext im nächsten Teil der Anwendung anzuzeigen.

Bei der Anzeige eines Quellcodeausschnitts würde ich mich nicht scheuen, eine Schaltfläche hinzuzufügen, die den Rest des Codes in Ajax wiedergeben kann, so dass ich ihn direkt im Browser aus dem Callstack heraus erkunden kann. Das ist es, was Symfony tut, und es ist eine großartige Funktion. Wenn dies durch eine grundlegende statische Analyse mit der Möglichkeit, Methoden, Klassen und Vererbung zu durchsuchen, ergänzt würde, würde dies dem gesamten Team eine Menge Debugging-Arbeit ersparen.

Wenn der Callstack ein Paket durchläuft, wäre es schön, die Version des Pakets anzuzeigen, mit dem ich für eine bestimmte Datei arbeite. Oft erhalte ich in einem Paket, das ich entwickle, einen Fehler, und ich könnte genauso gut visuell wissen, dass es sich um eine ältere Version handelt.

Statisches Routing
----------------

Innerhalb von Nette Router würde ich es erlauben, einen neuen Typ namens statischer Router zu definieren. Er wäre ein Abkömmling des Basis-Routers, könnte aber nur string-literal akzeptieren, was kein regulärer Ausdruck wäre, sondern ein gerader Pfad. Viele Seiten arbeiten statisch (Kontakte, verschiedene Artikel, erstaunlich oft sogar die Homepage), und der Router muss deshalb jedes Mal Regex-Regeln für die Übereinstimmung und URL-Zusammensetzung auswerten.

Wenn beispielsweise eine statische Route in einer Latte-Vorlage verwendet wird, könnte sie zur Kompilierungszeit der Vorlage sicher in eine statische Zeichenkette "{$basePath}/path" übersetzt werden, so dass die 100 URLs, die sich beispielsweise im Layout befinden, nicht bei jeder Anfrage kompiliert werden müssen.

Ich weiß, dass ich die gleiche Funktionalität durch Caching auf der Vorlagenseite erreichen kann, aber ich würde eine Systemlösung viel mehr schätzen.

Im Next.js-Framework habe ich mich völlig in das Konzept des statischen Routings verliebt. Es gibt kein "n:href", d.h. man muss die URL im Voraus kennen, was einen dazu zwingt, nicht so viele Umleitungen vorzunehmen, sich im Voraus über die URL-Formate Gedanken zu machen und sie beständig zu halten.

PSR-Schnittstelle
------------

Ich finde es schade, dass Nette die PSR-Schnittstelle nicht nativ implementiert. Als Paketentwickler führt Sie dies in die Irre, wenn Sie überhaupt eine Composer-Abhängigkeit von Nette definieren wollen. Einerseits löst Nette viele Probleme für Sie, andererseits wissen Sie, dass Sie es danach einfach nicht mehr ersetzen werden. Mehr als einmal habe ich mich dabei ertappt, dass ich lieber Symfony, Laravel oder eine ganz andere generische Schnittstelle verwendet habe, weil es keine generische Schnittstelle gibt.

Die fehlende Unterstützung generischer Standards für die künftige Portabilität ist der Hauptgrund, warum ich Nette 2022 komplett verlassen habe und wir neue Anwendungen in Teams ausschließlich in generischer Sprache entwickeln.

API-Schicht
----------

Was ist, wenn Sie Nette als Backend für die Bearbeitung von REST-API-Anfragen verwenden möchten? Erstellen Sie einen Presenter und senden Sie die Antwort als `$this->sendJson()`? Werden Sie eine Bibliothek eines Drittanbieters installieren? Oder sind Sie David Grudl, der den Bedarf an einer fehlenden Bibliothek löst, indem er gleich eine neue schreibt?

Warum kann Nette nicht etwas so Alltägliches wie die REST-API gleich von Anfang an implementieren? Heutzutage muss fast jede Anwendung über eine API kommunizieren - zum Beispiel, um Daten für das Frontend bereitzustellen.

Dies führt wiederum dazu, dass Neulinge die integrierte Latte und die Snippets verwenden, anstatt herauszufinden, dass es eine bessere Möglichkeit gibt, nämlich das gesamte Frontend separat zu erstellen und ihm lediglich Daten zuzuführen.

Vertrauen in das, was in 10 Jahren passieren wird
-------------------------

Der letzte Punkt ist sehr pragmatisch und entstammt einer Debatte, die wir von Zeit zu Zeit in den Geschäftsabteilungen des Teams hören.

Was würde passieren, wenn David Grudl die Entwicklung von Nette ganz einstellen würde? Was wäre, wenn jemand aufhören würde, sie zu entwickeln? Worauf würden wir umsteigen? Und wie schwierig wäre das?

Viele Entwickler (die oft keine Erfahrung mit Unternehmen haben) machen sich über diese Bedenken lustig und sagen, dass das schon in Ordnung ist. Ja, aber es ist nicht so.

Wenn Sie sich mit der Frage auseinandersetzen, ob Sie Zeit in Nette oder React und Node.js investieren sollen, um junge Entwickler in Ihrem Unternehmen auszubilden, gewinnt leider die letztere Option.

Nette hat den Zug noch nicht verpasst, aber in den Dutzenden von Gesprächen, die ich in den letzten sechs Monaten geführt habe, spüre ich das sehr deutlich.
