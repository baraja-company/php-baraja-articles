Internet-Suchmaschinen-Algorithmus - Sortierung und Deskriptor
==============================================================

> id: ca4aaac5-0cd8-41db-b860-c32661c43d82
> slug:
> 	cs: vyhledavac-trideni-a-popisovac
> 	de: internet-suchmaschinen-algorithmus---sortierung-und-deskriptor
> 
> publicationDate: '2016-09-11 13:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '541310dfdc96b75a320c7cad1b03e734'

In dieser Lektion über die Prinzipien einer Internet-Suchmaschine werden wir verstehen, wie eine Suchmaschine die Ergebnisse sortiert, beschreibt und auswertet.

Sortierung der Ergebnisse
----------------

Stellen wir uns ein fertiges Fass vor, das gerade auf dem Suchserver bereit liegt. Unsere erste Suchanfrage kommt vom Benutzer, und jetzt müssen wir die erste "grobe" Sortierung vornehmen, die dann weiter verfeinert wird.

Nehmen wir die folgende Beispiel-Eingabeabfrage:

```txt
[O (and) pejskovi (and) a (and) kočičce]
```

Ja, das ist die Form, in der der Suchserver die bearbeitete Anfrage des Benutzers erhält und nun auf die Rückgabe des Ergebnisses wartet. Insgesamt haben wir weniger als 300 ms Zeit, um dies zu tun, also schnell :)

Im ersten Schritt erhalten wir Fässer voller zukünftiger Ergebnis-IDs (auch Kandidaten genannt) und auszuführender Operationen. Dieser abgerufene Lauf ist in manchen Fällen nicht vollständig, sondern enthält nur die Werte, die der Server in einem bestimmten Zeitintervall (auch **Timeout** genannt) von der Festplatte lesen konnte. Wir erhalten zum Beispiel die folgenden Zahlenreihen (die teilweise vorsortiert sind):

| Fässer | Dokumente |
|-------|---------|
| o | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| Hund | 5,19,23,42,46,58 |
| a | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| Muschi | 9,19,42,57,58,62,68,83 |

Wir führen nun eine grobe Schnittmenge aller Mengen durch und erhalten eine Liste von Dokument-IDs, die in allen Fässern gleich sind. In der Praxis gehen wir die kürzeste Liste durch und suchen die anderen.

Die gemeinsamen IDs aller Fässer sind "19, 42, 58", so dass die künftige Ergebnisseite 3 Einträge in einer noch unbekannten Reihenfolge enthält. Wir betrachten die Dokumente nach wie vor als IDs, da dies eine große Menge an Daten spart. In den Suchergebnissen wollen wir dem Nutzer jedoch in der Regel nicht die Dokumentennummern auflisten, sondern den Dokumententitel (Überschrift), die Beschreibung, die URL und andere Informationen. Dies wird durch die Komponente "Deskriptor" gewährleistet.

Deskriptor
---------

Aus dem vorangegangenen Kapitel bleibt uns eine Liste von Kandidaten für künftige Ergebnisse. Wir haben sie in sortierter Form im Sinne des Systems abgerufen, d. h. so, wie die Suchmaschine sie im Index der Reihe nach geordnet hat. An diesem Punkt können wir die Big Data nicht mehr sequentiell lesen, sondern müssen eine Art relationale Datenbank sequentiell durchlaufen, um zusätzliche Informationen für eine andere Art der Sortierung zu finden, die für den Benutzer besser verständlich ist.

Ein Datenbank-Slice könnte zum Beispiel so aussehen:


| ID | Titel | Label | URL | Rang |
|----|---------|---------|-----|------|
| 19 | Josef Čapek: Eine Geschichte von einem Hund und einer Katze | Das war, als der Hund und die Katze noch zusammen bauten; sie hatten ihr Häuschen am Wald und lebten dort zusammen und wollten alles so machen, wie es große Menschen machen | www.troglodytarium.cz/webz/axf/.../Capek_J_Pejsek_a_kocicka.htm | 72 |
| "Also gut", sagte der Hund, und die Katze nahm Seife und einen Topf mit Wasser, kniete sich auf den Boden, nahm den Hund als Bürste und schrubbte den ganzen Boden mit dem Hund. Der Boden war ganz nass | www.capek.narod.ru/povidani.htm | 86 |
| Wie der Hund und die Katze einen Kuchen für den Feiertag gemacht haben Morgen war der Feiertag des Hundes und der Geburtstag der Katze. Die Kinder wussten das und wollten den Hund und die Katze zu ihrem Geburtstag überraschen. Sie überlegten, was sie für den Hund tun könnten und | a.da.mek.sweb.cz/capek.j/dort.htm | 34 |

