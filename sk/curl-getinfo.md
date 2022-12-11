Získanie informácií o požiadavkách HTTP prostredníctvom cURL
============================================================

> id: '310e5143-6b38-44c1-a1bf-de37f94ee17d'
> slug:
> 	- null
> 	sk: ziskanie-informacii-o-poziadavkach-http-prostrednictvom-curl
> 
> cs: curl-getinfo
> publicationDate: '2022-07-06 19:50:00'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '49cb69f5cf2b844cb780f77c1e7d42d5'

Funkcia PHP `curl_getinfo()` poskytuje podrobné informácie o vykonanej požiadavke cURL. Tento článok vysvetľuje význam jednotlivých polí.

Príklad použitia
---------------

Volanie funkcie nad výsledkom kontextu z funkcie `curl_init()`:

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

Tabuľka hodnôt
--------------

Funkcia `curl_getinfo()` vracia asociatívne pole, z ktorého možno získať jednotlivé kľúče a hodnoty.

| Kľúč | Príklad hodnoty | Vysvetlenie |
|---------------------------|----------------------------|------------------------------------------------------------------------------------|
| `url` | 'https://baraja.cz/' | Stiahnutá adresa URL. |
| `content_type` | 'text/html; charset=utf-8' | Použité kódovanie a typ obsahu (deklarované cieľovým serverom). |
| `http_code` | 200 | Vrátený stavový kód HTTP. 200 znamená OK. |
| `header_size` | 462 | Veľkosť hlavičky požiadavky HTTP v bajtoch. |
| `request_size` | 47 | Veľkosť požiadavky. |
| | `filetime` | -1 | Čas súboru (tvrdenia servera). |
| `ssl_verify_result` | 0 | Kontrola SSL. |
| | `redirect_count` | 0 | Počet presmerovaní pred dosiahnutím cieľového dokumentu.
| `total_time` | 0.233384 | Celkový čas čakania na odpoveď. Uvedené v sekundách.
| `namelookup_time` | 0.021608 | Čas potrebný na vyriešenie domény prostredníctvom záznamov DNS. Zadané v sekundách. |
| | `connect_time` | 0.035031 | Čas na nadviazanie spojenia s cieľovým serverom. Zadané v sekundách. |
| | `pretransfer_time` | 0.187275 | Čas potrebný na prenos dát. Zadané v sekundách. |
| `upload_size` | 0.0 | Veľkosť nahraných údajov v bajtoch. |
| `size_download` | 0.0 | Veľkosť stiahnutých dát v bajtoch. |
| `speed_download` | 0.0 | Rýchlosť sťahovania v bajtoch za sekundu.
| `speed_upload` | 0.0 | Rýchlosť nahrávania v bajtoch za sekundu. |
| `download_content_length` | 15522.0 | Veľkosť stiahnutých dát v bajtoch. |
| `upload_content_length` | -1.0 | Veľkosť nahraných dát v bajtoch. |
| `starttransfer_time` | 0.233354 | Udáva hodnotu TTFB (Time To First Byte) v sekundách. |
| `redirect_time` | 0.0 | Čas strávený presmerovaním na stiahnutie kanonického obsahu.
| | `redirect_url` | `''` | kanonická adresa URL a cieľ presmerovania. |
| `primary_ip` | '76.76.21.21' | Z ktorej IP bol obsah stiahnutý. |
| `certinfo` | array (0) | Viac informácií o certifikáte cieľového miesta. |
| `primary_port` | 443 | Použitý sieťový port (80 znamená HTTP, 443 znamená HTTPS).
| `local_ip` | '192.168.0.186' | Lokálna IP adresa počítača, ktorý odoslal požiadavku. |
| `local_port` | 56568 | Port lokálneho počítača, z ktorého bola žiadosť odoslaná. |
| `http_version` | 3 | Verzia protokolu HTTP. |
| | `protokol` | 2 | Kód použitého protokolu. |
| `ssl_verifyresult` | 0 | Výsledok overenia SSL. |
| `scheme` | 'HTTPS' | Protokol na začiatku adresy URL. |
| `appconnect_time_us` | 186220 | Čas na nadviazanie spojenia s cieľovým serverom. Udáva sa v mikrosekundách. |
| | `connect_time_us` | 35031 | Čas pripojenia k cieľovému serveru. Zadáva sa v mikrosekundách. | |
| | `namelookup_time_us` | 21608 | Čas potrebný na prepísanie domény prostredníctvom záznamov DNS. Udáva sa v mikrosekundách. |
| | `pretransfer_time_us` | 187275 | Čas potrebný na prenos údajov. Udáva sa v mikrosekundách. |
| `redirect_time_us` | 0 | Čas strávený presmerovaním na stiahnutie kanonického obsahu. Uvedené v mikrosekundách. |
| `starttransfer_time_us` | 233354 | Udáva hodnotu času TTFB (Time To First Byte). V mikrosekundách.
| | `total_time_us` | 233384 | Celkový čas čakania na odpoveď. Udáva sa v mikrosekundách. |

Niektoré kľúče nemusia byť vždy k dispozícii. Pred načítaním hodnoty vždy overte existenciu kľúča a platnosť hodnoty.
