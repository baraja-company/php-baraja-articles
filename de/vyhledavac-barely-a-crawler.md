Internet-Suchmaschinen-Algorithmus - Kaum ein Crawler
=====================================================

> id: '5ad042bf-7fda-4b2e-a38c-762169d0b594'
> slug:
> 	cs: vyhledavac-barely-a-crawler
> 	de: internet-suchmaschinen-algorithmus---kaum-ein-crawler
> 
> publicationDate: '2016-09-11 12:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '5daf3854688c39f9e0071a93ab11dd4e'

In der heutigen Lektion werden wir über Datenfässer, ihre Struktur, StopSlovas und schließlich über Crawler sprechen.

Datenfässer
-------------

Hierbei handelt es sich um einen speziellen Datentyp, der sich auf mehreren Servern gleichzeitig in mehreren Kopien befindet. Dabei handelt es sich in der Regel um datenintensive Dateien von Hunderten von GB, die nur langsam gelesen werden können (weshalb sie in Teile aufgeteilt sind) und praktisch nicht bearbeitet werden können. Wenn wir auch nur eine minimale Änderung vornehmen wollen, müssen wir das ganze Fass neu berechnen. Die Suchmaschine Seznam zum Beispiel kann Datenfässer höchstens alle paar Tage oder Wochen neu berechnen, während Google alle paar Stunden eine Neuberechnung vornimmt (und zwar nur einige Teile, nie das Ganze auf einmal).

Die Fässer enthalten Wörter und deren Vorkommen im Internet. Man kann sagen, dass es sich hierbei um Rohdaten für Suchergebnisse handelt, die noch nicht sortiert sind (manchmal sind sie teilweise nach relativer Wichtigkeit sortiert) und im Voraus aufbereitet wurden. Die eigentliche Suche findet statt, lange bevor der Nutzer die Suchanfrage stellt, und hier werden alle Ergebnisse aufbereitet. Die Suche selbst wäre nicht in Echtzeit möglich, so dass die Suchmaschine im Grunde die hier bereits vorbereiteten Ergebnisse liest (die Suche wird von der Indexer-Komponente durchgeführt, die in einem separaten Kapitel behandelt wird).

Die Fässer enthalten alle grundlegenden Informationen, die für die Erstellung von Suchergebnissen für jedes im Internet vorkommende Wort benötigt werden, so dass beim sequenziellen Lesen Leistungsprobleme behoben werden müssen. In der Praxis werden die Fässer in der Regel in Stapeln auf mehreren Servern und für einen schnelleren Zugriff auch im RAM gespeichert. Die Google-Suchmaschine beispielsweise liest die Fässer überhaupt nicht von der Festplatte, sondern speichert sie vollständig im RAM und behält nur eine Kopie davon auf der Festplatte, falls Daten aus dem RAM verloren gehen.

Große Suchmaschinen mit ausreichenden Ressourcen halten auch so genannte "goldene Fässer" vor, die die wichtigsten Seiten im Internet enthalten und bei großem Suchaufkommen durchsucht werden. Wenn z. B. mehrere Suchserver ausfallen, werden die Goldfässer vorübergehend für die Suche genutzt. Die Suchmaschine findet zwar nicht alle Ergebnisse, aber zumindest wird die Suche nicht vollständig unterbrochen, sondern nur die Anzahl der gesuchten Dokumente vorübergehend eingeschränkt. Außerdem ist es notwendig, die Fässer regelmäßig zu aktualisieren, da die Zahl der Seiten im Internet ständig zunimmt. Da die Fässer in der Regel in der Größenordnung von Gigabytes liegen, ist es nicht möglich, eine inkrementelle Wartung durchzuführen, sondern es ist notwendig, das Ganze einmal zu einem bestimmten Zeitpunkt neu zu erstellen. Daher führt die Suchmaschine ein Hilfsfass, in dem alle Daten in Form einer großen Tabelle vorliegen, aus der die Fässer aufgebaut werden - Google beispielsweise baut etwa alle 12 Stunden Fässer auf. Die große Herausforderung besteht gerade darin, dass die Fässer zumindest teilweise sortiert sein und den tatsächlichen Zustand des Internets widerspiegeln müssen. Solange die Suchmaschine die Fässer nicht aktualisiert, findet der Nutzer also keine neuen Webseiten.

Struktur des Fasses
----------------

In der Praxis wird ein Lauf als große Textdatei gespeichert, die die ID jedes Dokuments enthält, in dem das gesuchte Wort vorkommt, sowie die Position des Vorkommens des aktuell gesuchten Worts.

Beispiel für ein sehr kleines Fass mit drei Seiten:

```txt
[1:5,8,12,27,30;5:1,3,6;12:4,8,10]
```

Dieses Beispielfass enthält Seiten mit den Bezeichnungen "1", "5" und "12". Die zugehörigen Zahlen geben die Position des Wortes im Dokument an. Ein solcher Lauf erfasst nur die Vorkommen eines einzigen Wortes, so dass jedes Wort im Internet seinen eigenen Lauf hat. Bei der Suche nach mehreren Stichwörtern müssen mehrere Fässer gelesen werden (für jedes Wort ein anderes) und dann die Operationen entsprechend dem Binärbaum durchgeführt werden (mehr dazu in den vorherigen Kapiteln).

