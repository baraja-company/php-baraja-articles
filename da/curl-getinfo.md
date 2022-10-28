Få oplysninger om HTTP-forespørgsler via cURL
=============================================

> id: '310e5143-6b38-44c1-a1bf-de37f94ee17d'
> slug:
> 	- null
> 	da: fa-oplysninger-om-http-foresporgsler-via-curl
>
> cs: curl-getinfo
> publicationDate: '2022-07-06 19:50:00'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '36748f0e9ced28da3da80155a0253140'

PHP-funktionen `curl_getinfo()` giver detaljerede oplysninger om den udførte cURL-forespørgsel. I denne artikel forklares betydningen af hvert enkelt felt.

Eksempel på brug
---------------

Kald funktionen over resultatet af konteksten fra `curl_init()`:

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

Tabel over værdier
--------------

Funktionen `curl_getinfo()` returnerer et associerende array, hvorfra individuelle nøgler og værdier kan hentes.

| Nøgle                     | Eksempelværdi              | Forklaring                                                                             |
|---------------------------|----------------------------|----------------------------------------------------------------------------------------|
| `url`                     | 'https://baraja.cz/'       | Downloadet URL.                                                                        |
| `content_type`            | 'text/html; charset=utf-8' | Kodning og indholdstype anvendes (hævdet af målserveren).                              |
| `http_code`               | 200                        | HTTP-statuskode returneret. 200 betyder OK.                                            |
| `header_size`             | 462                        | Størrelsen af HTTP-forespørgselshovedet i bytes.                                       |
| `request_size`            | 47                         | Anmodningsstørrelse.                                                                   |
| `filetime`                | -1                         | Filtid (krav fra serveren).                                                            |
| `ssl_verify_result`       | 0                          | SSL-kontrol.                                                                           |
| `redirect_count`          | 0                          | Antal omdirigeringer før måldokumentet nås.                                            |
| `total_time`              | 0.233384                   | Samlet tid brugt på at vente på svar. Angivet i sekunder.                              |
| `namelookup_time`         | 0.021608                   | Tid brugt på at opløse domæne over DNS-poster. Angivet i sekunder.                     |
| `connect_time`            | 0.035031                   | Tid til at oprette en forbindelse til destinationsserveren. Angivet i sekunder.        |
| `pretransfer_time`        | 0.187275                   | Den tid, der er nødvendig for at overføre data. Angivet i sekunder.                    |
| `size_upload`             | 0.0                        | Størrelsen af de uploadede data i bytes.                                               |
| `size_download`           | 0.0                        | Størrelse af downloadede data i bytes.                                                 |
| `speed_download`          | 0.0                        | Downloadhastighed i bytes pr. sekund.                                                  |
| `speed_upload`            | 0.0                        | Upload-hastighed i bytes pr. sekund.                                                   |
| `download_content_length` | 15522.0                    | Downloaded data size in bytes.                                                         |
| `upload_content_length`   | -1.0                       | Størrelsen af de uploadede data i bytes.                                               |
| `starttransfer_time`      | 0.233354                   | Angiver TTFB-værdien (Time To First Byte) i sekunder.                                  |
| `redirect_time`           | 0.0                        | Tid brugt på at omdirigere til at downloade kanonisk indhold.                          |
| `redirect_url`            | `''`                       | kanonisk URL og omdirigering af destination.                                           |
| `primary_ip`              | '76.76.21.21'              | Fra hvilken IP-adresse indholdet blev hentet.                                          |
| `certinfo`                | array (0)                  | Flere oplysninger om certifikatet for målstedet.                                       |
| `primary_port`            | 443                        | Den anvendte netværksport (80 betyder HTTP, 443 betyder HTTPS).                        |
| `local_ip`                | '192.168.0.186'            | Lokal IP-adresse på den maskine, der har sendt anmodningen.                            |
| `local_port`              | 56568                      | Port på den lokale maskine, hvorfra anmodningen blev sendt.                            |
| `http_version`            | 3                          | HTTP-protokolversion.                                                                  |
| `protocol`                | 2                          | Kode for den anvendte protokol.                                                        |
| `ssl_verifyresult`        | 0                          | SSL-verifikationsresultat.                                                             |
| `scheme`                  | 'HTTPS'                    | Protokol i begyndelsen af URL'en.                                                      |
| `appconnect_time_us`      | 186220                     | Tid til at oprette forbindelse med målserveren. Angivet i mikrosekunder.               |
| `connect_time_us`         | 35031                      | Tid til at oprette forbindelse til destinationsserveren. Angivet i mikrosekunder.      |
| `namelookup_time_us`      | 21608                      | Den tid, der kræves for at omskrive domænet via DNS-poster. Angivet i mikrosekunder.   |
| `pretransfer_time_us`     | 187275                     | Tid brugt på at overføre data. Angivet i mikrosekunder.                                |
| `redirect_time_us`        | 0                          | Tid brugt på at omdirigere til at downloade kanonisk indhold. Angivet i mikrosekunder. |
| `starttransfer_time_us`   | 233354                     | Angiver værdien af TTFB-tiden (Time To First Byte). I mikrosekunder.                   |
| `total_time_us`           | 233384                     | Samlet tid brugt på at vente på et svar. Angivet i mikrosekunder.                      |

Nogle nøgler er ikke altid tilgængelige. Kontroller altid, at nøglen findes og at værdien er gyldig, før du læser værdien.
