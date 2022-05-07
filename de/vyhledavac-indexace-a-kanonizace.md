Internet-Suchmaschinen-Algorithmus - Indizierung und Kanonisierung
==================================================================

> id: daa97e66-e77f-4741-ade2-905c1cfdbd56
> slug:
> 	cs: vyhledavac-indexace-a-kanonizace
> 	de: internet-suchmaschinen-algorithmus---indizierung-und-kanonisierung
> 
> publicationDate: '2016-09-11 11:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d73a30f413e4b8b77dbceda60b3cf173

In der heutigen Lektion werden wir uns mit der Indizierung und Kanonisierung von Dokumenten im Internet beschäftigen.

Indizierung
--------

Der Indizierungsprozess wird von einer Komponente namens Indexer durchgeführt. Dies ist ein speziell entwickeltes Programm, das die heruntergeladenen Daten (die Daten, die der Crawler heruntergeladen hat) in einen speziellen Datentyp für die Suche umwandelt - Fässer.

Das Problem bei der Indizierung besteht darin, dass man die Dokumente nicht "intelligent" durchsuchen kann, sondern dass das sequenzielle Lesen (Lesen des gesamten Textes von Anfang bis Ende) unvermeidlich ist, so dass es sich um eine anspruchsvolle Disziplin handelt, für die Suchmaschinen die leistungsfähigsten Server einsetzen. Keine andere Aufgabe im Suchprozess ist so anspruchsvoll wie die Indizierung, bei der reiner Text zu Indizes wird.

Nehmen wir zum Beispiel eine Seite über Katzen, die von Wikipedia heruntergeladen wurde. Der Indexer erhält den kompletten Text der Seite und muss unnötige Dinge entfernen (z.B. Menüs zur Benutzersteuerung, Werbung, Fußzeilen, ...) und die Seite parsen, um den reinen Text zu erhalten. Der Text könnte zum Beispiel lauten:

> Die Hauskatze (Felis silvestris f. catus) ist eine domestizierte Form der Wildkatze, die schon seit Tausenden von Jahren ein Begleiter des Menschen ist. Wie ihre wilden Verwandten gehört sie zur Unterfamilie der Kleinkatzen und ist ein typischer Vertreter dieser Gruppe. Er hat einen geschmeidigen und muskulösen Körper, der perfekt an die Jagd angepasst ist, scharfe Krallen und Zähne sowie ein ausgezeichnetes Seh-, Hör- und Geruchssinnvermögen.