Berechnung der Relevanz
-----------------

Der letzte Schritt ist die Berechnung der Relevanz der Antwort. Für die meisten Ergebnisse ist es ausreichend, die Relevanz als eine einzige Zahl mit einem festen Radius darzustellen, nach dem die Ergebnisse sortiert werden. Nach Rang werden die Ergebnisse wiederum "grob" in mehrere Gruppen sortiert (die Anzahl der Gruppen hängt von der Varianz der Ränge ab) und mit diesen Gruppen wird weiter gearbeitet.

Die Gruppen mit dem höchsten Rang werden als erstes genommen. Oft reicht das aus, um die erste Seite der Ergebnisse zu sortieren, und wir müssen uns um nichts anderes kümmern. Wenn jedoch mehrere verschiedene Dokumente den gleichen Rang haben, müssen wir eine detailliertere Analyse durchführen und zusätzliche Hilfsränge berechnen, um die Seite zu bewerten. Der zusätzliche Rang kann zum Beispiel die Qualität der Links, das Alter des Dokuments, die Länge des Titels, das "Look and Feel" der URL und viele andere Faktoren sein.

Rückgabe der Ergebnisse an den Benutzer
---------------------------

Hurra! Wir haben die Ergebnisse und müssen jetzt nur noch die Seite für den Benutzer darstellen. Das Ergebnispaket wird an den Server zurückgegeben, der die Ergebnisse in eine vorbereitete Vorlage einpasst, Werbebanner auf der Seite einfügt und die Seite an den Nutzer zurückschickt. Gleichzeitig kann es auch andere Hilfsoperationen durchführen, wie z. B. die Zwischenspeicherung der Ergebnisse. Wenn eine andere Person in naher Zukunft (z. B. innerhalb der letzten Stunde) nach derselben Suchanfrage sucht, werden die Ergebnisse nicht erneut durchsucht, sondern nur aus dem temporären Speicher gelesen - was oft zu einer Verbesserung der Gesamtleistung der Suchmaschine beiträgt.

Schlussfolgerung und Einblick in die Semantik
---------------------------

Dieser Beitrag sollte die grundlegenden Prinzipien beschreiben, wie Suchmaschinen intern funktionieren und wie sie es schaffen, eine theoretisch unbegrenzte Anzahl von Dokumenten in einer noch vertretbaren Zeitspanne abzurufen. Dabei handelt es sich um umfangreiche mathematische Optimierungen, die über viele Jahre hinweg von den besten Programmierteams entwickelt wurden.

Eine ausführliche Beschreibung der Einzelheiten würde den Rahmen dieses Papiers sprengen. Die ganze Sache wird auch dadurch kompliziert, dass die meisten anderen Schritte von keiner der Suchmaschinen im Detail beschrieben werden, weil sie deren Betriebsgeheimnis sind.

Es ist auch wichtig zu wissen, dass Suchmaschinen den Inhalt von Seiten und die Bedeutung von Ergebnissen immer noch nicht verstehen und dass es sich immer noch um eine statistische Berechnung handelt, die auf der rohen Rechenleistung und der Fähigkeit der Menschen beruht, auf qualitativ hochwertigen Text zu verlinken, und dass dieses System auch ziemlich anfällig ist (man denke an das Beispiel der "Google-Bombe", bei der ein hinterhältiger Webmaster einer Suchanfrage Inhalte aufzwingt, die irrelevant sind).

Dieses Problem dürfte teilweise durch künstliche Intelligenz gelöst werden, die alle Suchmaschinen nach und nach zu integrieren versuchen. Dies ist jedoch noch ferne Zukunftsmusik, denn es ist keine leichte Aufgabe und meist nur eine Frage der Verbesserung statistischer Berechnungsmethoden. Nehmen wir als Beispiel die Google-Graph-Suche, die versucht, die Anfrage direkt zu beantworten (obwohl sie in ihrer internen Struktur immer noch nur in einer vordefinierten Wissensbasis sucht).

Wenn Sie also das nächste Mal eine Anfrage an Ihre Lieblingssuchmaschine stellen, denken Sie daran, wie viel Arbeit Ihre Suchmaschine leisten musste und wie viele TB an Daten sie von der Festplatte lesen musste, um die 10 wichtigsten Dokumente für Ihre Suchanfrage zu finden.
