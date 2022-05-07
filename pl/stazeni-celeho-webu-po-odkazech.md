Pobieranie całej witryny za pomocą łączy w PHP
==============================================

> id: dd4abb8e-8f9b-4867-b98f-ff1c859d387a
> slug:
> 	cs: stazeni-celeho-webu-po-odkazech
> 	pl: pobieranie-calej-witryny-za-pomoca-laczy-w-php
> 
> publicationDate: '2019-11-06 17:41:30'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: e70451decff5c7bdf19bca8181c0dd5e

Stosunkowo często rozwiązuję zadanie pobrania wszystkich stron w obrębie jednej witryny lub domeny, ponieważ następnie wykonuję różne pomiary z wykorzystaniem wyników lub wykorzystuję strony do wyszukiwania pełnotekstowego.

Jednym z możliwych rozwiązań jest użycie gotowego narzędzia [Xenu](http://home.snafu.de/tilman/xenulink.html), które jest bardzo trudne do zainstalowania na serwerze WWW (jest to program dla systemu Windows), lub [Wget](https://www.gnu.org/software/wget/), które nie wszędzie jest obsługiwane i tworzy kolejną niepotrzebną zależność.

Jeśli zadanie polega tylko na wykonaniu kopii strony do późniejszego przeglądania, bardzo przydatny jest program [HTTrack](https://www.httrack.com/), który lubię najbardziej, tylko że przy indeksowaniu sparametryzowanych adresów URL w niektórych przypadkach możemy stracić dokładność.

Zacząłem więc szukać narzędzia, które potrafi automatycznie indeksować wszystkie strony bezpośrednio w PHP z możliwością zaawansowanej konfiguracji. Ostatecznie stał się on projektem typu open source.

Baraja WebCrawler
-----------------

Właśnie dla takich potrzeb zaimplementowałem własny pakiet Composer [WebCrawler](https://github.com/baraja-core/webcrawler), który sam potrafi elegancko obsłużyć proces indeksowania stron, a jeśli natrafię na nowy przypadek, jeszcze go udoskonalam.

Instaluje się go za pomocą polecenia Composer:

```shell
composer require baraja-core/webcrawler
```

I jest łatwy w użyciu. Wystarczy utworzyć instancję i wywołać metodę `crawl()`:

```php
$crawler = new \Baraja\WebCrawler\Crawler;

$result = $crawler->crawl('https://example.com');
```

W zmiennej `$result` kompletny wynik będzie dostępny jako instancja encji `CrawledResult`, którą polecam przestudiować, ponieważ zawiera ona wiele ciekawych informacji o całej witrynie.

Ustawienia pełzacza
------------------

Często musimy w jakiś sposób ograniczyć pobieranie stron, ponieważ w przeciwnym razie prawdopodobnie pobralibyśmy cały Internet.

Służy do tego encja `Config`, której przekazywana jest konfiguracja w postaci tablicy (klucz-wartość), a następnie przekazywana do crawlera przez konstruktor.

Na przykład:

```php
$crawler = new \Baraja\WebCrawler\Crawler(
    new \Baraja\WebCrawler\Config([
        // klucz => wartość
    ])
);
```

Opcje ustawień:

| Klucz | Wartość domyślna | Możliwe wartości |
|-------------------------|---------------|-----------------|
| `followExternalLinks` | `false` | `Bool`: Pozostać tylko w tej samej domenie? Czy może indeksować również linki zewnętrzne? | Czy może indeksować linki zewnętrzne?
| `sleepBetweenRequests` | `1000` | `Int`: Czas oczekiwania pomiędzy pobraniem każdej strony w milisekundach. |
| `maxHttpRequests` | `1000000` | `Int`: Ile maksymalnie pobranych adresów URL?
| `maxCrawlTimeInSeconds` | `30` | `Int`: Jak długo może trwać maksymalne pobieranie w sekundach?
| `allowedUrls` | `['.+']` | `String[]`: Tablica dozwolonych formatów URL jako wyrażeń regularnych. |
| `forbiddenUrls` | `['']` | `String[]`: Tablica zakazanych formatów URL jako wyrażeń regularnych. |

Wyrażenie regularne musi pasować do całego adresu URL dokładnie w postaci ciągu znaków.
