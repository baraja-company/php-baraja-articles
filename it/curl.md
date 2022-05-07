cURL in PHP - scaricare dati tramite URL
========================================

> id: '0bd1aed6-460d-4b63-9afe-f5087d1c6046'
> slug:
> 	cs: curl
> 	it: curl-in-php---scaricare-dati-tramite-url
> 
> perex:
> 	- Knihovna cURL je robustní PHP knihovna pro pokročilou HTTP komunikaci.
> 	- La libreria cURL è una robusta libreria PHP per la comunicazione HTTP avanzata.
> 
> publicationDate: '2020-02-15 22:14:32'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '7c565449293cf1ce4950f309abaf04d0'

La libreria PHP `cURL` è un buon modo per scaricare dati da un server straniero.

Sulla base di una query, costruisce una richiesta HTTP che invia al server di destinazione, e una volta scaricata, contiene un'API per una (relativamente) facile gestione dei dati.

A differenza della funzione nativa `file_get_contents` (attraverso la quale possiamo anche fare richieste HTTP), offre opzioni di configurazione molto migliori e scarica pagine/file come un vero browser.

> La funzione `file_get_contents` usa internamente la libreria `cURL`, solo che non ha opzioni di configurazione così dettagliate.

Rilevare la modalità cURL in una richiesta
----------------------------

È spesso utile rilevare se la richiesta corrente è stata fatta tramite `cUrl` o classicamente nel browser.

Non c'è un'implementazione diretta per questo in PHP, ma possiamo scrivere noi stessi una semplice funzione:

```php
function isCurl(): bool
{
    return str_contains($_SERVER['HTTP_USER_AGENT'] ?? '', 'ricciolo');
}
```

Se hai Linux e il suo terminale, o sei su un Mac, prova questo comando:

```shell
curl https://php.baraja.cz/curl
```

Il comando fa una richiesta interna a questo sito e restituisce il risultato.

Se l'applicazione non ha rilevato una richiesta cURL, l'HTML sarà restituito come se la richiesta provenisse dal browser. Tuttavia, poiché i tipi di richiesta sono rilevati, non c'è nulla che ci impedisca di restituire un articolo Markdown pulito.

Il vantaggio è che i dati sono molto più puliti. Mostriamo l'HTML formattato all'utente nel browser, ma solo il contenuto di base al robot.

Uso dettagliato dell'API in PHP
--------------------------

Se stai cercando un uso dettagliato di cUrl, ti consiglio di leggere la <a href="https://www.php.net/manual/en/book.curl.php">documentazione ufficiale</a>, che è sempre aggiornata.

Per un uso casuale, c'è una libreria <a href="https://docs.guzzlephp.org/en/stable/">**Guzzle**</a> che gestisce
