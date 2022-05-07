Funzione PHP fopen()
====================

> id: '733ba757-1d5f-4717-a088-5ddabe730838'
> slug:
> 	cs: fopen
> 	it: funzione-php-fopen
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '6b791877e57d88840d603dea69967b69'

La funzione `fopen()` rappresenta un accesso di basso livello ai file su disco.

Il programmatore deve fare tutto da solo (aprire il file, leggere i dati, scrivere nuovi dati, chiudere il file).

Se avete solo bisogno di leggere e scrivere file velocemente, ci sono opzioni più semplici:

- <a href="/file-get-contents">file_get_contents()</a> - lettura da un file
- <a href="/file-put-contents">file_put_contents()</a> - scrivere su un file

Uso di base
----------------

```php
$text = 'Qualsiasi testo che viene salvato...';

$file = fopen('file.html', 'a+'); // Apre il file e il modo

fwrite($file, $text); // Salva su file
fclose($file);        // Chiude il file
```

> Se apriamo un file in lettura e questo non viene chiuso, nessun altro processo può accedervi!

Tipi di modalità di gestione dei file
----------------------------

Possiamo lavorare con i file in diverse modalità, che danno informazioni sui diritti di accesso.

Per esempio, se vogliamo aprire un file in sola lettura, la modalità `r` è sufficiente.

Se apriamo il file per la scrittura, allora sarà marcato come `open` sul disco e un altro processo (script) non sarà in grado di scriverci finché non lo chiudiamo di nuovo. Questo assicura che il file non sarà corrotto durante la scrittura.

| Modo, significato, significato.
|-------|--------|
| Apre il file, se non esiste sarà creato.
| Apre un file per aggiungere dati e o leggere dati, se non esiste sarà creato.
| `r` | Aprire in sola lettura |
| | `r+` | Aprire per leggere e scrivere |
| Aprire per la scrittura, i dati originali saranno cancellati e sostituiti con nuovi dati, se non esistono saranno creati.
| Aprire per scrivere e leggere, i dati originali saranno cancellati e sostituiti con nuovi dati, se non esistono saranno creati.
