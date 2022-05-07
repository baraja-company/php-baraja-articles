Einrichten eines HTTPS/SSL-Zertifikats - vollständige Anleitung
===============================================================

> id: '0346a091-2b2c-494f-b2fb-573796d4fb46'
> slug:
> 	cs: jak-nastavit-https-ssl-certifikat
> 	de: einrichten-eines-https-ssl-zertifikats---vollstaendige-anleitung
> 
> publicationDate: '2019-09-16 09:01:35'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '67b2b3ccce4815204a4bcadce781fa59'

Beim Einsatz des Protokolls `https` auf Kunden-Websites stieß ich oft auf verschiedene Schwierigkeiten, die auf ein mangelndes Verständnis der Probleme und eine zu große Komplexität der Konzepte zurückzuführen waren.

In diesem Tutorial beschreibe ich detailliert die Schritte, um ein gültiges Zertifikat auf einem Webserver zu erhalten und einzusetzen.

**In jeder Zwischenüberschrift fasse ich immer kurz den Schritt für Fortgeschrittene zusammen, und am Ende gehe ich auf die Details für Anfänger ein.**

**Warnung:** Der gesamte Prozess der Bereitstellung eines Zertifikats kann über eine Stunde dauern und ist oft unterbrochen (die Website kann nicht verfügbar sein).

Anforderungen an die Eingabe
-----------------

Die Anweisungen gehen davon aus, dass wir Zugang zu einem Terminal-Webserver haben, der unter Linux läuft und Apache verwendet.

Für Nginx gilt die gesamte Theorie gleichermaßen, nur die Verknüpfung der Zertifikatsdatei ist anders.

## Verbindung zum Webserver

Wir verbinden uns über SSH mit dem Server.

- Unter Windows empfehle ich das Programm **Putty**,
- Auf Mac oder Linux verwenden Sie einfach das integrierte Terminal.

Auf Mac oder Linux rufen Sie den Befehl auf:

```htaccess
ssh uživatel@server
```

Ich möchte zum Beispiel eine Verbindung mit dem Benutzer `root` auf der Website `baraja.cz` herstellen:

```htaccess
ssh root@baraja.cz
```

Oder an den Benutzer `jan` zu einer bestimmten IP-Adresse:

```htaccess
ssh jan@127.0.0.1
```

Nach dem Absenden Ihrer Anfrage wird die Verbindung entweder direkt hergestellt oder Sie werden nach einem Passwort gefragt. Bei der Eingabe des Passworts wird nichts angezeigt. Bestätigen Sie das Passwort mit der Eingabetaste und warten Sie, bis die Verbindung autorisiert wird.

> **Warnung:** Wenn wir keine Rechte für eine Aktion haben, müssen wir sie zuweisen. Wechseln Sie entweder direkt zum Benutzer `root` mit dem Befehl `sudo su`, oder stellen Sie dem Befehl, den Sie als root ausführen wollen, das Wort `root` voran, z.B. `root rm <name>` unter root wird die Datei `<name>` löschen. Bei der Verwendung des `sudo`-Befehls kann es vorkommen, dass wir regelmäßig nach einem Passwort gefragt werden.

**Details:**

Der SSH-Zugang wird von dem jeweiligen Hosting-Unternehmen eingerichtet, bei dem Sie einen Server gemietet haben.

- Im Falle von VPS erhalten Sie immer SSH-Zugang.
- Beim Hosting steht Ihnen SSH möglicherweise gar nicht zur Verfügung, und die Konfiguration erfolgt auf andere Weise (in der Regel über die Weboberfläche, oder Sie wenden sich an den Support).

## Umstellung von HTTP auf HTTPS

Wenn Sie eine bestehende Website von `http` auf `https` umstellen, müssen Sie sicherstellen, dass der gesamte Verkehr auf das neue `https`-Protokoll umgeleitet wird.

Im Falle von Apache kann dies leicht durch eine Umleitung in der Datei `.htaccess` erreicht werden:

```htaccess
<IfModule mod_rewrite.c>
	RewriteEngine On

	RewriteCond %{HTTPS} !on
	RewriteRule .? https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```

Diese Konfiguration stellt sicher, dass alle Anfragen an `http` mit `HTTP-Code 301` auf `https` umgeleitet werden. Diese Konfiguration ist die Standardeinstellung für das Nette-Framework, gilt aber auch für alle anderen Fälle.

