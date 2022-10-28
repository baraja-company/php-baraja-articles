Zjištění informací o HTTP requestu přes cURL
============================================

> id: "310e5143-6b38-44c1-a1bf-de37f94ee17d"
> slug:
>   cs: curl-getinfo
>
> publicationDate: "2022-07-06 19:50:00"
> mainCategoryId: "2a1ef8bc-14aa-438a-87e7-5b3f9643f325"

PHP funkce `curl_getinfo()` poskytuje podrobné informace o provedeném cURL requestu. Tento článek vysvětluje význam jednotlivých polí.

Příklad použití
---------------

Funkci zavoláme nad výsledkem kontextu z `curl_init()`:

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

Tabulka hodnot
--------------

Funkce `curl_getinfo()` vrací asociativní pole, z kterého lze načítat jednotlivé klíče a hodnoty.

| Klíč                      | Příklad hodnoty            | Vysvětlení                                                                         |
|---------------------------|----------------------------|------------------------------------------------------------------------------------|
| `url`                     | 'https://baraja.cz/'       | Stahovaná URL.                                                                     |
| `content_type`            | 'text/html; charset=utf-8' | Použité kódování a typ obsahu (tvrdí cílový server).                               |
| `http_code`               | 200                        | Vrácený stavový HTTP kód. 200 znamená OK.                                          |
| `header_size`             | 462                        | Velikost hlavičky HTTP requestu v bajtech.                                         |
| `request_size`            | 47                         | Velikost requestu.                                                                 |
| `filetime`                | -1                         | Čas souboru (tvrdí server).                                                        |
| `ssl_verify_result`       | 0                          | Kontrola SSL.                                                                      |
| `redirect_count`          | 0                          | Počet přesměrování, než jsme se dostali k cílovému dokumentu.                      |
| `total_time`              | 0.233384                   | Celkový čas strávený čekáním na response. Uvedeno v sekundách.                     |
| `namelookup_time`         | 0.021608                   | Čas potřebný pro přepsání domény přes DNS záznamy. Uvedeno v sekundách.            |
| `connect_time`            | 0.035031                   | Čas pro navázání spojení s cílovým serverem. Uvedeno v sekundách.                  |
| `pretransfer_time`        | 0.187275                   | Čas potřebný pro přenesení dat. Uvedeno v sekundách.                               |
| `size_upload`             | 0.0                        | Velikost odeslaných dat v bajtech.                                                 |
| `size_download`           | 0.0                        | Velikost stažených dat v bajtech.                                                  |
| `speed_download`          | 0.0                        | Rychlost stahování dat v bajtech za sekundu.                                       |
| `speed_upload`            | 0.0                        | Rychlost odesílání dat v bajtech za sekundu.                                       |
| `download_content_length` | 15522.0                    | Velikost stažených dat v bajtech.                                                  |
| `upload_content_length`   | -1.0                       | Velikost nahrávaných dat v bajtech.                                                |
| `starttransfer_time`      | 0.233354                   | Uvádí hodnotu času TTFB (Time To First Byte) v sekundách.                          |
| `redirect_time`           | 0.0                        | Čas strávený přesměrování do stažení kanonického obsahu.                           |
| `redirect_url`            | `''`                       | Kanonická URL a cíl přesměrování.                                                  |
| `primary_ip`              | '76.76.21.21'              | Z jaké IP byl stažen obsah.                                                        |
| `certinfo`                | array (0)                  | Další podrobnosti o certifikátu cílového webu.                                     |
| `primary_port`            | 443                        | Použitý síťový port (80 znamená HTTP, 443 znamená HTTPS).                          |
| `local_ip`                | '192.168.0.186'            | Lokální IP adresa stroje, který poslal request.                                    |
| `local_port`              | 56568                      | Port lokální stroje, odkud odešel request.                                         |
| `http_version`            | 3                          | Verze HTTP protokolu.                                                              |
| `protocol`                | 2                          | Kód použitého protokolu.                                                           |
| `ssl_verifyresult`        | 0                          | Výsledek kontroly SSL.                                                             |
| `scheme`                  | 'HTTPS'                    | Protokol na začátku URL.                                                           |
| `appconnect_time_us`      | 186220                     | Čas pro navázání spojení s cílovým serverem. Uvedeno v mikrosekundách.             |
| `connect_time_us`         | 35031                      | Čas pro navázání spojení s cílovým serverem. Uvedeno v mikrosekundách.             |
| `namelookup_time_us`      | 21608                      | Čas potřebný pro přepsání domény přes DNS záznamy. Uvedeno v mikrosekundách.       |
| `pretransfer_time_us`     | 187275                     | Čas potřebný pro přenesení dat. Uvedeno v mikrosekundách.                          |
| `redirect_time_us`        | 0                          | Čas strávený přesměrování do stažení kanonického obsahu. Uvedeno v mikrosekundách. |
| `starttransfer_time_us`   | 233354                     | Uvádí hodnotu času TTFB (Time To First Byte). Uvedeno v mikrosekundách.            |
| `total_time_us`           | 233384                     | Celkový čas strávený čekáním na response. Uvedeno v mikrosekundách.                |

Některé klíče nemusí být dostupné vždy. Před čtením hodnoty vždy ověřte existenci klíče a platnost získané hodnoty.
