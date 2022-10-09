Download af hele webstedet via links i PHP
==========================================

> id: dd4abb8e-8f9b-4867-b98f-ff1c859d387a
> slug:
> 	cs: stazeni-celeho-webu-po-odkazech
> 	da: download-af-hele-webstedet-via-links-i-php
> 
> publicationDate: '2019-11-06 17:41:30'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: e70451decff5c7bdf19bca8181c0dd5e

Relativt ofte løser jeg opgaven med at downloade alle siderne på et websted eller domæne, fordi jeg derefter udfører forskellige målinger med resultaterne, eller jeg bruger siderne til fuldtekstsøgning.

En mulig løsning er at bruge det færdige værktøj [Xenu](http://home.snafu.de/tilman/xenulink.html), som er meget svært at installere på en webserver (det er et Windows-program), eller [Wget](https://www.gnu.org/software/wget/), som ikke understøttes alle steder og skaber endnu en unødvendig afhængighed.

Hvis opgaven blot er at lave en kopi af siden til senere visning, er programmet [HTTrack](https://www.httrack.com/) meget nyttigt, som jeg kan lide mest, men når vi indtaster parametrerede URL'er, kan vi i nogle tilfælde miste nøjagtigheden.

Så jeg begyndte at lede efter et værktøj, der kan indeksere alle sider automatisk direkte i PHP med avanceret konfiguration. Til sidst blev det til et opensource-projekt.

Baraja WebCrawler
-----------------

Netop til disse behov har jeg implementeret min egen Composer-pakke [WebCrawler] (https://github.com/baraja-core/webcrawler), som kan håndtere processen med at indeksere sider elegant på egen hånd, og hvis jeg støder på en ny sag, forbedrer jeg den yderligere.

Det installeres med kommandoen Composer:

```shell
composer require baraja-core/webcrawler
```

Og det er nemt at bruge. Du skal blot oprette en instans og kalde metoden `crawl()`:

```php
$crawler = new \Baraja\WebCrawler\Crawler;

$result = $crawler->crawl('https://example.com');
```

I variablen `$result` vil det komplette resultat være tilgængeligt som en instans af entiteten `CrawledResult`, som jeg anbefaler at studere, fordi den indeholder en masse interessante oplysninger om hele webstedet.

Indstillinger for crawler
------------------

Ofte er vi nødt til at begrænse downloading af sider på en eller anden måde, for ellers ville vi sandsynligvis downloade hele internettet.

Dette gøres ved hjælp af entiteten `Config`, som overføres konfigurationen som et array (nøgle-værdi) og derefter overføres til crawleren af konstruktøren.

For eksempel:

```php
$crawler = new \Baraja\WebCrawler\Crawler(
    new \Baraja\WebCrawler\Config([
        // nøgle => værdi
    ])
);
```

Indstillingsmuligheder:

| Nøgle | Standardværdi | Mulige værdier |
|-------------------------|---------------|-----------------|
| `followExternalLinks` | `false` | `Bool`: Bliver du kun inden for samme domæne? Kan den også indeksere eksterne links? |
| `sleepBetweenRequests` | `1000` | `Int`: Ventetid mellem downloading af hver side i millisekunder. |
| `maxHttpRequests` | `1000000` | `Int`: Hvor mange URL'er må der maksimalt downloades? |
| `maxCrawlTimeInSeconds` | `30` | `Int`: Hvor lang tid må der maksimalt downloades i sekunder?
| `allowedUrls` | `['.+']` | `String[]`: Array af tilladte URL-formater som regulære udtryk. |
| `forbiddenUrls` | `[''']` | `String[]`: Array af forbudte URL-formater som regulære udtryk. |

Det regulære udtryk skal matche hele URL-adressen nøjagtigt som en streng.
