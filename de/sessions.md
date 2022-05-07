Sessions - Server-Cookies in PHP
================================

> id: ab782ff0-d5ba-4115-9ebb-ab37978a6527
> slug:
> 	cs: sessions
> 	de: sessions---server-cookies-in-php
> 
> publicationDate: '2019-11-06 17:06:18'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '3c84a09e5d180f6a47a58bea70b7ab21'

Oft müssen wir mehr Informationen in <a href="/cookies">Cookies</a> speichern, aber die Höchstgrenze für Cookies liegt bei 4 kB, was nicht viel ist. Sessions löst dieses Problem, indem es die Daten auf dem Webserver speichert und nur eine kurze Kennung im Browser des Kunden hinterlegt, um festzustellen, welche Daten zu welchem Kunden gehören.

Starten einer Sitzung
---------------------

Bevor wir mit den Sitzungen arbeiten können, müssen wir sie erst einmal starten. Dies geschieht durch den Aufruf der Funktion `session_start()` gleich zu Beginn des Skripts:

```php
session_start();
```

> Starke Warnung: Vor dem Aufruf der Funktion `session_start()` darf keine Ausgabe in HTML-Code ausgeführt werden!

Sicherheit der Sitzung
-------------------

Der Inhalt der Sitzung wird auf dem Server gespeichert und nur die Kennung wird an den Client-Browser gesendet, so dass der Benutzer keine Möglichkeit hat, zu erfahren, was in der Sitzung gespeichert ist. Die einzige Möglichkeit, wie das Skript den Benutzer beeinflussen kann, ist das Löschen der Kennung (woraufhin das Skript eine neue Kennung erzeugt).

Abrufen von Daten aus einer Sitzung
----------------------

Alle Sitzungen werden in der superglobalen Variable `$_SESSION` gespeichert und können als Array durchlaufen werden.

Zum Beispiel kann der Name des aktuell angemeldeten Benutzers durch Schreiben abgefragt werden:

```php
echo $_SESSION['Benutzer'];
```

Hinweis: Es kann sein, dass die Sitzung nicht immer existiert (zum Beispiel, wenn Sie ein neuer Benutzer sind). Daher sollten wir vor jeder Auflistung immer prüfen, ob sie existiert und gegebenenfalls eine alternative Fehlermeldung anbieten.

```php
if (isset($_SESSION['Benutzer']) && $_SESSION['Benutzer']) {
    echo 'Angemeldeter Benutzer:' . $_SESSION['Benutzer'];
} else {
    echo 'Niemand hat sich angemeldet.';
}
```

Speichern von Daten in der Sitzung
----------------------

Das Speichern erfolgt als einfaches Speichern von Daten in einer Variablen:

```php
$_SESSION['Benutzer'] = 'Honzik';
```

Der Webserver kümmert sich um die technische Bereitstellung der korrekten Speicherung auf dem Server und die Übermittlung der Kennung an den Nutzer.

Löschen von Sitzungen
----------------

Einzelne Werte können je nach Schlüssel getrennt gelöscht werden:

```php
unset($_SESSION['Benutzer']);
```

Oder alternativ alle verfügbaren Sitzungen:

```php
unset($_SESSION);
```

> Hinweis: Beim Löschen einer bestimmten Sitzung wird der Schlüsselwert nicht geleert, sondern der Schlüssel wird vollständig gelöscht. Daher wird eine Fehlerwarnung ausgegeben, wenn versucht wird, einen nicht existierenden Schlüssel zu lesen. Das Vorhandensein eines Schlüssels kann mit der Funktion `isset()` jederzeit leicht überprüft werden.

Maximale Gültigkeit der Sitzung
---------------------------------

Für jede gespeicherte Sitzung gibt es ein Limit, wie lange sie auf dem Server gespeichert wird. PHP enthält direkt ein Cron-Skript, das in regelmäßigen Abständen alte Sitzungen löscht.

Der Standardwert ist normalerweise "1440 Sekunden", was "24 Minuten" entspricht.

Die Erhöhung des Wertes muss an 2 Stellen vorgenommen werden:

- In <a href="/info">`php.ini` wird die maximale Länge der Gültigkeit, die der Server beibehält, festgelegt</a>. Der Wert wird durch die Richtlinie `session.gc_maxlifetime` festgelegt,
- Auf der Seite des PHP-Skripts müssen Sie die aktuell angeforderte Gültigkeit angeben.

Verwendung in PHP:

```php
// der Server hält die Sitzung nun bis zu 3600 Sekunden = 1 Stunde lang aufrecht
ini_set('session.gc_maxlifetime', '3600');

// alle Clients (Browser) werden
// Sitzung mit einer Gültigkeit von genau 3600 Sekunden gesendet
session_set_cookie_params(3600);

session_start(); // Wir können die Sitzung beginnen!
```
