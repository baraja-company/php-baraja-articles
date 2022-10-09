cURL i PHP - downloading af data via URL
========================================

> id: '0bd1aed6-460d-4b63-9afe-f5087d1c6046'
> slug:
> 	cs: curl
> 	da: curl-i-php-downloading-af-data-via-url
> 
> perex:
> 	- Knihovna cURL je robustní PHP knihovna pro pokročilou HTTP komunikaci.
> 	- cURL-biblioteket er et robust PHP-bibliotek til avanceret HTTP-kommunikation.
> 
> publicationDate: '2020-02-15 22:14:32'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '7c565449293cf1ce4950f309abaf04d0'

PHP-biblioteket `cURL` er en god måde at hente data fra en fremmed server på.

Baseret på en forespørgsel opbygger den en HTTP-forespørgsel, som den sender til målserveren, og når den er downloadet, indeholder den en API til (relativt) nem datahåndtering.

I modsætning til den oprindelige `file_get_contents`-funktion (hvorigennem vi også kan lave HTTP-forespørgsler), tilbyder den langt bedre konfigurationsmuligheder og downloader sider/filer som en rigtig browser.

> Funktionen `file_get_contents` bruger `cURL`-biblioteket internt, den har bare ikke så detaljerede konfigurationsmuligheder.

Registrering af cURL-tilstand i en anmodning
----------------------------

Det er ofte nyttigt at registrere, om den aktuelle forespørgsel blev foretaget via `cUrl` eller klassisk i browseren.

Der findes ingen direkte implementering af dette i PHP, men vi kan selv skrive en simpel funktion:

```php
function isCurl(): bool
{
    return str_contains($_SERVER['HTTP_USER_AGENT'] ?? '', 'krølle');
}
```

Hvis du har Linux og dets Terminal eller er på en Mac, kan du prøve denne kommando:

```shell
curl https://php.baraja.cz/curl
```

Kommandoen foretager en intern forespørgsel til dette websted og returnerer resultatet.

Hvis programmet ikke har registreret cURL-forespørgslen, returneres HTML-koden, som om forespørgslen kom fra browseren. Men da anmodningstyperne er registreret, er der intet, der forhindrer os i at returnere en renset Markdown-artikel.

Fordelen er, at dataene er meget bedre renset. Vi viser den formaterede HTML-tekst til brugeren i browseren, men kun det grundlæggende indhold til robotten.

Detaljeret brug af API'et i PHP
--------------------------

Hvis du er på udkig efter detaljeret brug af cUrl, anbefaler jeg at læse <a href="https://www.php.net/manual/en/book.curl.php">officiel dokumentation</a>, som altid er opdateret.

Til lejlighedsvis brug findes der et <a href="https://docs.guzzlephp.org/en/stable/">**Guzzle**</a> bibliotek, der håndterer
