Abfrage der IP-Adresse des Benutzers in PHP
===========================================

> id: '1d6d761e-c139-4624-8c32-b0f2c131a831'
> slug:
> 	cs: ip-adresa
> 	de: abfrage-der-ip-adresse-des-benutzers-in-php
> 
> perex:
> 	- 'Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem.'
> 	- 'Ermitteln der IP-Adresse des Benutzers in PHP, Speichern der IP-Adresse und Sperren des Benutzers. Untersuchung von VPN und Benutzer hinter Proxy oder NAT.'
> 
> publicationDate: '2020-02-28 10:30:21'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '284c4642c3fa98a026ce5a9e6625bb16'

In PHP ist es sehr einfach, eine IP-Adresse auf einer grundlegenden Ebene zu erkennen:

```php
echo 'Wissen Sie, Ihre IP-Adresse ist' . $_SERVER['REMOTE_ADDR'] . '?';
```

> **Warnung:** Die IP-Adresse als Schlüssel des Feldes `$_SERVER['REMOTE_ADDR']` zu erhalten ist nur möglich, wenn PHP vom Browser aus aufgerufen wurde. Im CLI-Modus (z. B. bei der Ausführung von Terminal mit cron) ist die IP-Adresse nicht verfügbar (was sinnvoll ist, da keine Netzwerkanforderung erfolgt).

Zuverlässige IP-Adressermittlung
-----------------------------

Nach vielen Jahren der Entwicklung bin ich schließlich bei dieser Implementierung geblieben:

```php
function getIp(): string
{
    if (isset($_SERVER['HTTP_CF_VERBINDUNGS_IP'])) { // Cloudflare-Unterstützung
        $ip = $_SERVER['HTTP_CF_VERBINDUNGS_IP'];
    } elseif (isset($_SERVER['REMOTE_ADDR']) === true) {
        $ip = $_SERVER['REMOTE_ADDR'];
        if (preg_match('/^(?:127|10)\.0\.0\.[12]?\d{1,2}$/', $ip)) {
            if (isset($_SERVER['HTTP_X_REAL_IP'])) {
                $ip = $_SERVER['HTTP_X_REAL_IP'];
            } elseif (isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
                $ip = $_SERVER['HTTP_X_FORWARDED_FOR'];
            }
        }
    } else {
        $ip = '127.0.0.1';
    }
    if (in_array($ip, ['::1', '0.0.0.0', 'localhost'], true)) {
        $ip = '127.0.0.1';
    }
    $filter = filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV4);
    if ($filter === false) {
        $ip = '127.0.0.1';
    }

    return $ip;
}
```

Viel besser also:

```php
echo 'Wissen Sie, Ihre IP-Adresse ist' . getIp() . '?';
```

Wenn die IP direkt erkannt werden kann, oder nur IPv6 ist, oder im CLI-Modus ist (z.B. cron), wird `127.0.0.1` (localhost) zurückgegeben.

Implementierungen, die die Header "X-Forwarded-For" und "X-Real-IP" berücksichtigen, sind direkt in PHP äußerst gefährlich, da die Daten leicht verändert werden können und ein Angreifer eine falsche IP-Adresse fälschen kann, um beispielsweise die Verwaltung einzusehen oder den Debug-Modus der Website zu aktivieren (Nette Tracy). Andererseits müssen wir einige Proxy-Anfragen akzeptieren (z. B. beim Proxy-Verkehr über Cloudflare oder beim Betrieb von Apache und Ngnix auf demselben Rechner, wenn sie lokal direkt nacheinander aufgerufen werden).

Im Falle eines direkten Benutzerzugriffs auf den Server gibt es nur eine korrekte Lösung, nämlich bei Apache (über die `RemoteIP`-Erweiterung) und bei Nginx über die `remote_ip`-Erweiterung sicherzustellen, dass `X-Forwarded-For` von der tatsächlichen IP-Adresse des Besuchers gesetzt wird und dass die IP-Adresse nicht mit einem HTTP-Header gesetzt werden kann.

Das Feld `$_SERVER['REMOTE_ADDR']` erhält automatisch die korrekte IP-Adresse (d.h. die IP-Adresse, von der die Anfrage direkt an PHP gesendet wurde) und wir müssen uns nicht darum kümmern.

Benutzerzugang über Proxy
----------------------------

Es kommt häufig vor, dass ein Benutzer über einen Proxy zugreift. Dann wird die tatsächliche IP-Adresse in der Variablen `$_SERVER['HTTP_X_FORWARDED_FOR']` gespeichert.

Dieser Fall kann beispielsweise eintreten, wenn das Routing auf dem Server mit der Methode "Ngnix -> Apache -> PHP" gelöst wird, wobei "Ngnix" als Reverse Proxy vor "Apache" dient. In diesem Fall sieht PHP nur die IP-Adresse innerhalb des internen Netzwerks (normalerweise in der Form `127.0.0.*`).

Beispielsweise kann sich der Dienst **Cloudflare** auf diese Weise verhalten, und es sollte darauf geachtet werden, ob wir mit der IP-Adresse des tatsächlichen Benutzers oder des Proxys arbeiten. Für mich ist es am besten, die am Anfang dieses Artikels erwähnte Funktion "getIp()" zu verwenden. Wir können die Erkennung durch Cloudflare sicherstellen, indem wir das Vorhandensein des `$_SERVER['HTTP_CF_CONNECTING_IP']`-Schlüssels überprüfen, der bei jeder Proxy-Anfrage automatisch übermittelt wird.

