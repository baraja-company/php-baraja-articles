Merkmalskennzeichen / Merkmalsein- und -ausschalter
===================================================

> id: '4209c32f-655c-46fa-9090-e5ec3883ea61'
> slug:
> 	cs: feature-flagy
> 	de: merkmalskennzeichen-merkmalsein-und-ausschalter
> 
> publicationDate: '2022-12-11 15:00:00'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '1e32c312eab2ca16539cdb7431e2dda9'

Wenn Sie eine komplexere Anwendung entwickeln, werden Sie die Möglichkeit zu schätzen wissen, mehr Funktionen im Voraus zu entwickeln, sie mit der nächsten Version Ihrer Software zu verteilen und die Funktion später zu aktivieren.

Das ist genau das, wofür Merkmalskennzeichnungen geschaffen wurden. Dieser Artikel zeigt Ihnen, wie Sie sie verwenden können.

Grundlegende Umsetzung
---------------------

Feature-Flags sind im Grunde ein sehr einfaches Konzept, bei dem eine einzelne Funktion/Methode aufgerufen wird, die entscheidet, ob ein neues Feature aktiv ist.

Zum Beispiel:

```php
echo '<h1>Wetter-Apps</h1>';
echo 'Heute ist es so:' . getWeather();

if (feature('Karte')) {
   echo 'Karte:' . getMap();
}
```

Um die Verfügbarkeit einer bestimmten Nachricht zu prüfen, wird die Funktion `feature()` aufgerufen, die anhand des Aufrufnamens entscheidet, ob sie die betreffende Funktion zulässt oder ignoriert.

Umsetzung der Entscheidungslogik
-------------------------------

Die Entscheidungslogik ist oft komplex. So können Sie beispielsweise eine bestimmte Funktion nur ab einem bestimmten Datum oder für Benutzer einer bestimmten Gruppe ausführen. Ich teste zum Beispiel oft die Einführung einer neuen Funktion bei, sagen wir, 5 % der Benutzer auf diese Weise, damit sie nicht alle auf einmal betrifft.

Bei der Entwicklung von Unternehmenssoftware beispielsweise führen wir auf diese Weise Werbekampagnen und Rabatte durch, die ab einem bestimmten Datum gültig sind.

Wenn ein bestimmtes neues Feature nicht funktioniert, kann es einfach mit einem Feature-Flag für die Benutzer deaktiviert und für eine Gruppe von Entwicklern aktiviert werden, die es testen und z. B. eine Korrektur einbringen werden.
