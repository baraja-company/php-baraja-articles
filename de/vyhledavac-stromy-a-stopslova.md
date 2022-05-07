Algorithmus für Internet-Suchmaschinen - Trees und StopLead
===========================================================

> id: '11ec73f2-e505-4338-89aa-8307a55b7ba0'
> slug:
> 	cs: vyhledavac-stromy-a-stopslova
> 	de: algorithmus-fuer-internet-suchmaschinen---trees-und-stoplead
> 
> publicationDate: '2016-09-11 10:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d011d2d6943271ca5e45c3d3cdf03d61

Jede Sekunde werden dem Internet 5 Millionen neue Seiten hinzugefügt, und diese Zahl nimmt ständig zu. Um dieses riesige Meer an Informationen zu ordnen und etwas darin zu finden, gibt es Suchmaschinen. Die folgende Arbeit soll in die Thematik der Suche einführen und den gesamten Prozess von der Erstellung einer neuen Seite bis zu ihrer Auffindbarkeit in einer Suchmaschine erklären.

Es ist nicht einfach, eine Menge von Milliarden von Dokumenten zu finden und zu sortieren. Allein Google benötigt 300.000 Webserver, um diese Aufgabe in wenigen Stunden zu bewältigen. Tatsächlich findet die Suche nach Ihrer Anfrage statt, lange bevor Sie sie überhaupt stellen. Google hat die Suchergebnisse, nach denen Sie in den nächsten Tagen fragen werden, bereits im Speicher.

Architektur der Suchmaschine
------------------------

<img src="{$baseUrl}/images/fulltext-schema.png" alt="Gemeinsame Suchmaschinenarchitektur für bis zu 50 Millionen Dokumente" class="w-100 mb-3">

Eine gut konzipierte Suchmaschine besteht aus vielen Komponenten, von denen es mehrere hundert gibt. Dieses Diagramm beschreibt die grundlegendsten Elemente, die eine Suchmaschine mindestens enthalten muss, um zu funktionieren. Dies ist eine alte und nicht mehr verwendete Architektur, die bis etwa 2005 funktionierte, als es nur ein paar Millionen Dokumente im Web gab. Diese Architektur erlaubt keine "unendliche" Menge an Inhalten und auch keine möglichen Ausfallzeiten der Suchserver (Basissuche). Sollte eine einzelne Komponente ausfallen, würde das gesamte System nicht mehr funktionieren und die Suchmaschine wäre nicht mehr verfügbar.

Eine vollständige Beschreibung der aktuellen Trends bei der Entwicklung neuer Architekturen würde den Rahmen dieses Papiers sprengen, da diese in der Regel Betriebsgeheimnisse großer Unternehmen sind. In diesem Beitrag soll erläutert werden, wie diese grundlegende Architektur funktioniert. Die einzelnen Komponenten werden in den folgenden Kapiteln ausführlich erläutert.

Eingabe abfragen
------------

Die auch für den normalen Benutzer sichtbarste Komponente ist die Abfrageeingabe, die derzeit meist als Textfeld dargestellt wird. Das Eingabefeld befindet sich auf einer speziellen Seite, die normalerweise nur für Eingaben vorgesehen ist.

In der nächsten Phase gibt der Benutzer den Suchbegriff ein und sendet das Formular an die Server der Suchmaschine, wodurch die Kommunikation mit der Ausgabekomponente, auch Hauptsuche genannt, eingeleitet wird. Die Ausgabeserver sind für die Bewältigung großer Nutzerzahlen ausgelegt und müssen in der Lage sein, alle Suchenden gleichzeitig zu bedienen.

Die Architektur eines Versandservers ist in der Regel ein einfacher Apache-Server mit einer Basisinstallation und ausgezeichneter Netzwerkbandbreite. Wenn eine Abfrage auf dem Server eingeht, wird sie durch Regressionsentscheidungsbäume verarbeitet, und es wird eine "Abfragekarte" erstellt, die die vollständige Semantik der anderen Bedeutungen erfasst, um zu verdeutlichen, wonach der Benutzer tatsächlich sucht. Methoden zum Umschreiben von Abfragen würden den Rahmen dieses Artikels sprengen, daher beschränken wir uns auf allgemeine Beschreibungen, wie das Umschreiben aussehen kann und wie nicht.