**Details:**

Die Datei `.htaccess` enthält die spezifische Webserver-Konfiguration, die sich auf jede Anfrage auswirkt. Sie befindet sich in der Regel im gleichen Verzeichnis wie die Datei "index.php" oder andere Dateien, die über das Internet zugänglich sind.

Die Einstellung ist nur für den Apache-Server gültig und kann bei einigen Hosts deaktiviert oder eingeschränkt sein. Ausführlichere Informationen erhalten Sie immer bei dem Unternehmen, bei dem Sie Ihre Website hosten.

-----

Manchmal kommt es vor, dass `.htaccess` nicht gut zusammenarbeiten will und die Konfiguration extrem schwierig ist (zum Beispiel für viele Domains, die sich alle unterschiedlich verhalten). In einem solchen Fall kann die Umleitung zu HTTPS notfalls direkt in PHP erfolgen:

```php
if (empty($_SERVER['HTTPS']) || $_SERVER['HTTPS'] === 'aus') {
	$location = 'https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'];
	header('HTTP/1.1 301 Dauerhaft verschoben');
	header('Standort:' . preg_replace('/^(https:\/\/(?:www\.)(.*)$/', '$1$2', $location));
	die;
}
```

Fügen Sie das Skript in `index.php` ein. Ich empfehle diese Lösung jedoch nicht.

## Dateien mit virtuellen Hosts finden

Im Falle von Apache müssen wir die Datei Virtual hosts finden.

Sie befinden sich normalerweise unter dem Pfad `/etc/apache2/sites-available`.

Listen Sie den Inhalt des Verzeichnisses mit dem Befehl `ls -al` auf und finden Sie die Datei, in der die virtuelle Datei für unsere Website eingerichtet ist.

VirtualHosts befinden sich in der Regel in Dateien mit der Erweiterung `.conf`. Die Voreinstellung ist oft in `000-default.conf` zu finden.

**Details:**

- Das Verzeichnis wird mit dem Befehl `cd` geöffnet, zum Beispiel `cd /etc/apache2/sites-available`.
- Der Inhalt der Datei wird mit dem Befehl `cat <name>` in das Terminal geschrieben oder mit den Befehlen `nano <name>` oder `vim <name>` bearbeitet.
- Der Editor wird normalerweise mit der Tastenkombination "STRG + X" oder durch zweimaliges Drücken von "ESC" aufgerufen.
- Dateien können teilweise in der GUI mit dem Midnight Commander (`mc` Befehl) durchsucht werden, der im Falle von Ubuntu einfach mit `apt-get install mc` oder `sudo apt-get install mc` installiert wird.

## Einrichten eines virtuellen Hosts für Port 443

Innerhalb des virtuellen Hosts müssen wir einen neuen für den Port "443" vorbereiten (ein häufiges Problem kann die Blockierung des Ports 443 durch die Firewall sein).

Der Inhalt der Datei könnte zum Beispiel so aussehen:

```htaccess
<IfModule mod_ssl.c>
     <VirtualHost *:443>
         ServerName baraja.cz

         ServerAdmin jan@barasek.com
         DocumentRoot /var/www/baraja.cz/www
         Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains; preload"
         ErrorLog ${APACHE_LOG_DIR}/baraja.cz.ssl.error.log
         CustomLog ${APACHE_LOG_DIR}/baraja.cz.ssl.access.log combined
         SSLEngine on
         SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
         SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
         SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
         SSLProtocol             all -SSLv3
         SSLCipherSuite          ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS
         SSLHonorCipherOrder     on
         SSLCompression          off
         <FilesMatch "\.(cgi|shtml|phtml|php)$">
                 SSLOptions +StdEnvVars
         </FilesMatch>
         <Directory /usr/lib/cgi-bin>
                 SSLOptions +StdEnvVars
         </Directory>
		 BrowserMatch "MSIE [2-6]" nokeepalive ssl-unclean-shutdown downgrade-1.0 force-response-1.0
         # MSIE 7 and newer should be able to use keepalive
         BrowserMatch "MSIE [17-9]" \
         ssl-unclean-shutdown
     </VirtualHost>
</IfModule>
```

Die Datei ist der wichtigste Bereich:

```htaccess
SSLEngine on
SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
```

