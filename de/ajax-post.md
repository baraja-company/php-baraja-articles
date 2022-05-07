Verarbeitung von Ajax-POST-Anfragen in PHP
==========================================

> id: c9c8fdb4-020d-4361-b425-4f4406a090ba
> slug:
> 	cs: ajax-post
> 	de: verarbeitung-von-ajax-post-anfragen-in-php
> 
> publicationDate: '2019-11-01 09:56:02'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '89254da0b9b22b1926410b3ef7e511ec'

Bei der Entwicklung von ajax Vue.js Anwendungen habe ich nach Jahren endlich herausgefunden, wie man ajax in PHP verwenden kann und wie man Daten per POST Methode erhält.

Die superglobale Variable `$_POST` ist nur für Formulare verfügbar
-------------------------------------------------------------

In PHP ist die <a href="/superglobale-variable">superglobale Variable `$_POST`</a> allgemein verfügbar, um die übermittelten Daten aus einem Formular zu speichern.

Es ist **relativ** einfach zu bedienen.

Auf der HTML-Seite müssen Sie ein Formular erstellen:

```html
<form action="process.php" method="post">
    Jméno: <input type="text" name="username">
    <input type="submit" value="Odeslat">
</form>
```

In der Datei `process.php` kann dann auf die Werte als Array-Elemente zugegriffen werden:

```php
echo htmlspecialchars($_POST['Nutzername'] ?? '');
```

> **Warnung:**
>
> Mit diesem einfachen Ansatz könnte jeder denken, dass POSTed-Daten automatisch als Array-Indizes in der Variable `$_POST` definiert sind. Aber das ist nicht wahr!

Der Grund, warum Daten, die von einem Formular mit der POST-Methode gesendet werden, in die Variable `$_POST` geschrieben werden, liegt darin, dass der Browser automatisch den HTTP-Header `'Content-Type': 'application/x-www-form-urlencoded'` sendet, wenn das HTML-Formular abgeschickt wird.

Ohne eine richtig gesetzte Kopfzeile kann auf die Werte nicht zugegriffen werden, und wir müssen zu einer Tricklösung greifen.

Senden von Daten per Ajax
-------------------

Wenn wir versuchen, Daten über Ajax zu senden, müssen wir den Ansatz auf der PHP-Seite ein wenig ändern. Sie können <a href="https://www.facebook.com/groups/frontendisti/permalink/2372671669611010/">die Diskussion auf Facebook</a> lesen, um Einzelheiten zu erfahren.

In Javascript können Sie zum Beispiel die <a href="https://github.com/axios/axios">axios</a>-Bibliothek verwenden, um Daten per Ajax zu senden. Um es einfach zu nutzen, verlinken Sie einfach Javascript vom CDN-Server und verwenden es sofort:

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script>
    axios.post('/api/form-process', {
        username: 'Jméno uživatele'
    })
    .then(response => {
        // Zpracování odpovědi z API
        alert(response.data.message); // Vyhodí hlášku se zprávou
    });
</script>
```

In diesem einfachen Fall wird die URL `'/api/form-process'` über Ajax aufgerufen und die POST-Methode übergibt das Objekt `{ username: 'User name' }`. Die Bibliothek selbst kümmert sich bereits automatisch um die Logistik der Datenübergabe, so dass sie als Json serialisiert gesendet werden. Im Sprachgebrauch der Frontend-Entwickler wird dies **json payload** genannt.

Auf der PHP-Seite würde ich erwarten, dass es als Formular verwendet wird (schließlich war es eine POST-Methode):

```php
echo htmlspecialchars($_POST['Nutzername'] ?? '');
```

In diesem Fall ist das Feld "$_POST" jedoch leer und es werden keine Daten übergeben. Die Variable `$_POST` wird nur für Daten verwendet, die von Formularen abgerufen werden (Sie erkennen das an dem HTTP-Header, den wir nicht weggeworfen haben).

In diesem Fall müssen wir also die Daten direkt aus der HTTP-Anfrage abrufen, wofür die **Tricklösung** verwendet wird. Es ist dann einfach zu benutzen:

```php
$data = json_decode(file_get_contents('php://eingabe'), true);

echo htmlspecialchars($data['Nutzername'] ?? '');

header('Inhalt-Typ: application/json');
echo json_encode([
    'Nachricht' => 'Der Server-Schleicher',
]);
die;
```

Als Beispiel gebe ich auch eine einfache Javascript-Antwort. Wichtig ist, dass der HTTP-Header `'Content-Type: application/json'` richtig gesetzt wird und das Skript beendet wird, nachdem alle Daten gesendet wurden.

Erzwingen der Verwendung von `$_POST`
-------------------------

Wenn Sie die übermittelten Daten dennoch direkt als Formular behandeln wollen, gibt es eine Möglichkeit, sie zu übertragen. In diesem Fall müssen Sie die Erstellung der Ajax-Abfrage selbst ändern und die HTTP-Header korrekt übergeben:

```js
axios.post(
    '/api/form-process',
    {
        username: 'Jméno uživatele'
    },
    {
        headers: {'Content-Type': 'application/x-www-form-urlencoded'}
    }
).then(response => {
    // Nějaké zpracování
});
```

In diesem Fall wird die Verarbeitung auf der PHP-Seite jedoch nicht angenehm sein, da wir die Daten noch korrigieren und in ein Array umwandeln müssen.

Ich habe es geschafft, diesen Horror zu erfinden:

```php
if (\count($_POST) === 1
    && preg_match('/^\{.*\}$/', $post = array_keys($_POST)[0])
    && ($json = json_decode($post)) instanceof \stdClass
) {
    foreach ($json as $key => $value) {
        $_POST[$key] = $value;
        unset($_POST[$post]);
    }
}

echo htmlspecialchars($_POST['Nutzername'] ?? '');
```

Es ist jedoch viel besser, sich an den ersten Ansatz zu halten und die Methode `'php://input'` zu verwenden, um die Daten zu erhalten.
