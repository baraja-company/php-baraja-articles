Stiahnutie celej stránky pomocou odkazov v PHP
==============================================

> id: dd4abb8e-8f9b-4867-b98f-ff1c859d387a
> slug:
> 	cs: stazeni-celeho-webu-po-odkazech
> 	sk: stiahnutie-celej-stranky-pomocou-odkazov-v-php
> 
> publicationDate: '2019-11-06 17:41:30'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: e70451decff5c7bdf19bca8181c0dd5e

Pomerne často riešim úlohu stiahnutia všetkých stránok v rámci jedného webu alebo domény, pretože potom s výsledkami vykonávam rôzne merania alebo stránky používam na fulltextové vyhľadávanie.

Jedným z možných riešení je použitie hotového nástroja [Xenu](http://home.snafu.de/tilman/xenulink.html), ktorý sa na webový server inštaluje veľmi ťažko (je to program pre Windows), alebo [Wget](https://www.gnu.org/software/wget/), ktorý nie je všade podporovaný a vytvára ďalšiu zbytočnú závislosť.

Ak je úlohou len vytvoriť kópiu stránky na neskoršie zobrazenie, je veľmi užitočný program [HTTrack](https://www.httrack.com/), ktorý sa mi páči najviac, len pri indexovaní parametrizovaných adries URL môžeme v niektorých prípadoch stratiť presnosť.

Preto som začal hľadať nástroj, ktorý dokáže automaticky indexovať všetky stránky priamo v PHP s pokročilou konfiguráciou. Nakoniec sa z neho stal opensource projekt.

Baraja WebCrawler
-----------------

Presne pre tieto potreby som implementoval vlastný balík Composer [WebCrawler](https://github.com/baraja-core/webcrawler), ktorý dokáže proces indexovania stránok elegantne zvládnuť sám, a ak narazím na nový prípad, ďalej ho vylepšujem.

Inštaluje sa pomocou príkazu Composer:

composer require baraja-core/webcrawler

A ľahko sa používa. Stačí vytvoriť inštanciu a zavolať metódu `crawl()`:

```php
$crawler = new \Baraja\WebCrawler\Crawler;

$result = $crawler->crawl("https://example.com);
```

V premennej `$result` bude k dispozícii kompletný výsledok ako inštancia entity `CrawledResult`, ktorú odporúčam preštudovať, pretože obsahuje množstvo zaujímavých informácií o celej stránke.

Nastavenia pásového vozidla
------------------

Často musíme nejako obmedziť sťahovanie stránok, pretože inak by sme pravdepodobne stiahli celý internet.

Toto sa vykonáva pomocou entity `Config`, ktorej sa konfigurácia odovzdá ako pole (kľúč-hodnota) a potom sa konštruktorom odovzdá prehľadávaču.

Napríklad:

```php
$crawler = new \Baraja\WebCrawler\Crawler(
    new \Baraja\WebCrawler\Config([
        // kľúč => hodnota
    ])
);
```

Možnosti nastavenia:

| Kľúč | Predvolená hodnota | Možné hodnoty |
|-------------------------|---------------|-----------------|
| `followExternalLinks` | `false` | `Bool`: Zostať len v rámci rovnakej domény? Môže indexovať aj externé odkazy?
| `sleepBetweenRequests` | `1000` | `Int`: Čas čakania medzi stiahnutím každej stránky v milisekundách. |
| `maxHttpRequests` | `1000000` | `Int`: Koľko maximálne stiahnutých adries URL? |
| `maxCrawlTimeInSeconds` | `30` | `Int`: Ako dlho trvá maximálne sťahovanie v sekundách?
| `allowedUrls` | `['.+']` | `String[]`: Pole povolených formátov URL ako regulárnych výrazov. |
| `forbiddenUrls` | `['']` | `String[]`: Pole zakázaných formátov URL ako regulárnych výrazov. |

Regulárny výraz musí presne zodpovedať celej adrese URL ako reťazcu.
