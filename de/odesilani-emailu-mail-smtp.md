Versenden von E-Mails (mail() und SMTP-Funktionen) in PHP
=========================================================

> id: '3051b5ea-9408-4aaa-8209-e3c527ef8ad2'
> slug:
> 	cs: odesilani-emailu-mail-smtp
> 	de: versenden-von-e-mails-mail-und-smtp-funktionen-in-php
> 
> perex:
> 	- 'Možnosti odesílání e-mailů v PHP, funkce mail(), SMTP, hlavičky, konfigurace a Nette Mailer.'
> 	- 'E-Mail-Versandoptionen in PHP, mail(), SMTP, Header, Konfiguration und Nette Mailer.'
> 
> publicationDate: '2019-11-26 12:00:02'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '53dc8075e16053ea9284299987c64952'

In PHP gibt es grundsätzlich 2 Möglichkeiten, Mails zu versenden:

- Die native Funktion `mail()`, die einige Einschränkungen hat,
- oder über einen SMTP-Server.

Die Funktion `mail()` muss den SMTP-Server verwenden, was ein sehr einfacher Weg ist, um E-Mails über den SMTP-Server zu versenden.
---------------

Die Idee dahinter ist einfach: Sie rufen die Funktion auf:

```php
mail('jan@barasek.com', 'Thema', 'Der Text der Nachricht...');
```

Und PHP wird das Senden selbst übernehmen.

Intern funktioniert der Versand, indem die Konfiguration aus der `php.ini` gelesen wird und der Standard-SMTP-Server gesucht wird, über den die E-Mail zugestellt werden soll. Dies erfordert also eine vorherige Konfiguration des Webservers.

Der größte Nachteil der Funktion `mail()` ist, dass der Programmierer die gesamte Logik selbst herausfinden muss. Dazu gehört z. B. das Wegwerfen von Kopfzeilen über die Verschlüsselung, die Verknüpfung von Zertifikaten zur Verschlüsselung von Nachrichten und so weiter.

Im Falle eines Sendefehlers wird ein "falscher" Wert zurückgegeben, den wir abfangen und selbst verarbeiten müssen. Wir können den spezifischen Fehler auf begrenzte Weise herausfinden, indem wir `error_get_last()` aufrufen, so zum Beispiel:

```php
if (@mail($to, $subject, $message) === false) {
	throw new \Exception(
		'Kann keine E-Mail senden:'
		. (@error_get_last()['Nachricht'] ?? '')
	);
}
```

> **TIP:** Beachten Sie, dass wir die Adresse, von der wir die E-Mail senden wollen, und die zu verwendende Kodierung nicht angegeben haben.
>
> Alle diese Einstellungen müssen über Header übergeben werden.

Wenn Sie dennoch die Funktion `mail()` verwenden müssen (zum Beispiel wegen des Hostings), empfehle ich das Paket `nette/mail` und den Dienst `SendmailMailer`, der das Versenden von E-Mails gut handhabt.

SMTP-Server
-----------

SMTP steht für `Simple Mail Transfer Protocol`, was (wie Sie gleich sehen werden) sehr zutreffend ist.

SMTP ist im Gegensatz zu `mail()` ein fortschrittlicheres Protokoll mit erweiterten Konfigurationsmöglichkeiten nicht nur auf der PHP-Seite, sondern auch direkt auf dem Mailserver.

Die SMTP-Unterstützung auf Hosts ist 2018 ausgezeichnet.

SMTP funktioniert im Wesentlichen so, dass PHP zunächst eine Verbindung zum SMTP-Server herstellt (dazu ist die Erweiterung `php_openssl.dll` in PHP erforderlich, die Sie wahrscheinlich bereits aktiviert haben), sich während der Verbindung authentifiziert (d.h. überprüft, ob die Anmeldedaten korrekt sind), und dann können wir mit dem Server auf ähnliche Weise wie mit einer Datenbank kommunizieren - d.h. einzelne Anfragen senden, aber immer eine einzige Verbindung aufrechterhalten. Ein großer Vorteil von SMTP ist die direkte Unterstützung von Verschlüsselung (bekannt als `TLS`).

E-Mails von localhost aus versenden - eine einfache Lösung
--------------------------------------------------

Ich muss oft E-Mails von localhost aus versenden, wenn ich eine neu geschriebene Anwendung teste.

> **Für Tipps:**
>
> Auf einem Mac ist die Situation einfach, weil der MAMP-Server auf "magische Weise" das aktuell angemeldete Apple Mail-Konto findet und Nachrichten immer von diesem Konto aus gesendet werden.

Allerdings kann man sich nicht immer auf dieses Verhalten verlassen, und es ist eine gute Idee, eine eigene Lösung einzurichten. Wenn Sie über eine Internetverbindung und ein Google-Konto verfügen, ist es sehr einfach, ein Gmail-Konto zu verwenden, mit dem Sie sich direkt von PHP aus verbinden und E-Mails darüber versenden können.

Wenn Sie das Paket `nette/mail` verwenden, ist die Konfiguration einfach:

```neon
mail:
	smtp: true
	host: smtp.gmail.com
	username: janbarasek@gmail.com
	password: *********
	secure: ssl
```

> Das Kennwort ist nicht das Anmeldekennwort für Ihr Konto (das wäre unsicher und Sie könnten z. B. keine **Zwei-Faktor-Authentifizierung** verwenden).
>
> Sie müssen ein so genanntes "Anwendungspasswort" verwenden, was in der Praxis bedeutet, dass Sie <a href="https://myaccount.google.com/apppasswords">Ihre Anwendung</a> direkt in Ihrem Google-Konto registrieren, dem ein zufällig generiertes Passwort zugewiesen wird, das Sie in PHP eingeben und über das es gesendet werden kann.
>
> Ausführliche Anweisungen finden Sie <a href="https://support.google.com/accounts/answer/185833?hl=cs">auf der Website von Google</a>.

Mail auf Wedos konfigurieren
---------------------------

Mit Wedos-Hosting kann man nur 500 E-Mails pro Tag versenden, und ich hatte eine Zeit lang Probleme mit der SMTP-Verbindung.

Durch das `nette/mail`-Paket wird es wie folgt gemacht (eine praktikable Lösung):

```neon
mail:
	smtp: true
	host: smtp-*******.wedos.net
	username: jan@barasek.com
	password: ******
	secure: tls
	port: 587
```

Der Parameter "host" ist für jedes Hosting unterschiedlich und kann in der E-Mail gefunden werden, die Wedos bei der Registrierung des Hostings sendet.

Der `Benutzername` steht für das Postfach, von dem aus die Mails gesendet werden. Die Mailbox muss vorhanden sein. Beim Versenden von E-Mails in PHP müssen wir auch die Sendeadresse auf dieselbe Adresse setzen (in Nette mit der Methode `->setFrom()`).

Wenn wir die Konfiguration nicht genau und korrekt ausfüllen, werden verschiedene Fehlermeldungen ausgegeben und die Mails können nicht versendet werden.

Wenn die Anzahl der gesendeten Nachrichten überschritten wird, wird eine Ausnahme ausgelöst, die über die Überschreitung des Limits informiert.
