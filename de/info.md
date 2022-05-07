PHP- und Serverkonfigurationsinformationen (phpinfo(), php.ini)
===============================================================

> id: '6f4a78c1-ca06-4619-9a28-a716a65a12a8'
> slug:
> 	cs: info
> 	de: php--und-serverkonfigurationsinformationen-phpinfo-php.ini
> 
> perex:
> 	- 'PHP info, informace o nastavení a konfiguraci webového serveru. Změna konfigurace přes php.ini'
> 	- 'PHP-Info, Informationen über die Einrichtung und Konfiguration des Webservers. Konfiguration über php.ini ändern'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '9ad0961571b298fb1f30ea3e67b2973e'

Wir müssen oft so viele Informationen über den Server wie möglich herausfinden, die native Funktion `phpinfo()` ist dafür hervorragend geeignet:

```php
phpinfo();

die;	// nach dem Schreiben der Konfiguration das Skript beenden
```

Dies macht es einfach, die installierte Version, Erweiterungen, Bibliotheken und vieles mehr zu sehen.

> Informationen zum Konfigurieren und Ändern von Einstellungen finden Sie am Ende dieses Artikels.

Suche nach einem bestimmten Konfigurationsabschnitt
-------------------------------------

Manchmal ist es sinnvoll, nur bestimmte Informationen aufzulisten. Daher können wir den ersten Parameter so einstellen, dass er genau angibt, woran wir interessiert sind:

```php
phpinfo(INFO_MODULES);
```

Für die Einstellung werden vordefinierte Konstanten verwendet:

| Konstante Name | Wert | Beschreibung
|-------------------|-----------|------
| INFO_GENERAL | 1 | Allgemeine Konfiguration, Speicherort der php.ini, Datum der letzten Aktualisierung, Webserver, Systeminformationen und mehr.
| INFO_CREDITS | 2 | PHP-Credits, für mehr siehe `phpcredits()`.
| INFO_CONFIGURATION| 4 | Aktuelle Standort- und Einstellungsrichtlinien. Weitere Informationen werden von der Funktion `ini_get()` bereitgestellt.
| INFO_MODULES | 8 | Informationen über installierte Module. Für weitere Informationen siehe die Funktion `get_loaded_extensions()`.
| INFO_ENVIRONMENT | 16 | Informationen über die Variable `Environment`, verfügbar als `$_ENV`.
| INFO_VARIABLES | 32 | Eine Übersicht über die Einstellungen der superglobalen Variablen, bekannt als `EGPCS` (Environment, GET, POST, Cookie, Server).
| INFO_LICENSE | 64 | Informationen über die PHP-Nutzungslizenz und andere Nutzungsbestimmungen.
| INFO_ALL | -1 | Alle Informationen anzeigen (Standardwert)

Superglobale Variable `$_SERVER`
---------------------------------

Wir können eine ganze Reihe von Informationen über die Servereinstellungen direkt während der Ausführung des Skripts herausfinden (z.B. die E-Mail an den Webmaster, die aktuelle IP-Adresse des Besuchers oder die aktuell aufgerufene URL).

Die Auflistung aller vorhandenen Werte ist einfach:

```php
foreach ($_SERVER as $key => $value) {
    echo $key . ':' . $value . '<br>';
}
```

> **Warnung:** Es müssen nicht alle Indizes vorhanden sein (wenn das Skript z. B. cron im CLI-Modus ausführt, wird der Index mit der Seiten-URL oder der IP-Adresse der Anfrage nicht vorhanden sein).

Lesen spezifischer Konfigurationsanweisungen
-----------------------------------------

Ein Großteil der Konfiguration wird in der Datei `php.ini` gespeichert und ist nicht direkt von PHP aus zugänglich. Zum Beispiel die maximale Dateigröße beim Hochladen.

Um die Konfiguration direkt zu lesen, verwenden Sie die Funktion `ini_get()` (Hinweis: Diese Funktion ist möglicherweise nicht auf allen Servern aktiviert, dies gilt insbesondere für Hosts).

Wenn wir zum Beispiel die maximale Dateigröße herausfinden wollen, die wir hochladen können, müssen wir unsere eigene Implementierung schreiben:

```php
/**
 * @Autor Jan Barášek
 */
public static function getMaxUploadFileSize(): int
{
    $maxUpload = min(
        ini_get('post_max_size'),
        ini_get('upload_max_filesize')
    );

    if (strncmp($maxUpload, 'M', 1) === 0) {
        return (int) str_replace('M', '', $maxUpload);
    }

    return (int) $maxUpload;
}
```

Gibt den maximalen Wert von `upload_max_filesize` und `post_max_size` in MB zurück.

Konfigurieren des Servers und Ändern von Einstellungen
-------------------------------------

Die Einstellungen selbst werden in der Datei `php.ini` gespeichert. Sein Standort kann leicht mit der Funktion `phpinfo()` oder durch Aufruf des Befehls `php --ini` ermittelt werden.

```shell
> php --ini

Configuration File (php.ini) Path: /etc/php/7.1/cli
Loaded Configuration File:         /etc/php/7.1/cli/php.ini
Scan for additional .ini files in: /etc/php/7.1/cli/conf.d
Additional .ini files parsed:      /etc/php/7.1/cli/conf.d/10-mysqlnd.ini,
/etc/php/7.1/cli/conf.d/10-opcache.ini,
/etc/php/7.1/cli/conf.d/10-pdo.ini,
/etc/php/7.1/cli/conf.d/20-calendar.ini,
/etc/php/7.1/cli/conf.d/20-ctype.ini,
/etc/php/7.1/cli/conf.d/20-exif.ini,
/etc/php/7.1/cli/conf.d/20-fileinfo.ini,
/etc/php/7.1/cli/conf.d/20-ftp.ini,
/etc/php/7.1/cli/conf.d/20-gd.ini,
/etc/php/7.1/cli/conf.d/20-gettext.ini
```

Alternativ kann der Pfad auch geschickt geparst werden (funktioniert auf Linux-Systemen):

```shell
php -r "phpinfo();" | grep php.ini
```

Er wird zurückkommen:

```shell
Configuration File (php.ini) Path => /etc/php/7.1/cli
Loaded Configuration File => /etc/php/7.1/cli/php.ini
```

> **Wichtig:** Normalerweise ist die Konfiguration in mehrere Dateien je nach Umgebung und Paketen aufgeteilt, wobei `php.ini` global für alle gilt, während z.B. die `CLI`-Konfiguration nur für den CLI-Modus gültig ist, d.h. für den Aufruf eines Cron oder eines Befehls aus dem Terminal.

Konfigurieren der Größenbeschränkung für Upload-Dateien
----------------------------------------------

Ein Beispiel für eine Eigenschaft, die oft direkt in der `php.ini` konfiguriert wird, ist die maximale Dateigröße für den Upload (die Standardeinstellung ist `2 MB`, was im Jahr 2018 schon sehr niedrig ist).

In der Konfigurationsdatei wird dies z.B. wie folgt geschrieben:

```shell
; Maximum allowed size for uploaded files.
upload_max_filesize = 40M

; Must be greater than or equal to upload_max_filesize
post_max_size = 40M
```

*Mittelpunkt bedeutet Kommentar, gefolgt von spezifischen Konfigurationsanweisungen.
