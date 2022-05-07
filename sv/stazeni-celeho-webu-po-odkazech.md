Hämta hela webbplatsen via länkar i PHP
=======================================

> id: dd4abb8e-8f9b-4867-b98f-ff1c859d387a
> slug:
> 	cs: stazeni-celeho-webu-po-odkazech
> 	sv: haemta-hela-webbplatsen-via-laenkar-i-php
> 
> publicationDate: '2019-11-06 17:41:30'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: e70451decff5c7bdf19bca8181c0dd5e

Relativt ofta löser jag uppgiften att ladda ner alla sidor på en webbplats eller domän, eftersom jag sedan utför olika mätningar med resultaten eller använder sidorna för fulltextsökning.

En möjlig lösning är att använda det färdiga verktyget [Xenu](http://home.snafu.de/tilman/xenulink.html), som är mycket svårt att installera på en webbserver (det är ett Windows-program), eller [Wget](https://www.gnu.org/software/wget/), som inte stöds överallt och skapar ytterligare ett onödigt beroende.

Om uppgiften bara är att göra en kopia av sidan för senare visning är programmet [HTTrack](https://www.httrack.com/) mycket användbart, som jag gillar mest, men när vi anger parametrerade URL:er kan vi i vissa fall förlora i noggrannhet.

Därför började jag leta efter ett verktyg som kan indexera alla sidor automatiskt direkt i PHP med avancerad konfiguration. Så småningom blev detta ett projekt med öppen källkod.

Baraja WebCrawler
-----------------

För just dessa behov har jag implementerat mitt eget Composer-paket [WebCrawler] (https://github.com/baraja-core/webcrawler), som kan hantera processen att indexera sidor på ett elegant sätt på egen hand, och om jag stöter på ett nytt fall förbättrar jag det ytterligare.

Den installeras med kommandot Composer:

```shell
composer require baraja-core/webcrawler
```

Och det är lätt att använda. Skapa bara en instans och anropa metoden `crawl()`:

```php
$crawler = new \Baraja\WebCrawler\Crawler;

$result = $crawler->crawl('https://example.com');
```

I variabeln `$result` kommer det fullständiga resultatet att finnas tillgängligt som en instans av entiteten `CrawledResult`, som jag rekommenderar att du studerar eftersom den innehåller mycket intressant information om hela webbplatsen.

Inställningar för krypande datorer
------------------

Ofta måste vi begränsa nedladdningen av sidor på något sätt, för annars skulle vi förmodligen ladda ner hela internet.

Detta görs med hjälp av entiteten `Config`, som skickas till konfigurationen som en array (nyckel-värde) och som sedan skickas till crawlern av konstruktören.

Till exempel:

```php
$crawler = new \Baraja\WebCrawler\Crawler(
    new \Baraja\WebCrawler\Config([
        // nyckel => värde
    ])
);
```

Inställningsalternativ:

| Nyckel | Standardvärde | Möjliga värden |
|-------------------------|---------------|-----------------|
| `followExternalLinks` | `false` | `Bool`: Stanna bara inom samma domän? Kan den också indexera externa länkar? |
| `sleepBetweenRequests` | `1000` | `Int`: Väntetid mellan nedladdningen av varje sida i millisekunder. |
| `maxHttpRequests` | `1000000` | `Int`: Hur många webbadresser kan maximalt laddas ner? |
| `maxCrawlTimeInSeconds` | `30` | `Int`: Hur lång tid tar den maximala nedladdningen i sekunder?
| `allowedUrls` | `['.+']` | `String[]`: Array av tillåtna URL-format som reguljära uttryck. |
| `forbiddenUrls` | `[''']` | `String[]`: Array av förbjudna URL-format som reguljära uttryck. |

Det reguljära uttrycket måste matcha hela webbadressen exakt som en sträng.
