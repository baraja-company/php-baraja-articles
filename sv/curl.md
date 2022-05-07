cURL i PHP - nedladdning av data via URL
========================================

> id: '0bd1aed6-460d-4b63-9afe-f5087d1c6046'
> slug:
> 	cs: curl
> 	sv: curl-i-php---nedladdning-av-data-via-url
> 
> perex:
> 	- Knihovna cURL je robustní PHP knihovna pro pokročilou HTTP komunikaci.
> 	- Biblioteket cURL är ett robust PHP-bibliotek för avancerad HTTP-kommunikation.
> 
> publicationDate: '2020-02-15 22:14:32'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '7c565449293cf1ce4950f309abaf04d0'

PHP-biblioteket `cURL` är ett bra sätt att hämta data från en utländsk server.

Baserat på en förfrågan skapar den en HTTP-förfrågan som skickas till målservern och när den väl har laddats ner innehåller den ett API för (relativt) enkel datahantering.

Till skillnad från den inhemska funktionen `file_get_contents` (genom vilken vi också kan göra HTTP-förfrågningar) erbjuder den mycket bättre konfigurationsalternativ och laddar ner sidor/filer som en riktig webbläsare.

> Funktionen `file_get_contents` använder biblioteket `cURL` internt, men har inte lika detaljerade konfigurationsalternativ.

Upptäcka cURL-läge i en begäran
----------------------------

Det är ofta användbart att upptäcka om den aktuella begäran gjordes via `cUrl` eller klassiskt i webbläsaren.

Det finns ingen direkt implementering för detta i PHP, men vi kan skriva en enkel funktion själva:

```php
function isCurl(): bool
{
    return str_contains($_SERVER['HTTP_USER_AGENT'] ?? '', 'curl');
}
```

Om du har Linux och dess Terminal, eller om du har en Mac, kan du prova det här kommandot:

```shell
curl https://php.baraja.cz/curl
```

Kommandot gör en intern förfrågan till denna webbplats och returnerar resultatet.

Om programmet inte upptäckte cURL-förfrågan kommer HTML-filen att returneras som om förfrågan kom från webbläsaren. Eftersom begäranstyperna upptäcks finns det dock inget som hindrar oss från att returnera en renodlad Markdown-artikel.

Fördelen är att uppgifterna blir mycket bättre rensade. Vi visar den formaterade HTML-koden för användaren i webbläsaren, men endast det grundläggande innehållet för roboten.

Detaljerad användning av API:et i PHP
--------------------------

Om du letar efter detaljerad användning av cUrl rekommenderar jag att du läser <a href="https://www.php.net/manual/en/book.curl.php">officiell dokumentation</a>, som alltid är uppdaterad.

För tillfällig användning finns det ett <a href="https://docs.guzzlephp.org/en/stable/">**Guzzle**</a>-bibliotek som hanterar
