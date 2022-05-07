cURL in PHP - Herunterladen von Daten über eine URL
===================================================

> id: '0bd1aed6-460d-4b63-9afe-f5087d1c6046'
> slug:
> 	cs: curl
> 	de: curl-in-php---herunterladen-von-daten-ueber-eine-url
> 
> perex:
> 	- Knihovna cURL je robustní PHP knihovna pro pokročilou HTTP komunikaci.
> 	- Die cURL-Bibliothek ist eine robuste PHP-Bibliothek für erweiterte HTTP-Kommunikation.
> 
> publicationDate: '2020-02-15 22:14:32'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '7c565449293cf1ce4950f309abaf04d0'

Die PHP-Bibliothek `cURL` ist eine gute Möglichkeit, Daten von einem fremden Server herunterzuladen.

Auf der Grundlage einer Abfrage erstellt es eine HTTP-Anfrage, die es an den Zielserver sendet, und nach dem Herunterladen enthält es eine API für die (relativ) einfache Datenverarbeitung.

Im Gegensatz zur nativen Funktion `file_get_contents` (über die wir auch HTTP-Anfragen stellen können), bietet sie viel bessere Konfigurationsmöglichkeiten und lädt Seiten/Dateien wie ein echter Browser herunter.

> Die Funktion "file_get_contents" verwendet intern die "cURL"-Bibliothek, sie hat nur nicht so detaillierte Konfigurationsoptionen.

Erkennung des cURL-Modus in einer Anfrage
----------------------------

Es ist oft nützlich zu erkennen, ob die aktuelle Anfrage über `cUrl` oder klassisch im Browser gestellt wurde.

Es gibt keine direkte Implementierung für diese Funktion in PHP, aber wir können selbst eine einfache Funktion schreiben:

```php
function isCurl(): bool
{
    return str_contains($_SERVER['HTTP_USER_AGENT'] ?? '', 'locken.');
}
```

Wenn Sie Linux und das dazugehörige Terminal verwenden oder auf einem Mac arbeiten, versuchen Sie diesen Befehl:

```shell
curl https://php.baraja.cz/curl
```

Der Befehl stellt eine interne Anfrage an diese Website und gibt das Ergebnis zurück.

Wenn die Anwendung keine cURL-Anfrage erkannt hat, wird der HTML-Code so zurückgegeben, als ob die Anfrage vom Browser gekommen wäre. Da die Anfragetypen jedoch erkannt werden, steht der Rückgabe eines bereinigten Markdown-Artikels nichts im Wege.

Dies hat den Vorteil, dass die Daten viel besser bereinigt werden. Wir zeigen dem Benutzer im Browser das formatierte HTML, dem Roboter aber nur den Basisinhalt.

Detaillierte Verwendung der API in PHP
--------------------------

Wenn Sie sich für die detaillierte Verwendung von cUrl interessieren, empfehle ich Ihnen die <a href="https://www.php.net/manual/en/book.curl.php">offizielle Dokumentation</a>, die immer auf dem neuesten Stand ist.

Für den gelegentlichen Gebrauch gibt es eine <a href="https://docs.guzzlephp.org/en/stable/">**Guzzle**</a>-Bibliothek, die mit
