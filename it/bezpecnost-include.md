Includere la sicurezza in PHP e nell'allegato dei file
======================================================

> id: '820f8de6-ff1e-406c-8fe5-95080642656f'
> slug:
> 	cs: bezpecnost-include
> 	it: includere-la-sicurezza-in-php-e-nell-allegato-dei-file
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1207d637c20fcb5e8f609be2eb449135'

Spesso, potremmo voler allegare un file a una pagina che abbiamo memorizzato su disco da qualche altra parte. Se inseriamo il suo nome esatto direttamente nella funzione attach, non c'è nulla di cui preoccuparsi.

Allegare un file in modo sicuro
--------------------------

```php
include 'menu.html';
```

La scrittura precedente è perfettamente sicura perché montiamo sempre lo stesso file. In questo caso non può verificarsi un errore di sicurezza. L'unico problema che può verificarsi è l'assenza del file **menu.html**, che farà scattare un messaggio di avvertimento (che probabilmente non apparirà comunque), ma questa situazione è rara perché di solito si allega un file la cui esistenza è praticamente certa.

Allegare un file secondo il modello
--------------------------

Ma cosa succede se, per esempio, vogliamo allegare articoli a un semplice sito di contenuti come questo? Su questo sito, ho una cartella fisica dove gli articoli sono memorizzati in formato HTML e li allego direttamente al codice sorgente.

Tuttavia, la semplice connessione non è sufficiente! Un principiante può chiamare i singoli articoli in questo modo:

```php
include 'articoli/' . $_GET['Articolo'] . '.html';
```

> Ma questo è estremamente pericoloso. Un attaccante può passare un link a un'altra directory usando `../` o qualcosa di simile come nome dell'articolo, e a volte è possibile liberarsi del finale passando un byte nullo alla fine. Dovresti almeno usare la funzione `basename()`, ma è meglio permettere solo i valori della whitelist.

Perché non caricare file non rilevanti?
--------------------------

Spesso non ci preoccupiamo di caricare un file non corretto (inaspettato) - è colpa dell'utente che richiede una pagina che in realtà non vuole, ma ci possono essere situazioni in cui è importante. In particolare:

- L'utente carica un file a cui il pubblico non può accedere e solo il server può arrivarci.
- Il caricamento di qualche altro script PHP può innescare un'azione inaspettata o un messaggio di errore che può dare un indizio su come il sito sta funzionando e aiutare a ulteriori attacchi.
- Il caricamento di un altro file può non solo causarne l'aggiunta al documento, ma anche l'esecuzione.

Whitelist e validazione dell'input
--------------------------

Se non abbiamo la possibilità di convalidare l'input in qualche modo sicuro (ad esempio da whitelist), non dovremmo comunque fare affidamento sull'onestà dell'utente e difendere gli script almeno a livello di PHP.

La prima cosa importante è mettere tutti i file caricati nella stessa cartella (directory) e disabilitare alcuni caratteri pericolosi, specialmente slash e dot. Questo renderà impossibile l'accesso ad altre cartelle che contengono file potenzialmente vulnerabili. La disabilitazione dei caratteri pericolosi può essere fatta anche semplicemente cancellandoli dalla stringa di input.

```php
$load = '../indice'; // questo input potrebbe essere potenzialmente pericoloso
$load = strtr($load, './', ''); // cancella tutti i punti e le barre dalla stringa
include $load .'.html';
```

File in esecuzione?
--------------------------

È importante notare che il costrutto **include** esegue i file quando sono collegati nello stesso modo come se fossero codice PHP, quindi è una buona idea permettere questa possibilità.

Spesso, però, si allegano file che non devono essere eseguiti in seguito, e ci interessa solo il testo memorizzato (contenuto) sotto forma di stringa. Pertanto, possiamo caricare il file in una variabile e lavorare con esso come una stringa, il che è abbastanza sicuro.

```php
$load = '../indice'; // questo input potrebbe essere potenzialmente pericoloso
$load = strtr($load, './', ''); // cancella tutti i punti e le barre dalla stringa
$file = file_get_contents($load . '.html'); // caricare il contenuto nella variabile
echo $file; // scarica il contenuto del file
```

Questa soluzione sembra interessante e sicura a prima vista - ed è sicura. Anche se l'utente riesce a chiamare un file PHP, questo non verrà mai eseguito. Tuttavia, può visualizzarlo (cioè il suo codice sorgente) e dobbiamo stare attenti a questo.

Riconoscere il file desiderato dallo script
--------------------------

Non c'è una guida definitiva per questo, ognuno deve farlo da solo secondo le necessità della sceneggiatura. Per esempio, riconosco un articolo da altri file per il fatto che contengono un'intestazione di dimensione H1. Quindi se qualcuno carica un file dove non c'è un'intestazione, non visualizzo nulla e la pagina finisce con un messaggio di errore. È sempre importante trovare qualche caratteristica unica che solo i file che vuoi hanno e che gli altri file non hanno, e puoi andare da lì.

Conclusione
--------------------------

Anche se la convalida e il caricamento dei file è relativamente semplice, un gran numero di principianti continua a commettere errori - e continuerà a farlo. La cosa più importante è capire bene il significato di ciò che stiamo caricando e come distinguere il contenuto che vogliamo dal resto. E soprattutto, lavorate con il contenuto come una stringa e non caricatelo mai direttamente nella pagina.
