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
> sourceContentHash: '81b5f92d7ee05563b6ece295ed5958d3'

Oltre alle variabili normali, in PHP esistono anche le cosiddette **variabili superglobali**, che contengono informazioni sulla pagina correntemente chiamata e sui dati passati.

In genere, in una pagina abbiamo un modulo che l'utente compila e vogliamo trasferire i dati al server web, dove li elaboriamo in PHP.

I metodi più comunemente utilizzati per farlo sono tre:

- `GET` ~ i dati vengono passati nell'URL come parametri
- `POST` ~ i dati vengono passati di nascosto insieme alla richiesta di pagina
- <a href="/ajax-post">Ajax POST</a> ~ elaborazione javascript asincrona

Metodo GET - `$_GET
--------------------

I dati inviati con il metodo GET sono visibili nell'URL (come parametri dopo il punto interrogativo); la lunghezza massima è di 1024 caratteri in Internet Explorer (gli altri browser *quasi* non lo limitano, ma i testi più grandi non dovrebbero essere passati in questo modo). Il vantaggio di questo metodo è soprattutto la semplicità (si può vedere cosa si sta inviando) e la possibilità di fornire un link al risultato dell'elaborazione. I dati vengono inviati a una variabile.

L'indirizzo della pagina ricevente potrebbe essere il seguente:

`https://____________.com/script.php?promenna=obsah&promenna2=obsah`

In PHP, ad esempio, possiamo scrivere il valore del parametro `variabile` come segue:

```php
echo $_GET['passeggiata'];	// stampa "contenuto"
```

> Questo metodo di scrittura dei dati direttamente nella pagina HTML non è sicuro, perché si può passare, ad esempio, del codice HTML nell'URL che verrebbe scritto nella pagina e poi eseguito.
>
> Dobbiamo **sempre** trattare i dati prima di qualsiasi output alla pagina; la funzione `htmlspecialchars()` è usata per questo.
>
> Per esempio: `echo htmlspecialchars($_GET['variabile']);`

Metodo POST - `$_POST
----------------------

I dati inviati con il metodo POST non sono visibili nell'URL, il che risolve il problema della lunghezza massima dei dati inviati. Per l'invio dei campi dei moduli si dovrebbe sempre utilizzare il metodo POST, che garantisce che, ad esempio, le password non siano visibili e che non si possa fornire un link alla pagina che elabora il risultato di un determinato input.

I dati sono disponibili nella variabile `$_POST` e l'uso è lo stesso del metodo GET.

Verifica dell'esistenza dei dati inviati
--------------------------------

Prima di elaborare qualsiasi dato, è necessario verificare che i dati siano stati effettivamente inviati, altrimenti si accederebbe a un'area di lavoro di un'azienda.
 a una variabile inesistente, con conseguente messaggio di errore.

La funzione `isset()` viene utilizzata per verificare l'esistenza di una variabile.

```php
if (isset($_GET['Nome'])) {
    echo 'Il tuo nome:' . htmlspecialchars($_GET['Nome']);
} else {
    echo 'Non è stato inserito alcun nome.';
}
```

Modulo di inserimento dati
------------------------

Il modulo è realizzato in HTML, non in PHP. Può trovarsi in una semplice pagina HTML. Tutta la "magia" è gestita dallo script PHP che accetta i dati.

Ad esempio, possiamo utilizzare un modulo per ricevere 2 numeri, inviati con il metodo GET:

```html
<form action="script.php" method="get">
    První číslo: <input type="text" name="x">
    Druhé číslo: <input type="text" name="y">

    <input type="submit" value="Sečíst čísla">
</form>
```

Nella prima riga si può vedere dove verranno inviati i dati e con quale metodo.

Le 2 righe successive sono semplici elementi del form, si noti l'attributo **name=""**, che contiene il nome della variabile che conterrà ciò che è ora nel form.

Seguono il pulsante per l'invio dei dati (obbligatorio) e il tag HTML di chiusura del modulo (obbligatorio affinché il browser sappia cos'altro inviare e cosa no).

> Possiamo avere un numero qualsiasi di moduli in una pagina e non possono essere annidati. In caso di annidamento, viene sempre inviato il modulo più annidato e gli altri vengono ignorati.

Elaborazione dei moduli sul server
-------------------------------

Ora abbiamo un modulo HTML finito e lo inviamo a `script.php`, che riceve i dati con il metodo GET. L'indirizzo della richiesta di pagina potrebbe essere simile a questo:

`https://________.com/script.php?x=5&y=3`