Auszug aus [Wikipedia](http://cs.wikipedia.org/wiki/Ko%C4%8Dka_dom%C3%A1c%C3%AD).

Die Suchmaschine analysiert nun die einzelnen Wörter aus dem Text und schreibt sie in einzelne Fässer. Es kann jedoch nicht direkt in die Tonne schreiben (vor allem, weil jede Tonne in vielen Kopien vorhanden ist, und auch, weil es die Suche stören würde), also erstellt es eine Liste von Anforderungen, wie die zukünftige Tonne aussehen soll, und speichert diese im temporären Speicher. In unserem Fall könnte eine solche Liste etwa so aussehen (einige unwichtige Wörter können ignoriert oder als Stoppwörter markiert werden):

| Dokument-ID | Wort | Position | Typ |
|--------------|-------|--------|-----------|
| 1 | Katze | 1 | Normal |
| 1 | Inland | 2 | Normal |
| 1 | Felis | 3 | Normal |
| 1 | Ist | 7 | StopWord |
| 1 | Domestiziert | 8 | Normal |

Eine solche Liste ist riesig (viel größer als die Originaltexte), aber sie ist trotzdem schneller zu durchsuchen, weil wir nicht alle Texte der Reihe nach lesen müssen, sondern nach Index in jeder Spalte suchen können und die Wörter einer Seite in Zeilen nacheinander sortiert werden.

Nachdem eine gewisse Zeit vergangen ist (oder eine bestimmte Anzahl von Dokumenten fertiggestellt wurde), stellt der Indexer die Arbeit an der Erstellung dieser Liste von Anforderungen (für einen zukünftigen Lauf) ein und beginnt erneut mit dem Lesen der Daten und dem Wiederaufbau der einzelnen Fässer (diese Liste enthält alte Datensätze, die sich in einem bereits funktionierenden Lauf befinden). Wenn neue Datensätze von bekannten Adressen hinzugefügt werden, werden diese aktualisiert, während sie bei neuen Dokumenten aufgenommen werden.

Auf diese Weise geht der Indexer die Liste erneut durch und erstellt nach und nach neue Fässer, die alle Elemente enthalten (sie sehen genauso aus wie im Beispiel in den vorangegangenen Kapiteln), und wenn alle Fässer fertig sind, werden sie an die einzelnen Suchserver gesendet. Die Aktualisierung der Fässer auf den Servern ist zeitaufwändig (vor allem wegen der riesigen Datenmengen, die bewegt werden), so dass die Server während dieses Vorgangs nicht verfügbar sind und die Daten auf verschiedenen Servern zu unterschiedlichen Zeiten aktualisiert werden. Dies führt zum Beispiel dazu, dass einige Nutzer unterschiedliche Ergebnisse erhalten, weil jeder Nutzer auf einem anderen Ausgabeserver sucht (aufgrund der Lastverteilung). Sobald die Aktualisierung abgeschlossen ist, ist alles wieder normal und alle Benutzer finden alle Dokumente gleichermaßen.

Der Indizierungsprozess ist für jede Suchmaschine wichtig, und diejenige, die ihn am häufigsten und sorgfältigsten durchführt, ist diejenige, die den aktuellsten Überblick über das Internet hat. Google führt diesen Vorgang alle paar Stunden durch, Seznam einmal pro Woche (und hat eine Million Mal weniger Daten).

Kanonisierung von Dokumenten
--------------------

Bei der ursprünglichen Entwicklung der Volltextsuchmaschine gab es keine Notwendigkeit für so etwas wie Kanonisierung, da das Internet ein Medium ist, das ständig neue Inhalte erstellt. Im Laufe der Zeit kam es jedoch zu einer Duplizierung (d. h. derselbe Inhalt erscheint unter mehreren verschiedenen URLs), und die Suchmaschinen müssen sich darauf einstellen. Ein typisches Beispiel ist Wikipedia, das viele Artikel enthält. Manche Autoren anderer Seiten übernehmen diese Texte (teilweise oder sogar vollständig) und schaffen so eine Vervielfältigung. In den meisten Fällen spielt dies jedoch keine Rolle, da die Quellseite einen viel höheren Rang (Linkqualität) hat als die plagiierte Seite, aber es kann manchmal vorkommen, dass es das Original auf Kosten des Plagiats herabsetzt.

Die Suchmaschinen mussten sich diesem Laster anpassen, und es wurde der Begriff "Kanonisierung" geprägt, der als "Entfernung" bestimmter Seiten aus dem Index verstanden werden kann. Dadurch werden übrigens die Indizes verkleinert, und die Suchmaschine muss nicht unnötigerweise immer wieder die gleichen Inhalte crawlen.

Duplikate werden von jeder Suchmaschine intern in 2 große Kategorien eingeteilt:

Natürliche Duplikate
-------------------

Diese ergeben sich aus dem natürlichen Verhalten des Internets und aus seinen Eigenschaften.

Die URL `http://mathematicator.cz` zum Beispiel hat wahrscheinlich den gleichen Inhalt wie die URL `http://www.mathematicator.cz` oder `http://mathematicator.cz/index.php`, da dies ein Standardverhalten des Apache-Servers (und des Internets im Allgemeinen) ist.

Wenn eine natürliche Duplikation gefunden wird, erstellt die Suchmaschine einen "kanonischen Satz", d. h. eine Gruppe von Seiten, aus der die Suchmaschine einen Vertreter auswählt, der bei der Suche hervorsticht. Wenn ein Link zu einer Seite aus dem kanonischen Satz führt, wird sein Rang automatisch an den Hauptvertreter weitergegeben.

Es ist oft eine gute Idee, der Suchmaschine bei der Erstellung dieses Satzes zu helfen und die Weiterleitung auf der Website korrekt einzurichten, was dazu führt, dass die Suchmaschine die Website besser betrachtet und der Hauptvertreter besser ausgewählt wird.

Duplikate, die zu Plagiaten führen
----------------------------

Plagiate sind heute ein Problem im Internet. Das Vorhandensein von Plagiaten an sich wäre nicht so wichtig, aber die Nutzer stören sich besonders daran, weil sie immer wieder dieselben Ergebnisse für dieselbe Anfrage finden. Haben Sie auch schon einmal mehrere Seiten mit identischem Text zu einer Anfrage gefunden? Dies ist genau das Verhalten, das die Suchmaschinen verhindern wollen.

Das größte Problem besteht darin, die Originalquelle zu ermitteln - und zwar maschinell. Auch hier fassen die Suchmaschinen alle ähnlichen Seiten in einem kanonischen Satz zusammen und wählen einen Hauptvertreter aus diesem Satz aus. Wenn die Quellen aus verschiedenen Bereichen stammen, kann die Situation nicht als natürliche Duplikation betrachtet werden (und ein beliebiger Kandidat ausgewählt werden), sondern alle Seiten müssen qualitativ bewertet und die beste Seite objektiv ausgewählt werden - und idealerweise die ursprüngliche Quelle des Inhalts.

Suchmaschinen treffen ihre Entscheidungen oft auf der Grundlage des Rangs der gesamten Domäne und der Stärke des Linknetzwerks zu einem bestimmten Dokument, aber selbst dies ist ein eher unzuverlässiger Ansatz. Der zweite Faktor ist in der Regel auch der Zeitpunkt der Erstellung (Indexierung) des Dokuments. Jede Suchmaschine merkt sich daher, welche Seiten häufig neue Inhalte generieren und besucht diese Seiten häufig, so dass sie die neue Seite im Idealfall sofort bemerkt und daher keine andere Seite als Stellvertreter wählt.

Eine detaillierte Beschreibung der Methoden, wie diese Auswahl funktioniert, würde den Rahmen dieses Papiers sprengen und könnte Gegenstand eines ganzen Buches sein.