Das Verständnis für diese Konfiguration ist absolut entscheidend. Wenn Sie nach detaillierteren Informationen suchen müssen, verwenden Sie die Wörter `SSLCertificateFile`, `SSLCertificateKeyFile` und `SSLCertificateChainFile`, die allen Apache-Servern gemeinsam sind.

**Warnung:** Das SSL-Problem ist relativ alt und es werden oft verschiedene Namen für dieselbe Sache verwendet, um die Abwärtskompatibilität zu wahren! Es ist daher wichtig, das Prinzip zu verstehen.

**Details:**

Es ist wichtig, dass VirualHost enthält:

- `<IfModule mod_ssl.c>` sagt, dass wir SSL verwenden wollen
- <VirtualHost *:443>", dass die gesamte Kommunikation über Port 443 läuft
- SSLEngine on", dass SSL für diesen VirtualHost aktiviert ist
- Die Dateien `SSLCertificateFile`, `SSLCertificateKeyFile` und `SSLCertificateChainFile` sind Dateien mit bestimmten Schlüsseln.

Mehr ist nicht nötig.

## Verstehen des Prinzips, was wir tun werden und wie das Zertifikat funktioniert

In der endgültigen Konfiguration von VirtualHost benötigen wir eigentlich nur 3 Dateien `SSLCertificateFile`, `SSLCertificateKeyFile` und `SSLCertificateChainFile`, die wir irgendwo auf dem Server ablegen können (Name und Ort spielen keine Rolle). Es ist wichtig, dass ein Arbeitspfad angegeben wird und dass der Inhalt der Dateien gültig ist.

Die spezifische Methode zur Beschaffung der Zertifikate kann von CA zu CA unterschiedlich sein. Es kommt darauf an, den Grundsatz zu verstehen und ihn dann auf Ihren Fall anzuwenden.

| Datei | Bedeutung |
|---------------------------|-------------------------------------|
| Dieses Zertifikat wird von der Behörde **gesendet** | "SSLCertificateFile" |
| "SSLCertificateKeyFile" | **Mein generierter** privater Schlüssel |
| `SSLCertificateChainFile` | Ich habe aus dem Web heruntergeladen Typ **intermediate + root** |

## Abrufen des privaten Schlüssels und Anfordern des Zertifikats

Auf dem Server erzeugen wir zunächst den privaten Schlüssel. Das Wort **privat** ist wichtig, denn es bedeutet, dass niemand außer dem Webserver diese Daten kennt. Im Idealfall sollte sie den Server nie verlassen und an einem sicheren Ort aufbewahrt werden. Der Verlust dieses Schlüssels bedeutet einen Verlust an Sicherheit, da ein Angreifer in der Lage ist, sich als ein bestimmter Server auszugeben.

Um den Schlüssel zu erzeugen, verwenden Sie den Befehl:

```shell
openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.key -out yourdomain.csr
```

Um es zu generieren, muss das Programm `openssl` auf dem Server installiert sein, was wir zum Beispiel durch `sudo apt install openssl` erreichen können.

Es kann mehrere Arten von Schlüsseln geben, in diesem Fall erzeugen wir einen RSA-Schlüssel mit einer Länge von 2048 Bytes (`rsa:2048`).

Die Ausgabe des Befehls sind 2 Dateien (die Sie entsprechend Ihrer Domain benennen):

- yourdomain.key" - dies ist der private Schlüssel. Speichern Sie den Pfad zu diesem Schlüssel in Apache VirtualHost unter `SSLCertificateKeyFile`.
- IhreDomain.csr" - dies ist eine "Zertifikatsanforderung" oder ein Antrag auf Ausstellung eines Zertifikats.

Der Inhalt des Antrags muss immer der zuständigen Behörde zur Genehmigung vorgelegt werden. Dies geschieht in der Regel über die Webschnittstelle in der Verwaltung auf der Website, auf der die Zertifikate verkauft werden. Die Genehmigung des Antrags hängt von der Art des Zertifikats ab. Sie wird in den meisten Fällen automatisch von einem Roboter durchgeführt und dauert zwischen 5 Minuten und 8 Stunden. Bei teuren Zertifikaten, bei denen auch die physischen Eigentumsverhältnisse des Standorts und der Betreibergesellschaft überprüft werden, erfolgt die Überprüfung manuell und kann mehrere Tage dauern.