**script.php**

```php
$x = $_GET['x'];	// 5
$y = $_GET['y'];	// 3

echo $x + $y;		// stampa 8
```

Correttamente, dovremmo prima verificare che entrambi i campi del modulo siano stati compilati; questo viene fatto con la funzione `isset()`:

```php
if (isset($_GET['x']) && isset($_GET['y'])) {
    $x = $_GET['x'];	// 5
    $y = $_GET['y'];	// 3

    echo $x + $y;		// stampa 8
} else {
    echo 'Il modulo non è stato compilato correttamente.';
}
```

> **TIP:** È possibile passare più parametri al costrutto `isset()` per verificare che esistano tutti.
>
> Pertanto, invece di `isset($_GET['x']) && isset($_GET['y'])`, si può semplicemente specificare:
>
> `isset($_GET['x'], $_GET['y'])`.

Elaborazione dei dati ricevuti con il metodo POST
--------------------------------------

Se i dati vengono ricevuti con il metodo POST, l'URL dello script da elaborare sarà sempre simile a questo:

`https://________.com/script.php`

E mai altrimenti. Semplicemente no. I dati sono nascosti nella richiesta HTTP e non possiamo vederli.

> Il metodo hidden POST è necessario per inviare nomi utente e password per motivi di sicurezza.
>
> Se sul sito si utilizzano password, il modulo di login e di registrazione deve essere ospitato su HTTPS e le password devono essere sottoposte a un hash appropriato (ad esempio, con BCrypt).

Gestione delle richieste ajax
------------------------------

In alcuni casi, durante l'elaborazione delle richieste ajax, potrebbe non essere facile recuperare i dati. Questo perché le librerie ajax di solito inviano i dati come `json payload`, mentre la variabile superglobale `$_POST` contiene solo i dati del modulo.

I dati sono comunque accessibili; ho descritto i dettagli nell'articolo <a href="/ajax-post">Elaborazione delle richieste ajax POST</a>.

Ottenere input grezzi
-----------------------------

A volte può accadere che un utente invii una richiesta utilizzando un metodo HTTP non appropriato e aggiungendovi il proprio input. Oppure, ad esempio, invia un file binario o intestazioni HTTP errate.

In questo caso, è bene utilizzare l'input nativo, che si ottiene in PHP come segue:

```php
$input = file_get_contents('php://ingresso');
```

Durante l'implementazione della libreria API REST, mi sono imbattuto in una serie di casi particolari in cui diversi tipi di server web decidevano in modo errato le intestazioni HTTP di input, oppure l'utente inviava in modo errato i dati del modulo, ecc.

Per questo caso, sono riuscito a implementare questa funzione che risolve quasi tutti i casi (l'implementazione dipende da `Nette\Http\RequestFactory`, ma si può sostituire questa dipendenza con qualcos'altro nel proprio progetto specifico):

```php
/**
 * Ottiene i dati POST direttamente dall'intestazione HTTP o cerca di analizzare i dati dalla stringa.
 * Alcuni client legacy inviano i dati come json, che sono in formato stringa base, quindi la fusione dei campi in array è obbligatoria.
 *
 * @Ritorno array<stringa|int, misto>
 */
private function getBodyParams(string $method): array
{
	if ($method === 'GET' || $method === 'CANCELLARE') {
		return [];
	}

	$request = (new RequestFactory())->fromGlobals();
	$return = array_merge((array) $request->getPost(), $request->getFiles());
	try {
		$post = array_keys($_POST)[0] ?? '';
		if (str_starts_with($post, '{') && str_ends_with($post, '}')) { // supporto per i client legacy
			$json = json_decode($post, true, 512, JSON_THROW_ON_ERROR);
			if (is_array($json) === false) {
				throw new LogicException('Json non è un array valido.');
			}
			unset($_POST[$post]);
			foreach ($json as $key => $value) {
				$return[$key] = $value;
			}
		}
	} catch (Throwable $e) {
		// Il silenzio è d'oro.
	}
	try {
		$input = (string) file_get_contents('php://ingresso');
		if ($input !== '') {
			$phpInputArgs = (array) json_decode($input, true, 512, JSON_THROW_ON_ERROR);
			foreach ($phpInputArgs as $key => $value) {
				$return[$key] = $value;
			}
		}
	} catch (Throwable $e) {
		// Il silenzio è d'oro.
	}

	return $return;
}
```
