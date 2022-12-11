Abrufen von HTTP-Anfrage-Informationen über cURL
================================================

> id: '310e5143-6b38-44c1-a1bf-de37f94ee17d'
> slug:
> 	- null
> 	de: abrufen-von-http-anfrage-informationen-ueber-curl
> 
> cs: curl-getinfo
> publicationDate: '2022-07-06 19:50:00'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '49cb69f5cf2b844cb780f77c1e7d42d5'

Die PHP-Funktion `curl_getinfo()` liefert detaillierte Informationen über die ausgeführte cURL-Anfrage. In diesem Artikel wird die Bedeutung der einzelnen Felder erläutert.

Beispiel für die Verwendung
---------------

Rufen Sie die Funktion über das Ergebnis des Kontexts von `curl_init()` auf:

```php
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, 'https://baraja.cz');
curl_setopt($ch, CURLOPT_HEADER, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 0);
curl_setopt($ch, CURLOPT_NOBODY, 1);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 0);
curl_exec($ch);
$info = curl_getinfo($ch);
curl_close($ch);

dump($info);
```

Tabelle der Werte
--------------

Die Funktion `curl_getinfo()` gibt ein assoziatives Array zurück, aus dem einzelne Schlüssel und Werte abgerufen werden können.

| Schlüssel | Beispielwert | Erläuterung |
|---------------------------|----------------------------|------------------------------------------------------------------------------------|
| "URL" | "https://baraja.cz/" | Heruntergeladene URL.
| `content_type` | 'text/html; charset=utf-8' | Verwendete Kodierung und Inhaltstyp (vom Zielserver gefordert).
| `http_code` | 200 | HTTP-Statuscode zurück. 200 bedeutet OK.
| "header_size" | 462 | HTTP-Anfrage-Header-Größe in Bytes.
| `request_size` | 47 | Anfragegröße. |
| | `Filetime` | -1 | Dateizeit (Serveransprüche).
| `ssl_verify_result` | 0 | SSL-Prüfung. |
| | `redirect_count` | 0 | Anzahl der Weiterleitungen, bevor das Zieldokument erreicht wird.
| `total_time` | 0.233384 | Gesamtdauer des Wartens auf Antwort. Angabe in Sekunden. |
| `namelookup_time` | 0.021608 | Zeit für die Auflösung der Domäne über DNS-Einträge. Angegeben in Sekunden. |
| | `connect_time` | 0.035031 | Zeit für den Aufbau einer Verbindung zum Zielserver. Angegeben in Sekunden. |
| | `pretransfer_time` | 0.187275 | Für die Datenübertragung benötigte Zeit. Angegeben in Sekunden. |
| `upload_size` | 0.0 | Größe der hochgeladenen Daten in Bytes.
| `size_download` | 0.0 | Heruntergeladene Datengröße in Bytes.
| `speed_download` | 0.0 | Downloadgeschwindigkeit in Bytes pro Sekunde.
| `speed_upload` | 0.0 | Upload-Geschwindigkeit in Bytes pro Sekunde.
| `download_content_length` | 15522.0 | Heruntergeladene Datengröße in Bytes.
| `upload_content_length` | -1.0 | Größe der hochgeladenen Daten in Bytes.
| "starttransfer_time" | 0.233354 | Gibt den TTFB-Wert (Time To First Byte) in Sekunden an.
| `redirect_time` | 0.0 | Zeit für die Umleitung zum Herunterladen kanonischer Inhalte.
| | `redirect_url` | `''` | kanonische URL und Umleitungsziel. |
| `primary_ip` | '76.76.21.21' | Von welcher IP der Inhalt heruntergeladen wurde. |
| `certinfo` | array (0) | Weitere Details über das Zertifikat der Zielsite.
| 443 | Der verwendete Netzwerk-Port (80 bedeutet HTTP, 443 bedeutet HTTPS).
| `local_ip` | '192.168.0.186' | Lokale IP-Adresse des Rechners, der die Anfrage gesendet hat.
| `local_port` | 56568 | Port des lokalen Rechners, von dem die Anfrage gesendet wurde.
| `http_version` | 3 | HTTP-Protokollversion. |
| | "Protokoll" | 2 | Code des verwendeten Protokolls
| `ssl_verifyresult` | 0 | SSL-Verifizierungsergebnis. |
| "Schema" | "HTTPS" | Protokoll am Anfang der URL.
| `appconnect_time_us` | 186220 | Zeit für den Verbindungsaufbau mit dem Zielserver. Angegeben in Mikrosekunden. |
| | `connect_time_us` | 35031 | Zeit bis zur Verbindung mit dem Zielserver. Angegeben in Mikrosekunden.
| | `namelookup_time_us` | 21608 | Zeit, die benötigt wird, um die Domäne über DNS-Einträge umzuschreiben. Angegeben in Mikrosekunden. |
| | `pretransfer_time_us` | 187275 | Zeit für die Datenübertragung. Angegeben in Mikrosekunden. |
| `redirect_time_us` | 0 | Zeit für die Umleitung zum Herunterladen kanonischer Inhalte. Angabe in Mikrosekunden. |
| `starttransfer_time_us` | 233354 | Gibt den Wert der TTFB-Zeit (Time To First Byte) an. In Mikrosekunden. |
| | `total_time_us` | 233384 | Gesamtdauer des Wartens auf eine Antwort. Angegeben in Mikrosekunden. |

Einige Tasten sind möglicherweise nicht immer verfügbar. Überprüfen Sie immer das Vorhandensein des Schlüssels und die Gültigkeit des Wertes, bevor Sie den Wert lesen.
