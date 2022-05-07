Herunterladen der gesamten Website über Links in PHP
====================================================

> id: dd4abb8e-8f9b-4867-b98f-ff1c859d387a
> slug:
> 	cs: stazeni-celeho-webu-po-odkazech
> 	de: herunterladen-der-gesamten-website-ueber-links-in-php
> 
> publicationDate: '2019-11-06 17:41:30'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: e70451decff5c7bdf19bca8181c0dd5e

Relativ häufig löse ich die Aufgabe des Herunterladens aller Seiten innerhalb einer Website oder Domäne, weil ich dann verschiedene Messungen mit den Ergebnissen durchführe oder die Seiten für eine Volltextsuche verwende.

Eine mögliche Lösung ist die Verwendung des vorgefertigten Tools [Xenu] (http://home.snafu.de/tilman/xenulink.html), das sehr schwierig auf einem Webserver zu installieren ist (es ist ein Windows-Programm), oder [Wget] (https://www.gnu.org/software/wget/), das nicht überall unterstützt wird und eine weitere unnötige Abhängigkeit schafft.

Wenn die Aufgabe nur darin besteht, eine Kopie der Seite für die spätere Betrachtung zu erstellen, ist das Programm [HTTrack] (https://www.httrack.com/) sehr nützlich, das ich am meisten mag, nur wenn wir parametrisierte URLs indizieren, können wir in einigen Fällen an Genauigkeit verlieren.

Also habe ich mich auf die Suche nach einem Tool gemacht, das alle Seiten automatisch direkt in PHP mit erweiterter Konfiguration indizieren kann. Schließlich wurde daraus ein Open-Source-Projekt.

Baraja WebCrawler
-----------------

Für genau diese Bedürfnisse habe ich mein eigenes Composer-Paket [WebCrawler] (https://github.com/baraja-core/webcrawler) implementiert, das den Prozess der Indizierung von Seiten elegant allein bewältigen kann, und wenn ich auf einen neuen Fall stoße, verbessere ich es weiter.

Es wird mit dem Befehl Composer installiert:

```shell
composer require baraja-core/webcrawler
```

Und es ist einfach zu bedienen. Erstellen Sie einfach eine Instanz und rufen Sie die Methode "crawl()" auf:

```php
$crawler = new \Baraja\WebCrawler\Crawler;

$result = $crawler->crawl('https://example.com');
```

In der Variablen `$result` steht das vollständige Ergebnis als Instanz der Entität `CrawledResult` zur Verfügung, deren Untersuchung ich empfehle, da sie viele interessante Informationen über die gesamte Website enthält.

Crawler-Einstellungen
------------------

Oft müssen wir das Herunterladen von Seiten irgendwie begrenzen, weil wir sonst wahrscheinlich das ganze Internet herunterladen würden.

Dazu wird die Entität "Config" verwendet, der die Konfiguration als Array (Schlüssel-Wert) übergeben wird und die dann vom Konstruktor an den Crawler übergeben wird.

Zum Beispiel:

```php
$crawler = new \Baraja\WebCrawler\Crawler(
    new \Baraja\WebCrawler\Config([
        // Schlüssel => Wert
    ])
);
```

Einstellungsmöglichkeiten:

| Schlüssel | Standardwert | Mögliche Werte |
|-------------------------|---------------|-----------------|
| `followExternalLinks` | `false` | `Bool`: Nur innerhalb der gleichen Domain bleiben? Kann es auch externe Links indizieren?
| `sleepBetweenRequests` | `1000` | `Int`: Wartezeit zwischen dem Herunterladen der einzelnen Seiten in Millisekunden.
| `maxHttpRequests` | `1000000` | `Int`: Wie viele URLs werden maximal heruntergeladen?
| `maxCrawlTimeInSeconds` | `30` | `Int`: Wie lange ist der maximale Download in Sekunden?
| `allowedUrls` | `['.+']` | `String[]`: Array der erlaubten URL-Formate als reguläre Ausdrücke.
| `forbiddenUrls` | `['']` | `String[]`: Array von verbotenen URL-Formaten als reguläre Ausdrücke.

Der reguläre Ausdruck muss mit der gesamten URL als Zeichenkette übereinstimmen.