VPN-/Proxy-Erkennung
-------------------

Es gibt keine zuverlässige Erkennung von Proxy- oder VPN-Nutzung, aber in einer realen Umgebung können wir zumindest einen Teil des Datenverkehrs herausfiltern.

Es gibt mehrere Möglichkeiten, dies zu tun: Nehmen Sie einen Bereich von IP-Adressen und vergleichen Sie die IP-Adresse, von der die Anfrage kam.

Bei einigen VPN-Anbietern sind Listen von IP-Adressen inoffiziell verfügbar (siehe z.B. https://gist.github.com/JamoCA/eedaf4f7cce1cb0aeb5c1039af35f0b7), bei Tor-Exit-Nodes offiziell (https://blog.torproject.org/changes-tor-exit-list-service, aber Tor-Bridges gibt es nicht).

Eine andere Möglichkeit besteht darin, irgendwo eine Online-Anfrage zu stellen, was sowohl das Laden der Seite verzögern kann, wenn der Dienst nicht funktioniert, als auch die IP-Adressen der Besucher an Dritte weitergeben kann. Ab 2023 würde ich dringend von diesem Ansatz abraten, da es dann mehr um den Schutz und die Manipulation von Nutzerdaten geht.

Diese Online-Abfrage kann "naiv" sein und muss nur feststellen, wem der Bereich gehört oder ob es sich um einen Proxy/VPN handelt (einige Dienste können dies zurückgeben, aber standardmäßig ist es nicht Teil der "IP-Informationen", z. B. von einem Whois-Dienst).

(Meistens) wird eine Art von Reputationsbewertung verwendet, bei der einige IP-Adressen mehr "Müll" produzieren als andere. Statistisch gesehen kommt mehr Mist von verschiedenen Proxys, VPNs und Tor als von privaten IP-Adressen (außer vielleicht von "infizierten" privaten IP-Adressen). Eine solche Reputationsbewertung wird von einigen DNS-Blockierlisten angeboten, siehe einige zufällige Listen, https://en.m.wikipedia.org/wiki/Comparison_of_DNS_blacklists und die Spalte "Listing-Ziel", oder sie wird direkt von Unternehmen wie Cloudflare in Form von "Bot-Management" usw. angeboten.

Vieles hängt davon ab, was das Ziel der Erkennung ist.

Speicherung von IP-Adressen
------------------

Das hängt davon ab, welche IP-Adresse Sie zur Verfügung haben.

- Die IPv4-IP-Adresse kann in 4 Bytes gespeichert werden, dazu wird die Funktion `ip2long` verwendet,
- Für eine IPv6-IP-Adresse müssen wir jedoch 16 Byte verwenden, und es gibt keine Umwandlungsfunktion.

Wenn Ihr Datenbankserver nicht direkt einen Datentyp für die IP-Adresse unterstützt, empfehle ich, die IP-Adresse als `varchar(39)` zu speichern, wobei beide Versionen als Zeichenkette passen und für den Menschen lesbar sind.

> Bei der Speicherung der IP-Adresse ist zu überlegen, ob es sinnvoll ist, auch den von der Funktion "gethostbyaddr" ermittelten Domänennamen zu speichern. Bei der Auflistung und Suche kann man die Namen nicht herausfinden, weil es sehr lange dauert und sie sich im Laufe der Zeit ändern können.

Blockierung der IP-Adresse des Besuchers
-----------------------------

Die ideale Lösung besteht darin, eine Liste der gesperrten IP-Adressen zu erstellen und diese Liste bei jeder Anfrage mit der aktuellen IP-Adresse zu vergleichen. Wenn die Adressen übereinstimmen, wird die Anfrage sofort gestoppt.

```php
$blackList = [
    'Erst-IP',
    'druha-ip',
];

if (\in_array(getIp(), $blackList, true) === true) {
    echo 'Leider ist Ihre IP-Adresse gesperrt :-(';
    die; // Beenden der Anfrage
}
```

Das Beispiel setzt eine Implementierung der Funktion `getIp()` wie im obigen Beispiel voraus.

Eine leistungsfähigere Lösung besteht darin, zu prüfen, ob der Index im Array vorkommt:

```php
$blackList = [
    'Erst-IP' => true,
    'druha-ip' => true,
];

if (isset($blackList[getIp()]) === true) {
    echo 'Leider ist Ihre IP-Adresse gesperrt :-(';
    die; // Beenden der Anfrage
}
```

Server-IP-Adresse und Servername
---------------------------------

Die IP-Adresse des Servers wird normalerweise im Feld `$_SERVER['SERVER_ADDR']` gespeichert und sein Name kann durch das Konstrukt `gethostbyaddr($_SERVER['SERVER_ADDR'])` ermittelt werden.

Wenn jedoch das Konzept "Ngnix -> Apache -> PHP" verwendet wird und "Ngnix" die Rolle eines Reverse Proxy einnimmt, wird die tatsächliche IP-Adresse des Servers nicht angezeigt.

In diesem Fall kann der Name des Servers im Feld `$_SERVER['SERVER_NAME']` oder mit der Funktion `php_uname('n')` ermittelt werden. [Offizielle Dokumentation der uname-Funktion](https://www.php.net/manual/en/function.php-uname.php).

Mit diesem Trick können wir dann die öffentliche IP-Adresse des Servers herausfinden: `gethostbyname(php_uname('n'))`.
