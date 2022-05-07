Abrufen von Parametern aus der URL mit der Methode GET
======================================================

> id: bbf2cb2c-f7f7-4be9-a9cd-960014db0f51
> slug:
> 	cs: metoda-get
> 	de: abrufen-von-parametern-aus-der-url-mit-der-methode-get
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: caebe20b6b7cdf0b3703ff55dad6f094

Sie haben eine Seite geöffnet, folgen der URL und sehen ein Fragezeichen mit einigen Parametern. Ein unerfahrener Programmierer würde denken, dass es sich um separate Dateien handelt, aber siehe da. Versuchen Sie, eine Datei zu erstellen, die ein Fragezeichen im Namen hat (das funktioniert nicht). **Das ist der Grund, warum dieser Artikel geschrieben wurde**.

Was ist das?
--------------------------

Eigentlich geht es darum, dass es sich um eine einzelne Datei handelt, der man Variablen über eine URL übergibt. Ich habe also beispielsweise eine Datei **index.php**, der ich den Namen des Artikels übergebe: **index.php?clanek=o-php**.

Code + Erklärung
--------------------------

Superglobale Variable `$_GET` enthält Schlüssel mit Parametern aus der URL

```php
echo $_GET['Artikel'] ?? '';
```

Sicherheit und Längenbegrenzung
--------------------------

Die GET-Methode ist nicht sicher, so dass vertrauliche Daten nicht über sie gesendet werden sollten. Einer der Hauptgründe dafür ist, dass es sich um eine unverschlüsselte Kommunikation handelt, und zweitens werden sie in der Historie gespeichert.

Vertrauliche Daten oder einfach alles sollte mit der Methode <a href="/method-post">POST</a> gesendet werden. GET ist eher für Seiten geeignet, bei denen es sinnvoll ist, Parameter anzugeben (z. B. Suchmaschinen, Artikelseiten), damit die Seite verlinkt werden kann.

Die Dauer des GET ist nicht unbegrenzt! Viele Anfänger zahlen dafür. Die maximale Länge beträgt etwa 1024 Zeichen (mancherorts sind es 1088). Senden Sie also für längere Texte <a href="/method-post">POST</a> mit.
