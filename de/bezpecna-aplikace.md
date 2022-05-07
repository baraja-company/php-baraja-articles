Sichere Anwendung
=================

> id: cc4667a3-aea3-4e0a-b323-da31ab78e019
> slug:
> 	cs: bezpecna-aplikace
> 	de: sichere-anwendung
> 
> publicationDate: '2019-10-01 14:19:04'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '74e52382b995b303550d8e4b783bbb84'

Wenn Sie es mit der Entwicklung von Webanwendungen ernst meinen und die Site später im Internet verfügbar sein soll, ist es sehr wichtig, sich mit der Sicherheit zu befassen.

Realistischerweise drohen den Entwicklern die folgenden Gefahren:

- **Die Anwendung hat einen internen Fehler**, z. B. weil dem Programmierer beim Schreiben des Codes ein Fehler unterlaufen ist, den er nicht bemerkt hat, oder weil der Fehler nur "manchmal" auftritt
- **Der Server hat eine Fehlkonfiguration** oder die Umgebung **hat sich geändert**, weil der Serveradministrator das Serververhalten geändert hat und die Sites nicht daran angepasst wurden. Oder wir stellen auf einem neuen Rechner ein und kennen die genaue Konfiguration nicht.
- Jemand versucht einen Angriff**, entweder ein externer Angreifer oder ein ehemaliger Mitarbeiter innerhalb des Systems.

Alle Situationen sind äußerst unangenehm und erfordern sofortiges Handeln. Es ist technisch nicht möglich, eine Anwendung so zu gestalten, dass sie nie ausfällt.

Während der Entwicklung ist es sehr wichtig, den Code zu testen, sobald er geschrieben ist, und wenn ein Fehler auf dem Server auftritt, ist es wichtig, den Entwickler sofort mit einer genauen Beschreibung des Problems zu informieren.

Um Fehler zu erkennen und den Zustand des Servers zu überwachen, empfehle ich das Tool <a href="https://tracy.nette.org/">Tracy</a>, das alle schwerwiegenden Fehler, unbehandelten Ausnahmen und mehr in Dateien direkt auf der Festplatte des Servers protokolliert. Außerdem können Sie eine E-Mail einrichten, an die Benachrichtigungen geschickt werden.

Sind Sie an einer Sicherheitsberatung für Ihre Anwendung interessiert?

Kontaktieren Sie mich.
