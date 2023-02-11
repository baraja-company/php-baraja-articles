Wie wählt man Technologien aus? Wann wechseln wir zu JavaScript?
================================================================

> id: d3ea7ea7-3e2f-455d-a424-cce374bcd1d2
> slug:
> 	cs: senior-9-vyber-technologii
> 	de: wie-waehlt-man-technologien-aus-wann-wechseln-wir-zu-javascript
> 
> publicationDate: '2023-02-11 14:40:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '72807451867478e1a7b6d67f4e5c31b3'

Die Auswahl der richtigen Technologien ist eine Grundvoraussetzung, um ein erfahrener Entwickler zu werden. Diese Entscheidungen sind oft nicht einfach, denn Sie müssen den aktuellen technischen Stand der Anwendung berücksichtigen, den Entwicklungsstand, den Wissensstand Ihres Teams, die auf dem Arbeitsmarkt üblichen Kenntnisse, die Kosten der einzelnen Technologien, die Risiken für Ihren Betrieb, die Sicherheit und Stabilität der Technologie und nicht zuletzt die Frage, woran die Entwickler in fünf Jahren interessiert sein werden, wenn 80 % Ihres derzeitigen Teams ersetzt werden.

Ich habe in 6 großen Unternehmen gearbeitet, die in PHP entwickeln. Nur 2 von ihnen versuchen, langfristig auf eine andere Technologie umzusteigen, die anderen bleiben dabei. Das birgt eine Menge Probleme. Ich versuche zum Beispiel gerade, einen leitenden PHP-Entwickler für ein Unternehmensprojekt zu finden, das ich für O2 entwickle und bei dem ich in ein Büro in Prag pendeln muss, und ich kann sehen, wie sich der Markt für PHP-Entwickler in den letzten fünf Jahren gelichtet hat. PHP ist einfach nicht mehr cool und nicht viele Leute wollen es machen. Es sind nicht genügend Junioren dabei.

Aus Gesprächen mit jüngeren Leuten weiß ich, dass React und allgemein "schlanke" Technologien heutzutage sehr beliebt sind. Aus Sicht der Anwendungsarchitektur ist es sinnvoll, wenn man diese Richtung frühzeitig entdeckt und Zeit hat, sich anzupassen. Anstelle der komplexen Erstellung von Web-Layouts und Formularen in Latte, für die man einen praktisch mittelmäßigen Entwickler für eine schon etwas komplexere Aufgabe braucht, braucht man in React nur einen Junior, der im Grunde vor einem Monat angefangen hat und in der zukünftigen Lösung noch nicht allzu viele Fehler macht.

Mit React können Sie einen großen Teil des Backends wegwerfen, der nur geschrieben wurde, damit das Frontend überhaupt existieren kann. Kurz gesagt, es macht die Entwicklung billiger, und als Bonus erhalten Sie eine schnellere Bereitstellung neuer Funktionen, weil die Entwickler sich nicht immer wieder mit den komplexen Problemen befassen müssen, die sich aus der Designsprache PHP ergeben.

Die meisten Webanwendungen benötigen gar kein oder nur ein minimales Backend mehr. Wenn man API-Endpunkte in Node.js (ebenfalls eine Technologie, die auf Javascript aufbaut) bereitstellt, kann ein Entwickler, der bisher nur mit React gearbeitet hat, plötzlich auch Teile des Backends schreiben, da es sich um dieselbe Sprache handelt.

Bei einer genaueren Analyse der Projekte, die ich in den letzten 5 Jahren entwickelt habe, gibt es nur wenige Dinge, die in Node.js fehlen, die mich immer noch dazu bringen, PHP für einige Operationen zu verwenden.

Nämlich:

- Doctrine (und allgemeiner relationaler Datenbankzugang auf der Grundlage von Objektentitäten)
- Versenden und Verwalten von E-Mails
- SOAP API (leider gibt es das manchmal noch)
- Sitzungen (Sie müssen sie z. B. durch ein JWT-Token ersetzen)
- Komplexe Legacy-Logik, die in PHP geschrieben wurde und die ich nicht ohne Weiteres refaktorisieren kann
- Schnelle Verarbeitung komplexer Datenstrukturen, bei denen Daten geändert werden müssen
- Bestehende Mitarbeiter im Team, die Sie umschulen müssen, um etwas Neues zu tun

Doch dann kam Node.js auf, das den Rest der Dinge besser macht. Zum Beispiel:

- Die Möglichkeit, die Anwendung direkt in die Cloud zu verlagern
- Viel (vielleicht sogar doppelt) billigere Entwicklung der gleichen Funktionalität
- Gleiche Logik bei BE und FE, ohne den Code doppelt schreiben zu müssen
- REST-API-Endpunkte
- Parallele Aufrufe von mehreren Codes auf einmal
- Möglichkeit, eine HTTP-Antwort zu senden, aber der Code wird weiter ausgeführt
- Kumpel
- Bibliotheken sollen mit Cloud-Diensten arbeiten
- Deutlich bessere Reaktionszeit, da keine große Anwendung gebootet werden muss
- Voll funktionsfähiges Paradigma (Sie werden DIs los, die Sie z. B. in JS nicht brauchen)
- Arbeiten mit Formularen und Daten
- Einfache Updates und eine aktive Entwicklergemeinschaft

**Kommentar zu Doctrine:** Ich weiß, dass JS eine Menge Bibliotheken für die Arbeit mit Datenbanken bringt. Oder sogar neue Paradigmen wie Mongo. Mir gefällt die Richtung, in die sich die Datenverarbeitung entwickelt. Andererseits glaube ich, dass tabellarische relationale Datenbanken niemals verschwinden werden. Wenn Sie ein wirklich großes Projekt durchführen, bei dem Sie mehr als zehn Millionen Datensätze verwalten, brauchen Sie einfach eine traditionelle Technologie, mit der Sie sehr vertraut sind und wissen, was Sie erwartet. Die Vorstellung, dass ich eine Spalte (Eigenschaft) hinzufügen möchte und dafür alle Entitäten mit einem Migrationsskript neu zuordnen müsste, ist mir zum Beispiel ziemlich unheimlich.
