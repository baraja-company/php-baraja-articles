La costruzione include
======================

> id: '881cc6ef-b1dc-4a71-ab22-d1943ce8095b'
> slug:
> 	cs: include-function
> 	it: la-costruzione-include
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '53294e511809a77c08883100fd7416c6'

| Supporto | PHP 4, PHP 5, PHP 7
|---------------|---------
| Descrizione breve | Aggiunge un altro file di testo o uno script allo script.
| Requisiti | Altro file di testo o script da inserire.
| Nota | Impossibile caricare file esterni.

Descrizione
--------------------------

Inserisce un altro file di testo o uno script nella pagina. Supporta i file di testo.

I file incorporati si comportano come se fossero direttamente nella pagina.

Gli script incorporati vengono eseguiti automaticamente.

Lo script incorporato trasferisce il valore delle variabili.

> Non può essere caricato da una memoria esterna. Può leggere solo testo semplice non formattato e file PHP.

Formati di percorso consentiti:

- `script.php` - file nella stessa directory, sarà eseguito
- `script.html` - file nella stessa directory, non sarà eseguito
- `./file.php` - file nella stessa directory, sarà eseguito
- `../page.html`
- Cartellefile\file.php` - scrivere in Windows
- `address/DalsiAddress/file.php` - scrivere su sistemi Unix

Gli slash di tipo `****` e `**/**` possono interconvertirsi, quindi non devi occupartene.

Voce di percorso illegale: `https://domena.pripona/slozka/soubor.php`.

Funzioni simili
--------------------------

- <a href="/file-get-contents">file_get_contents()</a>

Esempio
--------------------------

```php
include 'file.php';
```

Inserisce lo script `file.php` nella pagina e lo esegue.

`vars.php`.

```php
$color = 'verde';
$fruit = 'mela';
```

`test.php`.

```php
$color = '';
$fruit = '';

echo 'A' . $color . '' . $fruit; // A

include 'vars.php';

echo 'A' . $color . '' . $fruit; // Una mela verde
```

> ATTENZIONE: La seguente notazione non è possibile, trasferire il contenuto delle variabili definendole!

```php
include 'file.php?parameter=neco';
```

Valori di ritorno
--------------------------

Nessuna, mette solo il file.

**NOTA:** Permette di confrontare il contenuto dei file, ma questo è un rischio per la sicurezza. L'ordine delle parentesi conta! Esempio:

```php
if ((include 'file.php') == 'OK') {
    echo 'Il valore è "OK".';
}
```

> Questo è un potenziale rischio per la sicurezza!
>
> Può essere risolto con **file_get_contents()**, **readfile()** o **fopen()**. Fopen() dovrebbe essere usato solo su file .txt e .html.
>
> Una lettura più sicura del file può essere risolta definendo una funzione personalizzata.

Esempio:

```php
$string = get_include_contents('qualche file.php');

function get_include_contents(string $filename): ?string
{
    if (is_file($filename)) {
        ob_start();
        include (string) $filename;
        return ob_get_clean();
    }

    return null;
}
```

Note e suggerimenti di altri sviluppatori
--------------------------

**Jakub Vrána** mi ha mandato una mail:

```php
include 'articoli/' . $_GET['Articolo'] . '.html';
```

Questo è estremamente pericoloso.

Un attaccante può passare un link a un'altra directory usando `../` o qualcosa di simile come nome dell'articolo, e a volte è possibile liberarsi del finale passando un byte nullo alla fine.

Dovresti almeno usare la funzione `basename()`, ma è meglio permettere solo i valori della whitelist.
