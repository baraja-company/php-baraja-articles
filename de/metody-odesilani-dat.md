Datenübertragungsmethoden (GET und POST)
========================================

> id: '32f9083f-7cb1-469f-911a-765df059123d'
> slug:
> 	cs: metody-odesilani-dat
> 	de: datenuebertragungsmethoden-get-und-post
> 
> perex:
> 	- 'Metoda GET a POST, získávání dat z formuláře a URL. Komunikace přes API a zpracování dat.'
> 	- 'GET- und POST-Methode, Abrufen von Daten aus Formularen und URL, API-Kommunikation und Datenverarbeitung.'
> 
> publicationDate: '2019-11-26 11:38:32'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '2282538597ebfe95877ae0e005ddd352'

Neben den regulären Variablen gibt es in PHP auch sogenannte **superglobale Variablen**, die Informationen über die aktuell aufgerufene Seite und die übergebenen Daten enthalten.

Normalerweise haben wir ein Formular auf einer Seite, das der Benutzer ausfüllt, und wir wollen diese Daten an den Webserver übertragen, wo wir sie in PHP verarbeiten.

Hierfür gibt es 3 Methoden, die am häufigsten verwendet werden:

- `GET` ~ die Daten werden in der URL als Parameter übergeben
- POST" ~ die Daten werden verdeckt mit der Seitenanforderung übergeben
- <a href="/ajax-post">Ajax POST</a> ~ asynchrone Javascript-Verarbeitung

GET-Methode - `$_GET`
--------------------

Die mit der GET-Methode gesendeten Daten sind in der URL sichtbar (als Parameter nach dem Fragezeichen), die maximale Länge beträgt 1024 Zeichen im Internet Explorer (andere Browser schränken dies *fast* nicht ein, aber größere Texte sollten nicht auf diese Weise übergeben werden). Der Vorteil dieser Methode liegt vor allem in der Einfachheit (Sie können sehen, was Sie senden) und in der Möglichkeit, einen Link zum Ergebnis der Verarbeitung anzugeben. Die Daten werden an eine Variable gesendet.

Die Adresse der Empfangsseite könnte wie folgt aussehen:

https://____________.com/script.php?promenna=obsah&promenna2=obsah".

In PHP können wir dann zum Beispiel den Wert des Parameters `Variable` wie folgt schreiben:

```php
echo $_GET['.'];	// druckt "Inhalt"
```

> Diese Methode, Daten direkt in die HTML-Seite zu schreiben, ist nicht sicher, da wir z. B. HTML-Code in der URL übergeben können, der in die Seite geschrieben und dann ausgeführt werden würde.
>
> Wir müssen **immer** die Daten vor der Ausgabe auf der Seite behandeln, die Funktion `htmlspecialchars()` wird dafür verwendet.
>
> Zum Beispiel: "echo htmlspecialchars($_GET['variable']);`

POST-Methode - `$_POST`
----------------------

Die mit der POST-Methode gesendeten Daten sind in der URL nicht sichtbar, wodurch das Problem der maximalen Länge der gesendeten Daten gelöst wird. Für die Übermittlung von Formularfeldern sollte immer die POST-Methode verwendet werden, da auf diese Weise sichergestellt wird, dass z. B. Passwörter nicht sichtbar sind und dass kein Link zu der Seite bereitgestellt werden kann, die das Ergebnis einer bestimmten Eingabe verarbeitet.

Die Daten sind in der Variablen "$_POST" verfügbar, und die Verwendung ist dieselbe wie bei der GET-Methode.

Überprüfung des Vorhandenseins der gesendeten Daten
--------------------------------

Bevor wir Daten verarbeiten, sollten wir uns vergewissern, dass die Daten auch tatsächlich gesendet wurden, da wir sonst auf die
 auf eine nicht existierende Variable, was zu einer Fehlermeldung führen würde.

Die Funktion `isset()` wird verwendet, um das Vorhandensein einer Variablen zu überprüfen.