Die Überschneidung erfolgt in der Regel dadurch, dass eines der Dokumente durchgesehen und das andere durchsucht wird. Wenn die Fässer groß sind, werden sie möglicherweise nicht vollständig gelesen, so dass Suchmaschinen oft nur einen Teil davon lesen können und dann das Ergebnis zurückgeben müssen. Daher ist es sinnvoll, die Fässer zumindest teilweise zu sortieren, so dass die wichtigsten Seiten (nach denen der Nutzer wahrscheinlich sucht) am Anfang des Fasses stehen und alles andere am Ende des Fasses.

StopLead
---------

Sie werden vielleicht denken, dass die Fässer bei manchen Wörtern sehr groß und daher schwer zu bearbeiten sind. Das Wort "a" oder das Wort "1" kommt beispielsweise in mehr als der Hälfte der Dokumente vor, ist aber für den Suchenden bedeutungslos (weil es selten gesucht wird und mehr Umgebungswörter benötigt). Solche Wörter werden als Stoppwörter bezeichnet.

Das Problem bei StopWords ist, dass sie nicht alle gesucht werden können und Suchmaschinen daher nur ein verzerrtes Bild dessen zeigen, wie das Internet für den Suchbegriff tatsächlich aussieht.

Ein Beispiel für eine Suche mit StopSlovy ist die Abfrage `Prag 1`.

Die Suchmaschine liefert nur 3.020.000 Ergebnisse für diese Anfrage. In Wirklichkeit gibt es jedoch viel mehr solcher Websites. Allerdings werden nicht alle von ihnen aus Leistungsgründen durchsucht.

Zugriffsmöglichkeiten mit StopSlovy suchen:

- sie überhaupt nicht indizieren und keine Fässer für sie machen (kleine Suchmaschinen tun dies)
- Indizierung der Seiten, aber Speicherung nur der relevanten Seiten in Fässern (dies wird z. B. von Google und Seznam praktiziert)
- Indizieren Sie sie als reguläre Wörter und erstellen Sie komplette Fässer (diese Lösung hat das Problem, dass Fässer leicht die Größe einer normalen Festplatte überschreiten können und daher nicht gespeichert werden können und das Lesen Sekunden dauern kann - diese Lösung wird daher in der Praxis nicht oder nur für kleine und spezialisierte Suchmaschinen verwendet)

Im Allgemeinen gibt es keinen universell richtigen Ansatz, und selbst Google wendet ihn nicht perfekt an. Dieses Problem ist so groß, dass es ein Leben lang dauern könnte, es zu lösen. Für die Zwecke des vorliegenden Papiers ist diese allgemeine Beschreibung jedoch ausreichend.

Crawler (Roboter, auch Spinne)
---------------------------

Ein Crawler ist ein Programm, das das Internet systematisch durchforstet und festhält, welche Wörter auf welchen Seiten erscheinen, und das auch Zusatzinformationen (auch Metainformationen genannt) sammelt. Ein Crawler läuft in der Regel auf vielen Servern, da das Crawlen des Internets eine hohe Netzwerkbandbreite erfordert und daher viele Seiten gleichzeitig gecrawlt werden müssen. Google schätzt, dass bis zu 30.000 Seiten gleichzeitig gecrawlt werden.

Bevor Crawler eine Seite aufruft, schaut er in eine Datenbank mit den Adressen, die er in Zukunft ansteuern will. Wenn er die Adresse bereits besucht hat, aktualisiert er nur seine Aufzeichnungen (er besucht die großen Websites alle paar Stunden, die Nachrichtenseiten alle paar Minuten und die normalen Websites höchstens einmal im Monat).

Gelangt der Roboter an eine neue Adresse, die er nicht kennt, schaut er sich die darin enthaltenen Links zu anderen Dokumenten (Seiten) an. Für jeden Link wird seine Wichtigkeit berechnet, und wenn es sich um einen wichtigen Link handelt, wird er auch später auf der Seite angezeigt.

List zum Beispiel durchsucht auf diese Weise nur einige Link-Ebenen auf jeder Website, während Google versucht, die gesamte Website zu durchsuchen, einschließlich unwichtiger Dokumente, was es zur umfassendsten Suchmaschine im Internet macht.
Der Roboter versucht auch, die Seite zu bewerten, während er sie durchsucht, und berechnet ihren "Rang", der eine numerische Darstellung der Bedeutung der Seite im Internet ist. In der Vergangenheit hat Google den PageRank verwendet, der einen Bereich von 0 bis 10 umfasst. Google berechnet den PageRank 10 nur für sich selbst und für Websites wie microsoft.com oder w3c.org. Heutzutage wird der Wert anders berechnet (künstliche Intelligenz und Vektormathematik).

Der höchste PageRank in der Tschechischen Republik ist Seznam.cz mit einem Wert von 7. Der Rang hat einen großen Einfluss darauf, wie oft ein Roboter die Seite besuchen wird und wie weit oben sie in den Suchergebnissen erscheint. Ränge werden nur aus Backlinks berechnet, die auf die gewünschte Seite verweisen.

Der Crawler macht nichts weiter mit den Daten, er lädt sie nur aus dem Internet herunter und speichert sie in einer Datenbank, wo sie auf den Indizierungsprozess warten.
