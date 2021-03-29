Stažení celého webu po odkazech v PHP
=====================================

> id: dd4abb8e-8f9b-4867-b98f-ff1c859d387a
> slugCS: stazeni-celeho-webu-po-odkazech
> publicationDate: "2019-11-06 17:41:30"
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc

Relativně často řeším úlohu stažení všech stránek v rámci jednoho webu nebo domény, protože s výsledky pak provádím různá měření, případně stránky používám pro fulltextové vyhledávání.

Jedno z možných řešení je použití již hotového nástroje [Xenu](http://home.snafu.de/tilman/xenulink.html), který se ale velmi obtížně instaluje na webovém serveru (jde o program pro Windows), případně [Wget](https://www.gnu.org/software/wget/), který zase ale není podporovaný všude a vzniká další zbytečná závislost.

Pokud je úkolem jenom provést kopii stránek pro pozdější prohlížení, velmi dobře poslouží program [HTTrack](https://www.httrack.com/), který mám z uvedených asi nejraději, jenom při indeování parametrizovaných URL můžeme v některých případech přijít o přesnost.

Začal jsem proto hledat nástroj, který bude umět zaindexovat všechny stránky automaticky přímo v PHP s možností pokročilé konfigurace. Nakonec z toho vznikl opensource projekt.

Baraja WebCrawler
-----------------

Přesně pro tyto potřeby jsem implementoval vlastní Composer balík [WebCrawler](https://github.com/baraja-core/webcrawler), který umí proces indexování stránek řešit elegantně sám a pokud narazím na nový případ, tak ho dále vylepšuji.

Instaluje se Composer příkazem:

```shell
composer require baraja-core/webcrawler
```

A použití je snadné. Stačí vytvořit instanci a zavolat `crawl()` metodu:

```php
$crawler = new \Baraja\WebCrawler\Crawler;

$result = $crawler->crawl('https://example.com');
```

V proměnné `$result` bude dostupný kompletní výsledek jako instance entity `CrawledResult`, kterou doporučuji prostudovat, protože obsahuje mnoho zajímavých informací o celém webu.

Nastavení crawleru
------------------

Často potřebujeme stahování stránek nějak omezit, protože jinak bychom patrně stáhli celý internet.

K tomu slouží entita `Config`, které se předává konfigurace jako pole (klíč - hodnota) a následně předává konstruktorem do Crawleru.

Například:

```php
$crawler = new \Baraja\WebCrawler\Crawler(
    new \Baraja\WebCrawler\Config([
        // key => value
    ])
);
```

Možnosti nastavení:

| Klíč                    | Výchozí hodnota | Možné hodnoty |
|-------------------------|---------------|-----------------|
| `followExternalLinks`   | `false`       | `Bool`: Zůstat jen v rámci stejné domény? Může indexovat i externí odkazy? |
| `sleepBetweenRequests`  | `1000`        | `Int`: Čas čekání mezi stažením jednotlivých stránek v milisekundách. |
| `maxHttpRequests`       | `1000000`     | `Int`: Kolik maximálně stáhnout URL? |
| `maxCrawlTimeInSeconds` | `30`          | `Int`: Jak dlouho maximálně stahovat v sekundách? |
| `allowedUrls`           | `['.+']`      | `String[]`: Pole povolených formátů URL jako regulární výrazy. |
| `forbiddenUrls`         | `['']`        | `String[]`: Pole zakázaných formátů URL jako regulární výrazy. |

Reguární výraz se musí přesně shodovat s celou URL adresou jako string.