```php
if (isset($_GET['Name'])) {
    echo 'Ihr Name:' . htmlspecialchars($_GET['Name']);
} else {
    echo 'Es wurde kein Name eingegeben.';
}
```

Formular zur Dateneingabe
------------------------

Das Formular ist in HTML erstellt, nicht in PHP. Sie kann auf einer einfachen HTML-Seite stehen. Die gesamte "Magie" wird von dem PHP-Skript übernommen, das die Daten annimmt.

Als Beispiel können wir ein Formular verwenden, um 2 Zahlen zu erhalten, die mit der GET-Methode gesendet werden:

```html
<form action="script.php" method="get">
    První číslo: <input type="text" name="x">
    Druhé číslo: <input type="text" name="y">

    <input type="submit" value="Sečíst čísla">
</form>
```

In der ersten Zeile können Sie sehen, wohin die Daten gesendet werden und mit welcher Methode.

Die nächsten 2 Zeilen sind einfache Formularelemente, beachten Sie das Attribut **name=""**, das den Namen der Variablen enthält, die den Inhalt des Formulars enthält.

Es folgen die Schaltfläche zum Übermitteln der Daten (obligatorisch) und der abschließende HTML-Tag des Formulars (obligatorisch, damit der Browser weiß, was noch zu übermitteln ist und was nicht).

> Wir können beliebig viele Formulare auf einer Seite haben, und sie können nicht verschachtelt werden. Bei einer Verschachtelung wird immer das am stärksten verschachtelte Formular gesendet, die übrigen werden ignoriert.

Formularverarbeitung auf dem Server
-------------------------------

Jetzt haben wir ein fertiges HTML-Formular und senden es an `script.php`, das die Daten mit der GET-Methode erhält. Die Adresse der Seitenanforderung könnte wie folgt aussehen:

`https://________.com/script.php?x=5&y=3`

**script.php**

```php
$x = $_GET['x'];	// 5
$y = $_GET['y'];	// 3

echo $x + $y;		// druckt 8
```

Korrekterweise sollten wir zuerst überprüfen, ob beide Formularfelder ausgefüllt wurden, dies geschieht mit der Funktion `isset()`:

```php
if (isset($_GET['x']) && isset($_GET['y'])) {
    $x = $_GET['x'];	// 5
    $y = $_GET['y'];	// 3

    echo $x + $y;		// druckt 8
} else {
    echo 'Das Formular wurde nicht korrekt ausgefüllt.';
}
```

> **TIP:** Sie können mehrere Parameter an das `isset()`-Konstrukt übergeben, um zu überprüfen, ob sie alle existieren.
>
> Anstelle von `isset($_GET['x']) && isset($_GET['y'])` können Sie also einfach angeben:
>
> `isset($_GET['x'], $_GET['y'])`.

Verarbeitung der mit der POST-Methode empfangenen Daten
--------------------------------------

Wenn die Daten mit der POST-Methode empfangen werden, sieht die URL des zu verarbeitenden Skripts immer wie folgt aus:

https://________.com/script.php".

Und niemals anders. Einfach nein. Die Daten sind in der HTTP-Anfrage versteckt und wir können sie nicht sehen.

> Die versteckte POST-Methode ist aus Sicherheitsgründen erforderlich, um Benutzernamen und Kennwörter zu senden.
>
**Sicherheit:** Wenn Sie auf Ihrer Website mit Passwörtern arbeiten, sollte das Anmelde- und Registrierungsformular über HTTPS gehostet werden, und Sie müssen die Passwörter entsprechend hacken (z. B. mit BCrypt).

Handhabung von Ajax-Anfragen
------------------------------

In manchen Fällen kann es bei der Verarbeitung von Ajax-Anfragen schwierig sein, die Daten abzurufen. Der Grund dafür ist, dass Ajax-Bibliotheken in der Regel Daten als "json payload" senden, während die superglobale Variable "$_POST" nur Formulardaten enthält.

Auf die Daten kann weiterhin zugegriffen werden, die Details habe ich im Artikel <a href="/ajax-post">Handling ajax POST requests</a> beschrieben.