Wenn Sie es eilig haben, gibt es eine Zertifizierungsstelle `Let's encrypt`, die Anträge automatisch mit einer Gültigkeit von 3 Monaten genehmigt.

> **Warnung:** In einigen Fällen bietet die CA eine automatische Anfrageerstellung an. Dies ist nicht empfehlenswert, da es den privaten Schlüssel kennt. In diesem Fall müssen Sie sich den privaten Schlüssel immer von der Agentur besorgen und ihn in einer Datei auf dem Server ablegen, so als ob Sie den Antrag erstellen würden.

## Abrufen eines Schlüssels für eine bestimmte CA/Sicherheitsbehörde

In der Zwischenzeit, während wir darauf warten, dass der Antrag genehmigt und das Zertifikat ausgestellt wird, sichern wir den **öffentlichen Schlüssel** der CA. In Apache VirtualHost wird dies durch die Datei `SSLCertificateChainFile` repräsentiert (auf Englisch heißt dies `Intermediate and Root CA`). Dieser Schlüssel ist im Prinzip öffentlich und teilt dem Webbrowser mit, wer die CA ist. Diese Datei wird in der Regel von der Website der zuständigen Behörde heruntergeladen oder uns per E-Mail zugestellt.

Sie sollten immer den richtigen Schlüssel für den von Ihnen gewählten Algorithmus herunterladen. RapidSSL kann zum Beispiel hier heruntergeladen werden: https://knowledge.digicert.com/generalinformation/INFO1548.html

## Abrufen des Zertifikats

Auf der Grundlage des Antrags hat uns die Zertifizierungsstelle ein Zertifikat ausgestellt. Im Fall von Apache VirtualHost wird sie durch die Datei "SSLCertificateFile" dargestellt.

Wichtig ist, dass das Zertifikat eine begrenzte Gültigkeit hat und wir diesen Vorgang wiederholen müssen (idealerweise vor Ablauf). Sie können das Ablaufdatum in Chrome leicht herausfinden, indem Sie auf das grüne Vorhängeschloss im Browser klicken und sich die Zertifikatsdetails ansehen, falls es von der Zertifizierungsstelle mitgeteilt wird.

Nach dem Ablaufdatum ist das Zertifikat ungültig, und die Website verweigert die Verbindung, und die Benutzer erhalten eine Fehlermeldung über die Sicherheitsverletzung.

## Prüfen und Änderungen vornehmen

Die Korrektheit der Apache-Einstellungen kann teilweise mit dem Befehl `apache2ctl -S` überprüft werden.

Damit die Änderungen wirksam werden, muss der Apache neu gestartet werden, zum Beispiel mit dem Befehl:

```shell
sudo service apache2 restart
```

Wenn keine Fehlermeldung ausgegeben wird, überprüfen wir sofort die Funktionalität über einen Webbrowser (ich empfehle Google Chrome, der die meisten Details anzeigt).

Im Falle eines Fehlers rufen wir den Befehl `apache2ctl -S` auf, der den Pfad zu den Logs anzeigen kann, wo wir ungefähr sehen können, was falsch ist. Es ist wichtig, dass Sie alle Schritte in dieser Anleitung in der richtigen Reihenfolge ausführen und keine der Tasten vertauschen.

## Künftige Unterstützung

Vergessen Sie nicht, das Ablaufdatum in Ihrem Kalender zu markieren, damit Sie Ihr Zertifikat rechtzeitig erneuern können. Einige Zertifizierungsstellen senden eine Benachrichtigung per E-Mail, doch ist dies nicht immer zuverlässig.

Sie können den Erneuerungsprozess durchführen und parallel dazu ein neues Zertifikat erhalten, während das aktuelle noch läuft, und es dann einfach gleichzeitig ersetzen.

Für die automatische Erneuerung empfehle ich [Certbot] (https://certbot.eff.org), das die Gültigkeit von Zertifikaten automatisch überwachen und erneuern kann. Die Erneuerung erfolgt z. B. durch einen Cron, der einmal im Monat nachts einen Befehl zur Ausstellung eines neuen Zertifikats aufruft und dieses sofort ausliefert.

Wenn Sie sich mit der Verwaltung von Zertifikaten nicht auskennen, ist es eine gute Idee, bei einem etablierten Unternehmen zu hosten, das Ihnen ein funktionales Hosting einschließlich eines Zertifikats zur Verfügung stellt. Das erspart Ihnen eine Menge Ärger.
