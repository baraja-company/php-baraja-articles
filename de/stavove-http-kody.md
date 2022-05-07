HTTP-Statuscodes
================

> id: '97800bb6-004c-4bdb-b126-f87e77cc5120'
> slug:
> 	cs: stavove-http-kody
> 	de: http-statuscodes
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '6c1da4a7233c1eba531e9336daae5acb'

Bei der HTTP-Kommunikation werden so genannte "Statuscodes" übertragen, die Auskunft darüber geben, wie die Übertragung erfolgt ist. Sie wissen sicher, dass ein "200"-Code Erfolg bedeutet und ein "404"-Code eine nicht existierende Seite.

Die Statuscodes sind je nach Präfix in mehrere Gruppen unterteilt.

1xx Informativ
--------------

| Code | Bedeutung |
|-------|--------|
| "100" | Weiter |
| "101" | Vermittlungsprotokoll |

2xx Erfolg
----------

| Code | Bedeutung |
|-------|--------|
| "200" | OK (alles OK) |
| "201" | Erstellt |
| `202` | Akzeptiert |
| `203` | Unerlaubte Informationen |
| `204` | Kein Inhalt |
| `205` | Inhalt wiederherstellen |
| `206` | Teilinhalt |

3xx Umleitung
----------------

| Code | Bedeutung |
|-------|--------|
| "300" | Multiple Choice |
| "301" | Permanente Umleitung |
| "302" | Gefunden |
| `303` | Mehr sehen |
| `304` | Keine Änderung |
| `305` | Proxy verwenden |
| "306" | **Alt, aber für zukünftige Verwendung reserviert** |
| "307" | Vorübergehende Umleitung |

4xx Client (Benutzer) Fehler
-----------------------------

| Code | Bedeutung |
|-------|--------|
| `400` | Schlechte Anfrage |
| `401` | Unautorisierte Verbindung |
| "402" | Zahlung angefordert |
| "403" | Deaktiviert |
| "404" | Nicht gefunden |
| "405" | Methode nicht erlaubt |
| "406" | Inakzeptabel |
| "407" | Proxy-Authentifizierung erforderlich |
| "408" | Zeitüberschreitung der Anfrage |
| "409" | Netzwerkkonflikt |
| `410` | Daten weg |
| `411` | Angeforderte Länge stimmt nicht überein |
| `412` | Annahme gescheitert |
| `413` | Entity-Anfrage ist zu groß |
| `414` | Request-URI ist zu lang |
| "415" | Nicht unterstützter Medientyp |
| `416` | Angeforderter Umfang nicht zufriedenstellend |
| "417" | Erwartung fehlgeschlagen |

5xx Server-Fehler
--------------

| Code | Bedeutung |
|-------|--------|
| "500" | Interner Serverfehler |
| "501" | Nicht implementiert |
| `502` | Schlechtes Gateway |
| `503` | Dienst nicht verfügbar |
| `504` | Gateway-Zeitüberschreitung abgelaufen |
| `505` | HTTP-Version nicht unterstützt |
| "509" | Bandbreitenlimit überschritten |
