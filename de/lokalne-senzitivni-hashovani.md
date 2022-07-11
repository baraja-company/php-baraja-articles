Lokal sensitives Hashing
========================

> id: '76e09294-08ea-4dde-a431-2116595d9f04'
> slug:
> 	cs: lokalne-senzitivni-hashovani
> 	de: lokal-sensitives-hashing
> 
> publicationDate: '2022-07-11 10:45:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: c23def9586aa05d73c4f1cf1fd1dda43

Das Prinzip der meisten Hash-Funktionen für das Fingerprinting von Dokumenten besteht darin, dass sie für jede einzelne Eingabe immer die gleiche Ausgabe liefern. Dies wird als deterministisches Verhalten bezeichnet. Gleichzeitig führt eine kleine Änderung der Eingabe zu einer großen Änderung der Ausgabe (und damit zu einem völlig anderen Hashwert). Dies ist besonders nützlich, wenn wir überprüfen wollen, ob sich das Eingabedokument geändert hat, und wenn ja, erhalten wir etwas völlig anderes. Ein Beispiel für eine solche Funktion ist MD5 oder SHA1.

Wenn wir aus dem Hash auf den Inhalt der ursprünglichen Eingabe schließen wollen, gibt es keine einfache Möglichkeit, dies zu tun, und wir haben keine andere Wahl, als jede Option mit roher Gewalt zu erzwingen, bis wir einen Treffer erhalten. Der Grund dafür ist, dass wir aufgrund der großen Veränderung der Ausgabe nicht durch Iterationen feststellen können, ob wir uns dem Ziel nähern oder nicht.

Einige Hash-Funktionen wie BCrypt geben für dieselbe Eingabe jedes Mal eine andere Ausgabe zurück, um sie für das Hashing von Passwörtern robust zu machen. In der Tat enthält die Ausgabe direkt Salz, was einen so genannten Wörterbuchangriff unmöglich macht. Diese Funktion ist nur für das Hashing von Kennwörtern nützlich, aber für die Überprüfung von Dokumenten ist sie völlig ungeeignet.

Prüfung der Ähnlichkeit von Dokumenten und Suche nach Duplikaten von Inhalten
-----------------------------------------------------------

Stellen Sie sich vor, wir lösen einen Algorithmus einer Web-Suchmaschine, die prüfen will, wie stark sich eine Webseite verändert hat. Oder es soll schnell überprüft werden, ob der Text auf einer Seite dem Text auf einer anderen Seite ähnlich ist oder sogar vollständig dupliziert wurde.

Eine Möglichkeit, dies zu überprüfen, besteht darin, die einzelnen Seiten miteinander zu vergleichen, was sehr ressourcenintensiv ist. Eine andere Möglichkeit ist die Berechnung eines SHA1-Hashes, aber damit wird der Inhalt des gesamten Dokuments gehasht, und wenn sich die Seite nur um ein Zeichen ändert, erhalten wir einen anderen Hash - daher ist er für diese Zwecke nicht geeignet.

Daher besteht eine mögliche Lösung darin, den Hash aus verschiedenen Bereichen des Dokuments unterschiedlich zu berechnen und dann nach Änderungen in der Ausgabe der Hash-Funktion zu suchen.

Das Prinzip des ortsabhängigen Hashings
----------------------------------

Wir haben den HTML-Code einer Webseite heruntergeladen, für die wir einen Hashwert berechnen wollen. Wir unterteilen das Dokument in verschiedene Regionen (Zellen) durch einen Algorithmus, der korrekt konfiguriert werden muss.

Wir hacken jede Region unabhängig voneinander mit einem gemeinsamen Algorithmus und verketten das Ergebnis zu einer einzigen Zeichenfolge.

Die Ausgabe kann dann z. B. "3791744029724361934" lauten (dieser Inhaltshash wurde vom Ahrefs-Tool bereitgestellt).

Wenn wir zum Beispiel wissen, dass der Inhalt der Kopfzeile die ersten 5 Zeichen und die Fußzeile die letzten 5 Zeichen darstellt, wissen wir, dass die Mitte der Zeichenkette den Inhalt der Seite darstellt. Wenn sich der Inhalt der Fußzeile auf allen Seiten ändert (z. B. wenn der Webmaster einen neuen Link hinzufügt, sich das Aktualisierungsdatum ändert usw.), dann ändern sich beim Herunterladen der neuen Version der Seite nur einige Zeichen in der Raute im rechten Bereich geringfügig, und wir wissen, dass sich nur die Fußzeile geändert hat, der Inhalt aber unverändert bleibt. So können wir eine solche Änderung ignorieren und müssen nicht die gesamte Website durchgehen.

Wie optimiert Google das Web-Crawling?
----------------------------------------

Internet-Bots müssen sparen, wo sie können. Das Crawlen von Webseiten ist sehr teuer und Aktualisierungen kosten Rechenzeit.

Bei der Beobachtung des Verhaltens des Google-Robot-Algorithmus habe ich zum Beispiel festgestellt, dass er nur auf große inhaltliche Veränderungen reagiert. Wenn sich die Seite nur wenig ändert, wird sie auf normale Weise gecrawlt. Wenn sich jedoch beispielsweise die Fußzeile und die Kopfzeile einer Website erheblich ändern, wird dies als Neugestaltung gewertet und der größte Teil der Website wird auf einmal bearbeitet, um das neue Aussehen so schnell wie möglich zu erhalten.

Auf ähnliche Weise werden auch standortübergreifende Duplikate erkannt. Wenn eine Gruppe von Seiten sehr ähnlich ist oder denselben Inhalt hat, erhalten sie einen sehr ähnlichen Hash, den der Roboter verwenden kann, um schnell zu überprüfen, ob die Dokumente ähnlich sind, und kann das kanonische Dokument auswählen und die anderen ignorieren.

Implementierung der Hashing-Funktion
-----------------------------

Es gibt keine fertige Implementierung der ortsabhängigen Hashing-Funktion in PHP. Gleichzeitig ist mir kein frei verfügbares Paket bekannt, das diese Funktion gut implementiert.
