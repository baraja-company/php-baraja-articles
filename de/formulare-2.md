Formulare, Formularverarbeitung in PHP
======================================

> id: d1871a8d-bcfe-408d-ac12-6b827633f77e
> slug:
> 	cs: formulare-2
> 	de: formulare-formularverarbeitung-in-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '91f43ee06328e88a6a1058ecbf028222'

Ich nehme an, wir haben ein HTML-Formular erstellt, das wir abschicken, und nun wollen wir die Daten verarbeiten. Es gibt einen <a href="/formulare">separaten Artikel</a> über die Erstellung eines HTML-Formulars.

Empfang von Daten - verschiedene Wege
----------------------------

Die Art und Weise, wie das Formular gesendet wird, wird direkt in der HTML-Datei festgelegt

Es gibt 2 Möglichkeiten:

- **GET** - Es ist in der Adressleiste nach dem Fragezeichen sichtbar
 Zum Beispiel: `php.baraja.cz/search.php?query=formulare`
- **POST** - Versteckt (nicht sichtbar), die meisten Formulare werden per Post verschickt

Wir müssen sie dann mit der gleichen Methode in PHP lesen.

Abrufen der Daten vom Benutzer und Übermittlung an das Skript
------------------------------------------------------

Die Basis ist ein HTML-Formular, wie man es erstellt, können Sie <a href="/formulare">in einem separaten Artikel</a> nachlesen.

Nehmen wir zunächst ein einfaches Formular zur Eingabe des Namens des Benutzers an:

```html
<form action="welcome.php" method="GET">
   Zadejte jméno: <input type="text" name="username">
   <input type="submit" value="odeslat">
</form>
```

Es wird ein Textfeld angezeigt, in das Sie einen Namen eingeben und auf "Senden" klicken können. Wenn die Schaltfläche angeklickt wird, wird der Inhalt des Feldes an das Skript `welcome.php` gesendet.

Nun zur eigentlichen Verarbeitung in der Datei `welcome.php`:

```php
$username = $_GET['Nutzername'];

echo 'Der eingegebene Name lautet:' . $username;
```

Beachten Sie die spezielle Variable "$_GET". Dies ist eine **superglobale Variable**, die Daten aus dem Formular enthält und auf die als Array zugegriffen werden kann.

Das Problem bei dieser Lösung ist jedoch, dass die empfangenen Daten **nicht sicher** sind und eine ähnliche Form leicht angegriffen werden kann. So kann ein potenzieller Angreifer beispielsweise statt eines Namens Javascript-Code in das Feld eingeben, der auf die Seite geschrieben und ausgeführt wird.

Deshalb müssen wir immer alle Benutzerdaten bereinigen, bevor wir sie in HTML-Code ausgeben:

```php
$username = $_GET['Nutzername'] ?? 'Unbekannt';

echo 'Der eingegebene Name lautet:' . htmlspecialchars($username);
```

Weiterverarbeitung
----------------

Wir können alles mit den empfangenen Daten machen und sie wie eine gewöhnliche Variable behandeln.

Fügen Sie zum Beispiel den Wert in zwei Felder ein:

```php
echo $_GET['x'] + $_GET['y'];
```

Oder in einer Datei, Datenbank, E-Mail, ... speichern.

Die folgenden Funktionen sind dafür nützlich:

- <a href="/file-put-contents">file_put_contents</a> - Funktion zum Speichern von Daten in einer Datei
- <a href="/hashovani">MD5</a> - Prüfsummenberechnung, zum Beispiel für Passwörter
- <a href="/cookies">Cookies</a> - speichert Daten in **Cookies** (kleine Dateien im Webbrowser)
