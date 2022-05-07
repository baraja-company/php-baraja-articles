Cookies in PHP
==============

> id: '392dd88b-d2f5-4943-a993-01aaad7ccd32'
> slug:
> 	cs: cookies
> 	de: cookies-in-php
> 
> perex:
> 	- 'Cookies je krátká textová informace v paměti prohlížeče, kam můžeme v PHP zapisovat a označit si uživatele.'
> 	- 'Ein Cookie ist ein kurzes Stück Textinformation im Speicher des Browsers, in das wir den Benutzer in PHP schreiben und markieren können.'
> 
> publicationDate: '2019-09-11 10:18:29'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '78f9aad0278acd51b98179e7d29fe3d8'

**Warnung:** Dieser Artikel wurde vor vielen Jahren geschrieben und einige Informationen sind möglicherweise veraltet oder falsch. Bitte beachten Sie dies beim Lesen.

Cookies sind kleine Textinformationen, die im Browser des Besuchers einer Website gespeichert werden. Sie werden immer mit jeder neu geladenen Seite übertragen und können vom Benutzer jederzeit gelöscht, geändert und gelesen werden, so dass sie zur Speicherung persönlicher Daten nicht gut geeignet sind.

*Warnung: Wenn Ihre Website Cookies zur Nachverfolgung von Nutzern oder Add-ons von Drittanbietern (z. B. Facebook-Like-Button, Google-Analytics-Traffic-Meter, Werbebanner) verwendet, müssen Sie die Nutzer darüber informieren.*

> "Noch ein Hinweis: Ihre Website sollte keine Werbe- oder Messcodes enthalten, solange Sie keine Zustimmung eingeholt haben. Und das ist scheiße."
>
> -- <a href="https://phpfashion.com/jak-na-souhlas-s-cookie-ve-zkurvene-eu">David Grudl</a>

Einen Wert aus einem Cookie lesen
--------------------------

Alle Cookies werden in der superglobalen Variable `$_COOKIE` gespeichert, die jeden Schlüssel als Array speichert.

Wenn wir zum Beispiel den Namen des aktuell angemeldeten Benutzers unter dem Schlüssel `user` im Cookie gespeichert haben, können wir ihn leicht abrufen:

```php
echo $_COOKIE['Benutzer'];
```

Bitte beachten Sie: Es kann sein, dass Cookies nicht immer vorhanden sind (zum Beispiel, wenn Sie ein neuer Benutzer sind). Daher sollten wir vor jeder Auflistung immer prüfen, ob Cookies vorhanden sind, und gegebenenfalls eine alternative Fehlermeldung anbieten.

```php
if (isset($_COOKIE['Benutzer']) && $_COOKIE['Benutzer']) {
    echo 'Angemeldeter Benutzer:' . $_COOKIE['Benutzer'];
} else {
    echo 'Niemand hat sich angemeldet.';
}
```

Alle verfügbaren Cookies abrufen
--------------------------------

Da alle Cookies in der superglobalen Variable `$_COOKIE` gespeichert sind, können sie leicht aufgelistet werden:

```php
var_dump($_COOKIE);
```

Alternativ können Sie auch den Zyklus durchlaufen und alle Schlüssel und Werte abrufen:

```php
foreach($_COOKIE as $key => $value) {
    echo $key . ':' . $value;	// Schreiben Sie den Schlüssel und den Wert aus
    echo '<br>';				// die Zeile umbrechen
}
```

Speichern des Wertes in einem Cookie
--------------------------

Die Funktion `setcookie()` wird verwendet, um Daten in Cookies zu speichern.

Der erste Parameter ist der Cookie-Schlüssel, der verwendet wird, um ihn aus dem Feld "$_COOKIE" zu lesen, und der zweite Parameter ist die Zeichenkette mit den Daten selbst.

Mit dem dritten Parameter können wir (optional) die Gültigkeitsdauer festlegen, für die das Cookie verfügbar sein soll. Der Zeitpunkt der Verfügbarkeit wird als <a href="/date">Zeitstempel</a> angegeben. Wenn wir also einen Cookie mit einer Gültigkeit von 1 Stunde ab diesem Zeitpunkt setzen wollen, müssen wir nur `time() + 3600` schreiben.

```php
$data = 'Einige Inhalte, die wir speichern wollen.';

setcookie('TestCookie', $data);
setcookie('TestCookie', $data, time() + 3600);
```

Speichern von größeren Daten
-------------------

Cookies sind nicht geeignet, um größere Daten zu speichern (Browser erlauben in der Regel nur 4 kB und maximal 20 Cookies, wobei die Größe auch Cookie-Namen, Gültigkeitseinstellungen usw. umfasst).

Es ist besser, größere Daten auf dem Server zu speichern und nur eine Kennung in den Cookie zu setzen, anhand derer wir erkennen können, zu welchem Benutzer er gehört. Diese Methode heißt `$_SESSION` und wird in einem separaten Artikel behandelt.

Wenn Sie Daten nicht unbedingt immer synchron auf dem Server speichern müssen, können Sie den in Javascript verfügbaren **<a href="https://jecas.cz/localstorage">Localstorage</a>** Speicher verwenden. Seine Kapazität liegt in der Größenordnung von MB und die Daten unterliegen keinem Verfall.
