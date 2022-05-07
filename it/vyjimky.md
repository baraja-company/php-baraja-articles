Eccezioni e cattura in PHP
==========================

> id: '61b0f178-bb1c-4166-9e8a-af49de2e2a8c'
> slug:
> 	cs: vyjimky
> 	it: eccezioni-e-cattura-in-php
> 
> publicationDate: '2020-02-16 22:18:18'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: d843fe11a092943db429cb8a28384a31

Le eccezioni sono gli strumenti della programmazione orientata agli oggetti, che fornisce un modo elegante per lanciare e gestire (trattare) gli errori delle applicazioni.

Un'eccezione viene prima lanciata (`thrown`), trattata (`try`) e catturata (`catch`). Solo il lancio è obbligatorio.

Filosofia della generazione di eccezioni
-------------------------

Prima della comparsa delle eccezioni, la gestione degli errori nella programmazione era molto complicata perché dovevamo fare affidamento sui valori di ritorno delle funzioni e catturarli a modo nostro e comportarci di conseguenza.

Infatti, le funzioni stesse non applicano la gestione degli errori, il che può portare a problemi fatali, ma David ha scritto su questo in <a href="https://phpfashion.com/programatori-chyby-neignoruji">Programmers don't ignore errors</a>.

Un esempio di gestione degli errori dimenticati:

```php
// passando da disco a disco
copy('c:/oldfile', 'd:/newfile');
unlink('c:/oldfile');
// se la prima operazione fallisce, il file viene irrimediabilmente cancellato
```

Questo perché il modo corretto di gestire l'output della funzione `copy()` è di non continuare e lanciare un errore. Nel caso delle buone vecchie funzioni, potrebbe essere così:

```php
function backup(): bool
{
   if (copy('c:/oldfile', 'd:/newfile')) {
      return unlink('c:/oldfile');
   }

   return false;
}
```

La nostra funzione `backup()` restituirà `true` solo se la funzione `copy()` non ha fallito e la funzione `unlink()` ha restituito `true`. Altrimenti, restituirà `false`.

Ma questo è ora sicuro per l'applicazione? Non lo è! Perché ora dobbiamo trattare l'output della funzione `backup()` nel punto in cui la chiamiamo, e se fallisce, non sapremo nemmeno perché. In breve, restituirà `false` e dobbiamo rilevare l'errore da soli in qualche modo. Credo che in questo caso sia un bene vedere che i programmatori spesso rinunciano alla gestione degli errori, o semplicemente si dimenticano di gestire qualcosa e l'applicazione lancia errori difficili da rilevare a causa di questo.

La soluzione a questo problema è usare eccezioni che forzano il trattamento, e se non vengono trattate, l'applicazione si blocca completamente e si scopre sempre il perché.

Definizione di base delle eccezioni
--------------------------

In PHP, un'eccezione è un tipo speciale di interfaccia implementata dalla classe nativa `Exception` che useremo.

Se l'elaborazione di qualche parte del programma fallisce, si lancia semplicemente un'eccezione che descrive il problema:

```php
if (copy('c:/oldfile', 'd:/newfile') === false) {
   throw new \Exception('Impossibile copiare il file "oldfile".');
}
```

Il lancio di un'eccezione è fatto con la parola chiave `throw`, seguita dalla creazione di un'istanza della classe con l'eccezione. Possiamo anche ottenere un'istanza in altri modi (per esempio, passandola da una variabile), e la semplice creazione di un'istanza di eccezione non ne causa il lancio.

Il primo argomento del costruttore della classe ``Exception`` accetta il testo dell'eccezione, che dovrebbe spiegare in modo conciso cosa è appena successo. È buona pratica includere anche informazioni sull'operazione che viene eseguita e un riferimento ai dati. Per esempio, se una copia di un file è fallita, è buona pratica passare il nome del file. Se l'esecuzione della query SQL fallisce, passiamo di nuovo la query in esecuzione. Questo ci aiuterà molto più tardi quando gestiremo gli errori, perché possiamo vedere esattamente qual è il problema.

