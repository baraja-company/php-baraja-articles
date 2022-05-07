Metodi di invio dei dati (GET e POST)
=====================================

> id: '32f9083f-7cb1-469f-911a-765df059123d'
> slug:
> 	cs: metody-odesilani-dat
> 	it: metodi-di-invio-dei-dati-get-e-post
> 
> perex:
> 	- 'Metoda GET a POST, získávání dat z formuláře a URL. Komunikace přes API a zpracování dat.'
> 	- 'Metodo GET e POST, recupero di dati da moduli e URL. Comunicazione API ed elaborazione dei dati.'
> 
> publicationDate: '2019-11-26 11:38:32'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '2282538597ebfe95877ae0e005ddd352'

Oltre alle variabili regolari, abbiamo anche le cosiddette **variabili superglobali** in PHP, che portano informazioni sulla pagina attualmente chiamata e sui dati che stiamo passando.

Tipicamente, abbiamo un modulo su una pagina che l'utente compila e vogliamo trasferire quei dati al server web dove li elaboriamo in PHP.

Ci sono 3 metodi più comunemente usati per farlo:

- `GET` ~ i dati sono passati nell'URL come parametri
- `POST` ~ i dati sono passati segretamente insieme alla richiesta della pagina
- <a href="/ajax-post">Ajax POST</a> ~ elaborazione javascript asincrona

Metodo GET - `$_GET`
--------------------

I dati inviati dal metodo GET sono visibili nell'URL (come parametri dopo il punto interrogativo), la lunghezza massima è di 1024 caratteri in Internet Explorer (altri browser *quasi* non lo limitano, ma i testi più grandi non dovrebbero essere passati in questo modo). Il vantaggio di questo metodo è soprattutto la semplicità (puoi vedere cosa stai inviando) e la possibilità di fornire un link al risultato dell'elaborazione. I dati vengono inviati a una variabile.

L'indirizzo della pagina di ricezione potrebbe essere così:

`https://____________.com/script.php?promenna=obsah&promenna2=obsah`

In PHP, possiamo quindi, per esempio, scrivere il valore del parametro `variabile` come segue:

```php
echo $_GET['passeggiata'];	// stampa "contenuto"
```

> **Avviso forte:** Questo metodo di scrivere dati direttamente nella pagina HTML non è sicuro, perché possiamo passare, per esempio, codice HTML nell'URL che verrebbe scritto nella pagina e poi eseguito.
>
> Dobbiamo **sempre** trattare i dati prima di qualsiasi output alla pagina, la funzione `htmlspecialchars()` è usata per questo.
>
> Per esempio: `echo htmlspecialchars($_GET['variabile']);`

Metodo POST - `$_POST`
----------------------

I dati inviati con il metodo POST non sono visibili nell'URL, il che risolve il problema della lunghezza massima dei dati inviati. Il metodo POST dovrebbe sempre essere usato per inviare i campi del modulo, in quanto questo assicura che, per esempio, le password non siano visibili e che non si possa fornire un link alla pagina che elabora il risultato di un particolare input.

I dati sono disponibili nella variabile `$_POST` e l'uso è lo stesso del metodo GET.

Verifica dell'esistenza dei dati inviati
--------------------------------

Prima di trattare qualsiasi dato, dovremmo prima verificare che i dati siano stati effettivamente inviati, altrimenti accederemmo a
 ad una variabile inesistente, che lancerebbe un messaggio di errore.

La funzione `isset()` è usata per verificare l'esistenza di una variabile.

```php
if (isset($_GET['Nome'])) {
    echo 'Il tuo nome:' . htmlspecialchars($_GET['Nome']);
} else {
    echo 'Non è stato inserito alcun nome.';
}
```

Modulo di inserimento dati
------------------------

Il modulo è fatto in HTML, non in PHP. Può essere su una semplice pagina HTML. Tutta la "magia" è gestita dallo script PHP che accetta i dati.

Per esempio, possiamo usare un modulo per ricevere 2 numeri, inviati con il metodo GET:

```html
<form action="script.php" method="get">
    První číslo: <input type="text" name="x">
    Druhé číslo: <input type="text" name="y">

    <input type="submit" value="Sečíst čísla">
</form>
```

Nella prima riga potete vedere dove i dati saranno inviati e con quale metodo.

Le prossime 2 linee sono semplici elementi del modulo, notate l'attributo **name=""**, c'è il nome della variabile che conterrà ciò che è ora nel modulo.

Poi viene il pulsante per inviare i dati (obbligatorio) e il tag HTML di chiusura del modulo (obbligatorio affinché il browser sappia cos'altro inviare e cosa no).

> Possiamo avere un numero qualsiasi di moduli in una pagina, e non possono essere annidati. Se si verifica l'annidamento, la forma più annidata viene sempre inviata e le altre vengono ignorate.

Elaborazione del modulo sul server
-------------------------------

Ora abbiamo un modulo HTML finito e lo inviamo a `script.php`, che riceve i dati usando il metodo GET. L'indirizzo della pagina richiesta potrebbe essere così:

`https://________.com/script.php?x=5&y=3`

**script.php**

```php
$x = $_GET['x'];	// 5
$y = $_GET['y'];	// 3

echo $x + $y;		// stampa 8
```

Correttamente, dovremmo prima verificare che entrambi i campi del modulo siano stati compilati, questo viene fatto con la funzione `isset()`:

```php
if (isset($_GET['x']) && isset($_GET['y'])) {
    $x = $_GET['x'];	// 5
    $y = $_GET['y'];	// 3

    echo $x + $y;		// stampa 8
} else {
    echo 'Il modulo non è stato compilato correttamente.';
}
```

> **TIP:** Potete passare più parametri al costrutto `isset()` per verificare che esistano tutti.
>
> Pertanto, invece di `isset($_GET['x']) && isset($_GET['y'])`, potete semplicemente specificare:
>
> `isset($_GET['x'], $_GET['y'])`.

Elaborazione dei dati ricevuti dal metodo POST
--------------------------------------

Se i dati vengono ricevuti tramite POST, l'URL dello script da elaborare sarà sempre così:

`https://________.com/script.php`

E mai altrimenti. Proprio no. I dati sono nascosti nella richiesta HTTP e non possiamo vederli.

> Il metodo POST nascosto è richiesto per inviare nomi utente e password per motivi di sicurezza.
>
> **Sicurezza:** Se stai lavorando con le password sul tuo sito, il modulo di login e registrazione dovrebbe essere ospitato su HTTPS e devi fare un hash delle password in modo appropriato (per esempio, con BCrypt).

Gestire le richieste ajax
------------------------------

In alcuni casi, quando si elaborano richieste ajax, potrebbe non essere facile recuperare i dati. Questo perché le librerie ajax di solito inviano dati come `json payload`, mentre la variabile superglobale `$_POST` contiene solo dati di modulo.

Si può ancora accedere ai dati, ho descritto i dettagli nell'articolo <a href="/ajax-post">Gestione delle richieste ajax POST</a>.
