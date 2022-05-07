Rückgabe von UUID zu Ganzzahl
=============================

> id: d842517b-c4f6-4cef-a839-d08ff3804fca
> slug:
> 	cs: navrat-z-uuid-na-integer
> 	de: rueckgabe-von-uuid-zu-ganzzahl
> 
> publicationDate: '2021-05-02 17:00:00'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: b7c136f3e3ca8a4563230c557ab1fa9c

In der Softwareentwicklung gerät ein Programmierer oft in eine Sackgasse, wenn er vor einer Architekturentscheidung steht, die sich auf Jahrzehnte hinaus auf die Zukunft seiner Arbeit auswirken wird. Gleichzeitig ist es eine Entscheidung, die nicht mehr rückgängig gemacht werden kann, und jeder Fehler hat seinen Preis. Datenbanken sind ein typisches Beispiel für eine architektonische Entscheidung, bei der bei jedem kleinen Fehler Köpfe rollen.

Eine der großen Entscheidungen der letzten Zeit war die Frage, wie Primärschlüssel in Datenbanktabellen gespeichert werden sollen. Dies scheint zwar ein triviales Problem zu sein, aber es steckt viel mehr dahinter, als Sie vielleicht denken.

Optionen für Primärschlüssel
-------------------------

Grundsätzlich gibt es vier Möglichkeiten:

- ganzzahlig
- Ganzzahl ohne Vorzeichen
- große Intention
- <a href="/uuid-performance">UUUID</a>

Integer ist einfach eine ganze Zahl (im Falle von `unsigned` dann unsigned, also immer positiv, und im Falle von `big int` dann kann sie extrem große Werte erreichen). Ein sehr einfaches Konzept. Eine UUID ist dann eine Textzeichenfolge (z. B. in der Form "c4a760a8-dbcf-5254-a0d9-6a4474bd1b62"), die aus mehreren Teilen besteht, von denen jeder bestimmte Eigenschaften haben kann und für den Aufbau großer Multiserver- oder dezentraler Anwendungen nützlich ist. Es gibt ein großes Ökosystem nützlicher Technologien rund um UUIDs, die Probleme lösen, von denen Sie vielleicht nicht einmal wissen, dass Sie sie haben oder in Zukunft haben werden.

Verwenden Sie den richtigen Hammer
-------------------------

Vor nicht allzu langer Zeit (Winter 2020) erläuterte mein Freund Paul das Konzept der Anwendung der geeigneten Lösung für ein Problem einer bestimmten Größe. Dies ist ein großartiger und wichtiger Gedanke, den viele Entwickler gerne vergessen - er führt zu enorm komplexen Lösungen, wenn dies nicht nötig ist. Im Englischen gibt es einen schönen Ausdruck für dieses **Over-engineering**.

UUID-Größe und Einzigartigkeit
--------------------------

Der grundsätzliche Vorteil der UUID besteht darin, dass wir, wenn die Anwendung zu groß wird und wir die Datenbank auf viele Webserver aufteilen, wo eine Datenbanktabelle so groß ist, dass sie nicht auf die Festplatte eines Rechners passt, sie auf viele physische Rechner aufteilen können, von denen jeder seinen eigenen Teil der Tabelle kennt und den Rest bei seinen Kollegen abfragt. UUID löst auch das grundsätzliche Problem des Einfügens neuer Zeilen, bei dem wir im Falle extrem großer Anwendungen an vielen Stellen parallel Zeilen schreiben müssen und nicht warten wollen, bis die Schreibkapazität des Hauptservers frei wird.

Das Konzept, an vielen Stellen gleichzeitig zu schreiben, wird z. B. von Chat-Anwendungen genutzt. Wenn du eine Nachricht über den Messenger sendest, wird sie an den nächstgelegenen Facebook-Datenbankserver gesendet, der der Nachricht eine UUID und einen Zeitstempel zuweist und sie in seine lokale Datenbank schreibt. Ihr Freund auf der anderen Seite der Welt wiederum schreibt die Nachrichten in sein lokales Rechenzentrum, und in der Zwischenzeit sorgt die gesamte Cloud-Infrastruktur für eine weltweite Synchronisierung. Klingt cool, oder? :)

Damit ein solches paralleles Schreiben funktioniert, muss das Problem der Datensatzkollision gelöst werden. Wenn die einzelnen lokalen Datenbanken eine einfache Ganzzahl verwenden, werden zwei unabhängige Server sehr bald zwei verschiedene Datensätze unter demselben Bezeichner schreiben. Wenn diese Datensätze synchronisiert werden, kommt es zu einer Kollision. Normalerweise gibt es keine Lösung - Sie können die IDs nicht neu nummerieren, weil andere Sitzungen dazu führen können.