Gestione delle eccezioni
-----------------

Per esempio, abbiamo una funzione `backup()` che fa il backup dei dati e può lanciare un paio di errori:

```php
function backup(): void
{
   if (copy('c:/oldfile', 'd:/newfile')) {
      if (unlink('c:/oldfile') === false) {
         throw new \Exception('Non posso rimuovere il vecchio file.');
      }
   }

   throw new \Exception('Non è possibile copiare i file di backup.');
}
```

Notate che la funzione non restituisce alcun output, e noi specifichiamo il tipo `void` nella definizione. La funzione non ha bisogno di restituire nulla, perché il successo è considerato uno stato in cui non viene lanciato nessun errore e non abbiamo bisogno di trattare uno scenario positivo.

Se dovessimo usare la funzione in un'applicazione senza trattamento, per esempio, come segue:

```php
echo 'I file di backup...';
backup();
echo 'Backup completato.';
```

Questo è il modo normale in cui funziona. Tuttavia, se si verifica un errore, lo script terminerà automaticamente e il testo dell'eccezione sarà visualizzato sull'output. È importante che non continui ad eseguire il codice e sappiamo che non ci sarà corruzione dei dati.

Se vogliamo continuare l'esecuzione, dobbiamo **pulire** l'errore, cosa che facciamo usando i costrutti `try` e `catch`:

```php
echo 'I file di backup...';
try {
   backup();
} catch (\Exception $e) {
   echo 'Backup fallito:' . $e->getMessage();
}
echo 'Backup completato.';
```

Se viene lanciata un'eccezione, il codice nell'area `catch()` (che accetta l'eccezione se corrisponde al suo tipo di dati) viene chiamato e il codice interno viene eseguito.

Otteniamo sempre un'istanza della classe delle eccezioni, che può essere usata, per esempio, per visualizzare un messaggio di errore, che è gestito dal metodo `getMessage()`. È anche utile conoscere il metodo `getFile()`, che restituisce il percorso su disco del file che contiene l'errore, `getCode()`, che restituisce il codice di stato dell'errore, e `getLine()`, che restituisce il numero di linea dove è stata lanciata l'eccezione.

Eccezioni preparate
------------------------

Oltre all'eccezione di base `\Exception`, PHP include altri tipi di eccezione predefiniti che sono adatti a diversi casi d'uso.

| Tipo di dati | Spiegazione |
|------------|-----------|
| `LogicException` | Errore logico, prevedibile nella progettazione del programma |
| `BadFunctionCallException` | Errore di chiamata di funzione; funzione non trovata; chiamata non permessa |
| `BadMethodCallException` | Errore nella chiamata del metodo |
| `InvalidArgumentException` | Argomento errato (non valido) passato alla funzione o al metodo |
| `OutOfRangeException` | Valore fuori dall'intervallo della matrice o dell'insieme |
| `LengthException` | Il valore supera la lunghezza consentita |
| `DomainException` | Il valore non rientra nel dominio o nell'intervallo richiesto |
| `RuntimeException` | Errore rilevabile solo in fase di esecuzione (per esempio, mancata scrittura di un file) |
| `OverflowException` | Overflow del buffer o delle operazioni aritmetiche, spesso causato dall'elaborazione di più dati del previsto |
| `UnderflowException` | Underflow di un buffer o di un'operazione aritmetica, sono stati passati meno dati del previsto |
| `OutOfBoundsException` | Indice fuori dall'intervallo della matrice o dell'insieme |
| `RangeException` | Valore non compreso nell'intervallo richiesto |
| `UnexpectedValueException` | Valore inaspettato (ad esempio il valore di ritorno di una funzione) |

Le eccezioni `LogicException` e `RuntimeException` dovrebbero essere prevenute da una corretta progettazione del programma. Personalmente, li uso solo per situazioni eccezionali, come la mancata scrittura di un file e la comunicazione con un servizio esterno.

Raccomando di non catturare affatto la `RuntimeException` e lasciare che l'applicazione fallisca. Questo è di solito un problema serio che dovrebbe essere segnalato il prima possibile.
