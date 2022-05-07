Superglobale Variablen
======================

> id: bc2107ee-6551-4559-8c02-9cecdf98d33b
> slug:
> 	cs: superglobalni-promenna
> 	de: superglobale-variablen
> 
> publicationDate: '2019-11-01 09:29:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '792e217f1331f38ed0719d7abeb05e67'

Superglobale Variablen werden zur Übergabe des globalen Anwendungsstatus und der HTTP-Kommunikation verwendet.

Der größte Vorteil dieser Variablen ist, dass sie immer und überall verfügbar sind. In der Praxis handelt es sich um Arrays von Werten, bei denen der Zugriff auf bestimmte Informationen über einen Index erfolgt. In verschiedenen Kontexten kann die Verfügbarkeit von Schlüsseln variieren (siehe unten).

Arten von superglobalen Variablen
--------------------------------

Alle Superglobale in PHP sind Arrays und werden durch ein Dollarzeichen gefolgt von einem Unterstrich (außer `$GLOBALS`) und Großbuchstaben gekennzeichnet.

In `PHP 7` gibt es insbesondere die folgenden Punkte:

| Variable | Beschreibung |
|-------------|-------|
| `$_GET` | URL-Parameter <a href="/methods-odesilani-dat">mit der GET-Methode gesendet</a>
| `$_POST` | Formulardaten <a href="/methods-odesilani-dat">durch POST gesendet</a>. Beachten Sie, dass <a href="/ajax-post">sich bei ajax anders verhalten kann</a>.
| `$_REQUEST` | Formulardaten, die mit einer beliebigen Methode (`$_GET`, `$_POST` und `$_REQUEST`) gesendet werden.
| `$_FILES` | Technische Informationen über die aktuell hochgeladenen Dateien, zum Beispiel über das Konstrukt `<input type="file">`
| `$_SERVER` | <a href="/info">Web-Server-Einstellungen</a>, IP-Adresse, Konfiguration... es variiert je nach Umgebung (beim Aufruf eines PHP-Skripts aus dem Terminal wird es andere Werte enthalten und z.B. werden Informationen über die aktuelle Anfrage fehlen).
| `$_COOKIE` | Konfigurierte <a href="/cookies">Cookies</a>.
| `$_SESSION` | Sitzungsdaten (<a href="/sessions">Sitzung</a>), wenn sie existieren und in der Vergangenheit gesetzt wurden.
| `$GLOBALS` | **Warnung, es enthält keinen Unterstrich im Namen!** Dies ist die sogenannte <a href="global-variable">global-variable</a> und eine alternative Schreibweise für das Schlüsselwort `global`. Wenn Sie eine globale Variable `$variable` in Ihrer Anwendung haben, können Sie auch mit dem `$GLOBALS["variable"]` Konstrukt darauf zugreifen. Die Verwendung globaler Variablen ist jedoch von vornherein eine schlechte und unsaubere Lösung, die Sie besser nicht anwenden sollten.
| `$_ENV` | Informationen über die aktuelle Umgebung, in der PHP läuft.

Die Auflistung aller vorhandenen Werte ist leicht zu bewerkstelligen:

```php
foreach ($_SERVER as $key => $value {
	echo $key . ':' . $value . '<br>';
}
```

> Hinweis: Nicht alle Indizes müssen immer vorhanden sein (wenn das Skript z. B. cron im CLI-Modus ausführt, ist der Index mit der Seiten-URL oder der IP-Adresse der Anfrage nicht vorhanden).

Zugang zu Variablen
-------------------

Ich empfehle, dass alle globalen Variablen (außer `$_SESSION`) schreibgeschützt sind. Das liegt daran, dass sie globale Anwendungsdaten enthalten, die von anderem Code berücksichtigt werden können (z. B. von einer anderen installierten Bibliothek).

Ein weiterer Nachteil globaler Zustände ist, dass man sich nicht immer auf exakte Werte verlassen kann, selbst wenn sie existieren, so dass man ihre Schlüssel immer mit dem `isset()`-Konstrukt überprüfen sollte.

Um ein neues Cookie zu speichern, verwenden Sie `setcookie()` und geben Sie den Wert nicht direkt ein. Dies liegt daran, dass sie schreibgeschützt ist.

Gelernte Lektionen
-------

Vertrauen Sie niemals blind auf die Werte superglobaler Variablen!

Der Benutzer kann über die URL und die gesendeten Header beeinflussen, wie die Werte gesetzt werden. Alle Eingaben sollten stets sorgfältig validiert werden.

Register globals - das Problem mit der alten PHP-Version
------------------------------------------

In der alten Version von PHP (bis `5.4.0`) gab es eine spezielle `register-globals` Direktive (konfigurierbar in `php.ini`), die bewirkte, dass alle übergebenen Parameter in einer URL automatisch als Variablen registriert wurden.

Zum Beispiel:

Ein Benutzer hat die URL "https://example.com/script.php?var=24" aufgerufen.

Und PHP hat im Skript automatisch eine Variable `$var` mit dem Wert `24` angelegt.

Es hat also ganz klassisch funktioniert:

```php
echo $var;
```

Jeder könnte also eine beliebige Variable in das Skript einfügen und dessen Inhalt ändern. Offensichtlich hatte die Sicherheit nicht immer Priorität. Nicht wirklich.

Andere Quellen
------------

Eine ausführlichere Beschreibung finden Sie im <a href="https://www.php.net/manual/en/language.variables.superglobals.php">offiziellen Handbuch</a>.
