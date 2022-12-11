Få information om HTTP-förfrågningar via cURL
=============================================

> id: '310e5143-6b38-44c1-a1bf-de37f94ee17d'
> slug:
> 	- null
> 	sv: fa-information-om-http-foerfragningar-via-curl
> 
> cs: curl-getinfo
> publicationDate: '2022-07-06 19:50:00'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '49cb69f5cf2b844cb780f77c1e7d42d5'

PHP-funktionen `curl_getinfo()` ger detaljerad information om den utförda cURL-förfrågan. I den här artikeln förklaras innebörden av varje fält.

Exempel på användning
---------------

Kalla funktionen över resultatet av kontexten från `curl_init()`:

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

Värdetabell
--------------

Funktionen `curl_getinfo()` returnerar en associativ array från vilken enskilda nycklar och värden kan hämtas.

| Nyckel | Exempelvärde | Förklaring |
|---------------------------|----------------------------|------------------------------------------------------------------------------------|
| `url` | 'https://baraja.cz/' | Nedladdad URL. |
| `content_type` | 'text/html; charset=utf-8' | Kodning och innehållstyp som används (påstås av målservern). |
| `http_code` | 200 | HTTP-statuskod returnerad. 200 betyder OK. |
| `header_size` | 462 | Storleken på HTTP-förfrågningshuvudet i bytes. |
| `request_size` | 47 | Förfrågningsstorlek. |
| | `filetime` | -1 | Filtid (krav från servern). |
| `ssl_verify_result` | 0 | SSL-kontroll. |
| | `redirect_count` | 0 | Antal omdirigeringar innan måldokumentet nås.
| `total_time` | 0.233384 | Total tid för att vänta på svar. Anges i sekunder. |
| `namelookup_time` | 0.021608 | Tidsåtgång för att lösa upp domänen via DNS-poster. Anges i sekunder. |
| | `connect_time` | 0.035031 | Tid för att upprätta en anslutning till målservern. Anges i sekunder. |
| | `pretransfer_time` | 0.187275 | Tid som krävs för att överföra data. Anges i sekunder. |
| `upload_size` | 0.0 | Storleken på den uppladdade datan i bytes. |
| `size_download` | 0.0 | Storlek på nedladdad data i bytes. |
| `speed_download` | 0.0 | Nedladdningshastighet i bytes per sekund. |
| `speed_upload` | 0.0 | Uppladdningshastighet i bytes per sekund. |
| `download_content_length` | 15522.0 | Storlek på nedladdad data i bytes. |
| `upload_content_length` | -1.0 | Storlek på uppladdade data i bytes. |
| `starttransfer_time` | 0.233354 | Anger TTFB-värdet (Time To First Byte) i sekunder. |
| `redirect_time` | 0.0 | Tid som använts för att omdirigera för att ladda ner kanoniskt innehåll.
| | `redirect_url` | `''` | kanonisk URL och destination för omdirigering. |
| `primary_ip` | '76.76.21.21' | Från vilken IP-adress innehållet hämtades. |
| `certinfo` | array (0) | Mer information om certifikatet för målplatsen. |
| `primary_port` | 443 | Nätverksport som används (80 betyder HTTP, 443 betyder HTTPS). |
| `local_ip` | '192.168.0.186' | Lokal IP-adress för den maskin som skickade begäran. |
| `local_port` | 56568 | Port för den lokala maskinen från vilken begäran skickades. |
| `http_version` | 3 | HTTP-protokollversion. |
| | `protocol` | 2 | Kod för det protokoll som används. |
| `ssl_verifyresult` | 0 | Resultat av SSL-verifiering. |
| `scheme` | 'HTTPS' | Protokoll i början av URL:en. |
| `appconnect_time_us` | 186220 | Tid för att upprätta en anslutning till målservern. Anges i mikrosekunder. |
| | `connect_time_us` | 35031 | Tid för att ansluta till målservern. Anges i mikrosekunder. | |
| | `namelookup_time_us` | 21608 | Tid som krävs för att skriva om domänen via DNS-poster. Anges i mikrosekunder. |
| | `pretransfer_time_us` | 187275 | Tidsåtgång för överföring av data. Anges i mikrosekunder. |
| `redirect_time_us` | 0 | Tid som använts för att omdirigera för att ladda ner kanoniskt innehåll. Anges i mikrosekunder. |
| `starttransfer_time_us` | 233354 | Anger värdet av TTFB-tiden (Time To First Byte). I mikrosekunder. |
| | `total_time_us` | 233384 | Total tid som använts för att vänta på svar. Anges i mikrosekunder. |

Vissa nycklar är inte alltid tillgängliga. Kontrollera alltid att nyckeln finns och att värdet är giltigt innan du läser värdet.