Betrachten Sie die folgende Abfrage:

```txt
"O pejskovi a kočičce"
```

Diese Abfrage wird in einen binären Baum umgewandelt, der ihre semantische Essenz erfasst:

<img src="{$baseUrl}/images/fulltext-tree.png" alt="Symbolisches Parse-Baumdiagramm" class="w-100 mb-3">

Der Sinn eines Parse-Baums besteht darin, zu erfassen, wie eine Suchmaschine die Suchergebnisse betrachtet. Dieser Baum wird zusammen mit der Anfrage an die Hilfssuchserver (in dieser Arbeit als Basissuche bezeichnet) gesendet, wo nach einzelnen Stichwörtern gesucht (Indexlesungen) und dann nach Regeln (nach Operationen) sortiert wird.

| Operator | Sortiervorgänge |
|----------|------------------
| AND | Schnittpunkt |
| OR | Summe |
| NOT | Komplement |

Es gibt keine eigentliche Sortierung der Suchergebnisse, diese Komponente führt lediglich schnelle Binäroperationen auf riesigen Datenmengen (oft Millionen von Datensätzen) durch und vergleicht im Grunde nur die IDs der einzelnen Dokumente.

Wie die eigentliche Sortierung der Suchergebnisse auf der Ergebnisseite funktioniert, wird in den folgenden Abschnitten erläutert. Der Ausgabeserver wird einfach dazu verwendet, viele Daten von verschiedenen Suchservern zu kombinieren, um die Gesamtleistung zu erhöhen und die Last zu verteilen, und speist dann die ermittelten Werte in die Ergebnisseite ein (dies wird als "Webseite" bezeichnet), die zusammengesetzte Informationen von Dutzenden von Servern enthält.

Umschreiben in einen Binärbaum
-----------------------

Wie im vorigen Kapitel erwähnt, basiert die gesamte Suche auf Binärbäumen, die Ihnen sagen, wie die Ergebnisse aussehen und wonach Sie suchen müssen. Die gesamte "Logik" und "Intelligenz" einer Suchmaschine hängt direkt von der Qualität des Programms ab, das den Baum erstellt - nämlich dem Deskriptor.

Ein richtig konstruierter Baum sollte alle notwendigen Metainformationen über die Anfrage in einer Form enthalten, dass er auch mit Hilfe von sequentiellem Plattenlesen leicht verarbeitet werden kann und zufällige Speicherzugriffe ausschließt, also in der Regel die einzelnen Suchwörter (vom Benutzer), ihre Umschriften (und buchstabierten Formen) und nicht zuletzt die Verbindungen zwischen den Wörtern, d.h. ihre relativen Abstände im Text, enthält.

Transkriptionsmethoden für Abfragen
---------------------

Die einfachste Art, eine Abfrage zu transkribieren, besteht darin, sie in einzelne Wörter zu zerlegen (entsprechend den Leerzeichen), mit denen dann weiter gearbeitet wird. Der zweite Schritt besteht darin, nach StopWords zu suchen und sie durch einen variablen Zeiger zu ersetzen (denn, wie wir später zeigen werden, ist es sinnlos, überhaupt nach StopWords zu suchen).

Nehmen wir die folgende Abfrage: "Über einen Hund und eine Katze", so sieht die Transkription im einfachsten Fall so aus:

```txt
"O (and) pejskovi (and) a (and) kočičce"
```

Der zweite Schritt besteht darin, Wörter zu eliminieren, die für die Suche nicht relevant sind. Natürlich ist z. B. das Wort "über" oder "und" für uns Menschen von Bedeutung, aber nicht für eine Datenbanksuche, da es durch ein anderes Wort ersetzt und die Ergebnisse aufgelistet werden können. Das Ziel sollte nicht sein, eine wortwörtliche Übereinstimmung der Abfrage mit dem Index zu finden, denn das würde zu schlechten Ergebnissen führen, da Wörter in Texten im Internet oft in verschiedenen Schreibweisen vorkommen, und diese Methode versucht, Ergebnisse in anderen Formen zu finden als die, die wir vom Benutzer erhalten haben. Dies ist also der erste Hinweis auf eine "intelligente" Suchmaschine.

