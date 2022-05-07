Sessioni - cookie del server in PHP
===================================

> id: ab782ff0-d5ba-4115-9ebb-ab37978a6527
> slug:
> 	cs: sessions
> 	it: sessioni---cookie-del-server-in-php
> 
> publicationDate: '2019-11-06 17:06:18'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '3c84a09e5d180f6a47a58bea70b7ab21'

Spesso abbiamo bisogno di memorizzare più informazioni in <a href="/cookies">cookies</a>, ma il limite massimo per i cookie è di 4 kB, che non è molto. Sessions risolve questo problema memorizzando i dati sul server web, e memorizza solo un breve identificatore nel browser del cliente per dire quali dati appartengono a quale cliente.

Iniziare una sessione
---------------------

Prima di fare qualsiasi lavoro con le sessioni, dobbiamo prima avviarle. Questo viene fatto chiamando la funzione `session_start()` proprio all'inizio dello script:

```php
session_start();
```

> Forte avvertimento: nessun output in codice HTML deve essere eseguito prima di chiamare la funzione `session_start()`!

Sicurezza della sessione
-------------------

Il contenuto della sessione è memorizzato sul server e solo l'identificatore è inviato al browser client, quindi l'utente non ha modo di sapere cosa è memorizzato nella sessione. L'unico modo in cui lo script può influenzare l'utente è cancellando l'identificatore (al che lo script ne genererà uno nuovo).

Recuperare dati da una sessione
----------------------

Tutte le sessioni sono memorizzate nella variabile superglobale `$_SESSION` e possono essere attraversate come un array.

Per esempio, il nome dell'utente attualmente loggato può essere recuperato scrivendo:

```php
echo $_SESSION['utente'];
```

Nota: la sessione potrebbe non esistere sempre (per esempio, se sei un nuovo utente). Pertanto, dovremmo sempre controllare l'esistenza prima di qualsiasi annuncio e offrire un messaggio di errore alternativo se necessario.

```php
if (isset($_SESSION['utente']) && $_SESSION['utente']) {
    echo 'Utente registrato:' . $_SESSION['utente'];
} else {
    echo 'Nessuno si è registrato.';
}
```

Salvare i dati nella sessione
----------------------

Il salvataggio è fatto come un semplice salvataggio di dati in una variabile:

```php
$_SESSION['utente'] = 'Honzik';
```

Il server web si occuperà della disposizione tecnica della corretta memorizzazione sul server e dell'invio dell'identificatore all'utente.

Cancellare le sessioni
----------------

I singoli valori possono essere cancellati separatamente secondo la chiave:

```php
unset($_SESSION['utente']);
```

O in alternativa tutte le sessioni disponibili:

```php
unset($_SESSION);
```

> Nota: la cancellazione di una sessione specifica non svuota il valore della chiave, ma cancella completamente la chiave. Pertanto, verrà lanciato un avviso di errore quando si cerca di leggere una chiave inesistente. Possiamo sempre verificare facilmente l'esistenza di una chiave con la funzione `isset()`.

Validità massima della sessione
---------------------------------

Ogni sessione salvata ha un limite di tempo in cui sarà conservata sul server. PHP contiene direttamente uno script cron che cancella periodicamente le vecchie sessioni.

Il valore predefinito è di solito `1440 secondi`, che è `24 minuti`.

L'aumento del valore deve essere fatto in 2 punti:

- In <a href="/info">`php.ini` è impostata la lunghezza massima di validità che il server manterrà</a>. Il valore è impostato dalla direttiva `session.gc_maxlifetime`,
- Dal lato dello script PHP, è necessario specificare la validità corrente richiesta.

Uso in PHP:

```php
// il server manterrà ora la sessione per un massimo di 3600 secondi = 1 ora
ini_set('sessione.gc_maxlifetime', '3600');

// tutti i client (browser) saranno
// sessione inviata con una validità di esattamente 3600 secondi
session_set_cookie_params(3600);

session_start(); // possiamo iniziare la sessione!
```