Die UUID löst dieses Problem, indem sie z. B. jedem Server ein vereinbartes Präfix gibt, das am Anfang jeder UUID eingefügt wird, dann einen Zeitstempel und schließlich den eigentlichen Bezeichner.

> **Interessante Tatsache:** Beim Schreiben einer so großen Datenmenge sind wir nicht so sehr daran interessiert, wann welcher Datensatz geschrieben wurde, sondern in welcher Reihenfolge er geschrieben wurde (damit wir z. B. die Reihenfolge der Nachrichten für den Benutzer nicht vertauschen).

Sie könnten sich fragen, wie man mit einer Situation umgeht, in der sich die Server nicht darauf einigen können, welches Präfix zu verwenden ist. Dieses Problem tritt zum Beispiel bei dezentralen oder Offline-Anwendungen auf. In diesem Fall kann die UUID sogar zufällig generiert werden.

Die Frage ist nun, wie hoch die Wahrscheinlichkeit ist, dass bei der <a href="https://stackoverflow.com/questions/1155008/how-unique-is-uuid">Zufallsgenerierung einer UUID</a> ein Konflikt auftritt. Nun, das wird Ihnen wahrscheinlich nicht passieren. Es gibt ungefähr `2^122` eindeutige UUIDs (weil es sich um eine `128-Bit-Zahl` handelt). In der Praxis liegt die Wahrscheinlichkeit eines Konflikts bei etwa "0,00000000006 (6 × 10-11)". In der Praxis bedeutet dies, dass, wenn wir in den nächsten 100 Jahren jede Sekunde **1 Milliarde UUIDs** erzeugen, die Wahrscheinlichkeit eines Konflikts 50 % beträgt. Es ist also wahrscheinlicher, dass kein Konflikt auftritt, und die UUID ist die endgültige Lösung für Ihre Datenbankprobleme.

Gibt es einen Bedarf für eine solche robuste Lösung?
-------------------------------

Wenn Sie es nicht wissen, lautet die Antwort **nein**.

Wenn der Primärschlüssel auf "int" mit dem Kennzeichen "ohne Vorzeichen" eingestellt ist, gibt es "4.294.967.295" mögliche Werte (4 Milliarden). Einen Vergleich der Integer-Größen finden Sie in der <a href="https://dev.mysql.com/doc/refman/8.0/en/integer-types.html">MySql-Dokumentation</a>.

Wenn Sie 4 Milliarden Datensätze in einer Tabelle speichern, geht Ihnen der Speicherplatz wahrscheinlich schon früher aus.

Leistung von Ganzzahlen und Verknüpfungen
----------------------

Ganzzahlen sind wirklich sehr schnell. Es gibt native Optimierungen für sie in MySql. Indizes funktionieren einwandfrei (außerdem sind sie viel kleiner), sie benötigen nur 4 Bytes, sind sehr schnell zu verbinden und eignen sich für die meisten Fälle.

Wenn Sie ein Problem mit der Datenbankreplikation haben, könnte die beste Lösung darin bestehen, die gesamte Datenbank in die Cloud wie MS Azure zu verlagern und sie extern abzufragen. Selbst bei der Speicherung von Dutzenden Millionen von Datensätzen liegt die Zugriffsgeschwindigkeit via Integer auf eine bestimmte Zeile in der Größenordnung von Millisekunden (unter 3 ms auf einem gut konfigurierten Server), und mit einem geclusterten Index kann die Zeit auch bei einer großen Anzahl von Anfragen gut überschaubar sein.

Wenn Sie wirklich UUIDs verwenden müssen, sollten Sie die MySql-Welt verlassen und die Postgres-Datenbank verwenden, die im Gegensatz zu MySql einen eigenen Datentyp für <a href="https://www.postgresql.org/docs/9.1/datatype-uuid.html">UUUIDs</a> hat. Die Arbeit mit Joins ist ein großes Problem mit UUIDs und MySql, und wenn man nur 3 Tabellen verbindet (wobei jede nur einige zehntausend Datensätze hat), kann die gesamte Abfrage mehrere hundert Millisekunden bis zu wenigen Sekunden dauern. Und dies ist leider ein MySql-Problem, das Sie wahrscheinlich nicht lösen können.
