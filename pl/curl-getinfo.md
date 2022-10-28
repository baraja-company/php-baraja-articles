Pobieranie informacji o żądaniu HTTP poprzez cURL
=================================================

> id: '310e5143-6b38-44c1-a1bf-de37f94ee17d'
> slug:
> 	- null
> 	pl: pobieranie-informacji-o-zadaniu-http-poprzez-curl
>
> cs: curl-getinfo
> publicationDate: '2022-07-06 19:50:00'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '36748f0e9ced28da3da80155a0253140'

Funkcja PHP `curl_getinfo()` dostarcza szczegółowych informacji o wykonanym żądaniu cURL. Ten artykuł wyjaśnia znaczenie każdego pola.

Przykładowe zastosowanie
---------------

Wywołaj funkcję nad wynikiem kontekstu z `curl_init()`:

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

Tabela wartości
--------------

Funkcja `curl_getinfo()` zwraca tablicę asocjacyjną, z której można pobrać poszczególne klucze i wartości.

| Klucz                     | Przykładowa wartość        | Wyjaśnienie                                                                              |
|---------------------------|----------------------------|------------------------------------------------------------------------------------------|
| `url`                     | 'https://baraja.cz/'       | Pobrany adres URL.                                                                       |
| `content_type`            | 'text/html; charset=utf-8' | Użyte kodowanie i typ zawartości (żądane przez serwer docelowy).                         |
| `http_code`               | 200                        | Kod statusu HTTP zwrócony. 200 oznacza OK.                                               |
| `header_size`             | 462                        | Rozmiar nagłówka żądania HTTP w bajtach.                                                 |
| `request_size`            | 47                         | Wielkość żądania.                                                                        |
| `filetime`                | -1                         | Czas pliku (żądania serwera).                                                            |
| `ssl_verify_result`       | 0                          | Kontrola SSL.                                                                            |
| `redirect_count`          | 0                          | Liczba przekierowań przed osiągnięciem dokumentu docelowego.                             |
| `total_time`              | 0.233384                   | Całkowity czas oczekiwania na odpowiedź. Podane w sekundach.                             |
| `namelookup_time`         | 0.021608                   | Czas potrzebny do rozwiązania domeny przez rekordy DNS. Określone w sekundach.           |
| `connect_time`            | 0.035031                   | Czas na ustanowienie połączenia z serwerem docelowym. Określone w sekundach.             |
| `pretransfer_time`        | 0.187275                   | Czas potrzebny do przesłania danych. Określone w sekundach.                              |
| `size_upload`             | 0.0                        | Rozmiar przesłanych danych w bajtach.                                                    |
| `size_download`           | 0.0                        | Wielkość pobranych danych w bajtach.                                                     |
| `speed_download`          | 0.0                        | Prędkość pobierania w bajtach na sekundę.                                                |
| `speed_upload`            | 0.0                        | Prędkość wysyłania w bajtach na sekundę.                                                 |
| `download_content_length` | 15522.0                    | Wielkość pobranych danych w bajtach.                                                     |
| `upload_content_length`   | -1.0                       | Rozmiar przesłanych danych w bajtach.                                                    |
| `starttransfer_time`      | 0.233354                   | Wskazuje wartość TTFB (Time To First Byte) w sekundach.                                  |
| `redirect_time`           | 0.0                        | Czas spędzony na przekierowaniu do pobrania kanonicznej treści.                          |
| `redirect_url`            | `''`                       | kanoniczny adres URL i miejsce docelowe przekierowania.                                  |
| `primary_ip`              | '76.76.21.21'              | Z jakiego IP została pobrana zawartość.                                                  |
| `certinfo`                | array (0)                  | Więcej szczegółów na temat certyfikatu strony docelowej.                                 |
| `primary_port`            | 443                        | Używany port sieciowy (80 oznacza HTTP, 443 oznacza HTTPS).                              |
| `local_ip`                | '192.168.0.186'            | Lokalny adres IP maszyny, która wysłała żądanie.                                         |
| `local_port`              | 56568                      | Port lokalnej maszyny, z której wysłano żądanie.                                         |
| `http_version`            | 3                          | Wersja protokołu HTTP.                                                                   |
| `protocol`                | 2                          | Kod używanego protokołu.                                                                 |
| `ssl_verifyresult`        | 0                          | Wynik weryfikacji SSL.                                                                   |
| `scheme`                  | 'HTTPS'                    | Protokół na początku adresu URL.                                                         |
| `appconnect_time_us`      | 186220                     | Czas na nawiązanie połączenia z serwerem docelowym. Określone w mikrosekundach.          |
| `connect_time_us`         | 35031                      | Czas do połączenia z serwerem docelowym. Określone w mikrosekundach.                     |
| `namelookup_time_us`      | 21608                      | Czas wymagany do przepisania domeny przez rekordy DNS. Określone w mikrosekundach.       |
| `pretransfer_time_us`     | 187275                     | Czas potrzebny do przesłania danych. Określone w mikrosekundach.                         |
| `redirect_time_us`        | 0                          | Czas spędzony na przekierowaniu do pobrania kanonicznej treści. Podane w mikrosekundach. |
| `starttransfer_time_us`   | 233354                     | Wskazuje wartość czasu TTFB (Time To First Byte). W mikrosekundach.                      |
| `total_time_us`           | 233384                     | Całkowity czas oczekiwania na odpowiedź. Określone w mikrosekundach.                     |

Niektóre klucze mogą nie zawsze być dostępne. Zawsze sprawdzaj istnienie klucza i ważność wartości przed odczytaniem wartości.
