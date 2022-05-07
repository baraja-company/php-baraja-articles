Scaricare l'intero sito tramite link in PHP
===========================================

> id: dd4abb8e-8f9b-4867-b98f-ff1c859d387a
> slug:
> 	cs: stazeni-celeho-webu-po-odkazech
> 	it: scaricare-l-intero-sito-tramite-link-in-php
> 
> publicationDate: '2019-11-06 17:41:30'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: e70451decff5c7bdf19bca8181c0dd5e

Relativamente spesso risolvo il compito di scaricare tutte le pagine di un sito o di un dominio, perché poi eseguo varie misurazioni con i risultati, o uso le pagine per la ricerca full-text.

Una possibile soluzione è usare lo strumento già pronto [Xenu](http://home.snafu.de/tilman/xenulink.html), che è molto difficile da installare su un server web (è un programma Windows), o [Wget](https://www.gnu.org/software/wget/), che non è supportato ovunque e crea un'altra dipendenza non necessaria.

Se il compito è solo quello di fare una copia della pagina per la visualizzazione successiva, è molto utile il programma [HTTrack](https://www.httrack.com/), che mi piace di più, solo quando si tratta di URL parametrizzati si può perdere precisione in alcuni casi.

Così ho iniziato a cercare uno strumento che possa indicizzare tutte le pagine automaticamente direttamente in PHP con una configurazione avanzata. Alla fine, questo è diventato un progetto opensource.

Baraja WebCrawler
-----------------

Proprio per queste esigenze ho implementato il mio pacchetto Composer [WebCrawler](https://github.com/baraja-core/webcrawler), che può gestire il processo di indicizzazione delle pagine in modo elegante da solo, e se mi imbatto in un nuovo caso, lo miglioro ulteriormente.

Si installa con il comando Composer:

```shell
composer require baraja-core/webcrawler
```

Ed è facile da usare. Basta creare un'istanza e chiamare il metodo `crawl()`:

```php
$crawler = new \Baraja\WebCrawler\Crawler;

$result = $crawler->crawl('https://example.com');
```

Nella variabile `$result`, il risultato completo sarà disponibile come istanza dell'entità `CrawledResult`, che consiglio di studiare perché contiene molte informazioni interessanti su tutto il sito.

Impostazioni del crawler
------------------

Spesso abbiamo bisogno di limitare il download delle pagine in qualche modo, perché altrimenti probabilmente scaricheremmo tutto internet.

Questo viene fatto usando l'entità `Config`, a cui viene passata la configurazione come array (chiave-valore) e poi passata al Crawler dal costruttore.

Per esempio:

```php
$crawler = new \Baraja\WebCrawler\Crawler(
    new \Baraja\WebCrawler\Config([
        // chiave => valore
    ])
);
```

Opzioni di impostazione:

| Chiave | Valore predefinito | Valori possibili |
|-------------------------|---------------|-----------------|
| `followExternalLinks` | `false` | `Bool`: Resta solo all'interno dello stesso dominio? Può indicizzare anche i link esterni?
| `sleepBetweenRequests` | `1000` | `Int`: tempo di attesa tra il download di ogni pagina in millisecondi.
| `maxHttpRequests` | `1000000` | `Int`: Quanti URL massimi scaricati?
| `maxCrawlTimeInSeconds` | `30` | `Int`: Quanto dura il download massimo in secondi?
| `allowedUrls` | `['.+']` | `String[]`: array di formati URL consentiti come espressioni regolari.
| `forbiddenUrls` | `['']` | `String[]`: array di formati URL proibiti come espressioni regolari.

L'espressione regolare deve corrispondere all'intero URL esattamente come una stringa.
