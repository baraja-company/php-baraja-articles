cURL v PHP - sťahovanie údajov prostredníctvom adresy URL
=========================================================

> id: '0bd1aed6-460d-4b63-9afe-f5087d1c6046'
> slug:
> 	cs: curl
> 	sk: curl-v-php---stahovanie-udajov-prostrednictvom-adresy-url
> 
> perex: Knihovna cURL je robustní PHP knihovna pro pokročilou HTTP komunikaci.
> publicationDate: '2020-02-15 22:14:32'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '7c565449293cf1ce4950f309abaf04d0'

Knižnica PHP `cURL` je vhodným spôsobom na sťahovanie údajov z cudzieho servera.

Na základe dotazu zostaví požiadavku HTTP, ktorú odošle na cieľový server, a po stiahnutí obsahuje rozhranie API na (relatívne) jednoduchú manipuláciu s údajmi.

Na rozdiel od natívnej funkcie `file_get_contents` (prostredníctvom ktorej môžeme tiež zadávať požiadavky HTTP) ponúka oveľa lepšie možnosti konfigurácie a sťahuje stránky/súbory ako skutočný prehliadač.

> Funkcia `file_get_contents` interne používa knižnicu `cURL`, len nemá tak podrobné konfiguračné možnosti.

Zisťovanie režimu cURL v požiadavke
----------------------------

Často je užitočné zistiť, či bola aktuálna požiadavka vykonaná prostredníctvom `cUrl` alebo klasicky v prehliadači.

V PHP neexistuje priama implementácia tejto funkcie, ale môžeme si ju napísať sami:

```php
function isCurl(): bool
{
    return str_contains($_SERVER['HTTP_USER_AGENT'] ?? '', 'curl');
}
```

Ak máte Linux a jeho terminál alebo používate Mac, skúste tento príkaz:

curl https://php.baraja.cz/curl

Príkaz vykoná internú požiadavku na túto stránku a vráti výsledok.

Ak aplikácia nezistila požiadavku cURL, HTML sa vráti, ako keby požiadavka prišla z prehliadača. Keďže sa však typy požiadaviek zisťujú, nič nám nebráni vrátiť vyčistený článok Markdown.

Výhodou sú potom oveľa lepšie vyčistené údaje. Používateľovi v prehliadači zobrazíme formátované HTML, ale robotovi iba základný obsah.

Podrobné používanie rozhrania API v jazyku PHP
--------------------------

Ak hľadáte podrobné informácie o používaní cUrl, odporúčam prečítať si <a href="https://www.php.net/manual/en/book.curl.php">oficiálnu dokumentáciu</a>, ktorá je vždy aktuálna.

Na bežné použitie je k dispozícii knižnica <a href="https://docs.guzzlephp.org/en/stable/">**Guzzle**</a>, ktorá spracováva