Diese bedeutungslosen Wörter werden oft als "Stoppwörter" bezeichnet. Jede Suchmaschine sollte einen Index solcher Wörter führen und diesen Index regelmäßig (manuell) aktualisieren.

```txt
["pejskovi (and) * (and) kočičce"]
```

An dieser Stelle suchen wir nach 2 verschiedenen Wörtern ("Hund" und "Katze") mit einem zusätzlichen Wort dazwischen (intern wird es mit einem Sternchen markiert).

Der Zweck dieser Änderung ist nicht, die Qualität der Suche zu verbessern, sondern die Leistung zu steigern. Wenn wir nach einem Wort suchen, das zu häufig vorkommt, kommt es zu einer schnellen Auslastung, und diese Änderung hilft dem Prozess - vor allem dadurch, dass nicht nach einem Wort gesucht wird, das in den meisten Dokumenten ohnehin an dieser Stelle vorkommt.

Es ist auch wichtig zu beachten, dass wir diese Anpassung (Weglassen eines Wortes) nicht immer vornehmen können, so dass jede Suchmaschine einen separaten Index der im Internet gefundenen Phrasen führen sollte, um zu prüfen, welche Anpassung zu guten Ergebnissen führt und welche nicht. Gelegentlich nimmt eine Suchmaschine diese Anpassung jedoch auch dann vor, wenn sie den Begriff nicht in ihrem Index hat - vor allem dann, wenn ein Nutzer nach einem wichtigen Wort sucht, von dem erwartet wird, dass es auf den ersten Positionen wichtige Seiten gibt, was diesen "Fehler" übertrumpft, da die Nutzer diese in der Regel ohnehin anfordern.

Der letzte Schritt besteht darin, mit der englischen Sprache zu arbeiten und die Abfrage von unnötigen Zeichen zu "säubern". Bei der Abfrage "Waschmaschine" zum Beispiel erwartet der Nutzer in der Regel nur Ergebnisse für das Wort "Waschmaschine", und wir können das Kommazeichen ignorieren.

Die genauen Methoden, wie dieses System funktioniert, sind nicht bekannt, und jede Suchmaschine hat ihre eigenen. Man geht davon aus, dass es sich größtenteils um ein statistisches Modell handelt, das diese Anpassungen auf der Grundlage der Kenntnis von Milliarden von Texten in der Datenbank vornimmt.

Die englische Transkription ist ebenfalls eine Wissenschaft für sich, so dass auch hier nur eine grundlegende Beschreibung gegeben werden kann. In den meisten Fällen reicht es aus, jedem Wort seine (laut Wörterbuch) mit Bindestrich versehenen Formen hinzuzufügen und zusammen mit dem Wort zu suchen. Dadurch wird erreicht, dass die Suchmaschine Dokumente finden kann, in denen die Wörter nicht nur in ihrer Grundform (wie vom Benutzer eingegeben) vorkommen, sondern auch die flektierten Versionen - eine sehr nützliche Funktion. Das Problem bei diesem Ansatz liegt in der Flexion von unklaren und problematischen Wörtern sowie in der Flexion von kurzen Anfragen (bei denen es nicht möglich ist, aus dem Kontext zu ermitteln, wie das Wort zu flektieren ist). Das Wort "Spiele" soll ein Beispiel für eine problematische Flexion sein.

Bei einer englischen Anfrage "I love her" wird ein schlecht konzipierter Flexionist das Wort "games" beispielsweise als "games, gaming, gaming room, ..." flektieren, so dass es auch notwendig ist, die umgebenden Wörter als Phrasen zu indizieren und die Flexion nur in vertrauten Situationen durchzuführen oder ein anderes Verfahren zu verwenden (hier kommen oft phonetische Algorithmen ins Spiel).

Der letzte Schritt besteht darin, den fertigen Baum an die Base-Suchserver zu senden, wo die eigentliche Fass-Suche stattfindet - dies ist das Thema des nächsten Kapitels.
