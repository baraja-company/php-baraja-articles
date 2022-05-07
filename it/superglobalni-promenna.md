Variabili superglobali
======================

> id: bc2107ee-6551-4559-8c02-9cecdf98d33b
> slug:
> 	cs: superglobalni-promenna
> 	it: variabili-superglobali
> 
> publicationDate: '2019-11-01 09:29:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '792e217f1331f38ed0719d7abeb05e67'

Le variabili superglobali sono usate per passare lo stato globale dell'applicazione e la comunicazione HTTP.

Il vantaggio principale di queste variabili è che sono sempre e ovunque disponibili. In pratica, sono array di valori in cui si accede a informazioni specifiche per indice. In diversi contesti, la disponibilità delle chiavi può variare (spiegato di seguito).

Tipi di variabili superglobali
--------------------------------

Tutti i superglobal in PHP sono array e sono denotati da un segno di dollaro seguito da un trattino basso (eccetto `$GLOBALS`) e da caratteri maiuscoli.

In `PHP 7` ci sono in particolare le seguenti:

| Variabile | Descrizione |
|-------------|-------|
| `$_GET` | Parametri URL <a href="/metodi-odesilani-dat">inviato dal metodo GET</a>
| `$_POST` | Dati del modulo <a href="/methods-odesilani-dat">inviato da POST</a>. Nota che <a href="/ajax-post">può comportarsi diversamente in ajax</a>.
| `$_REQUEST` | Dati del modulo inviati da qualsiasi metodo (`$_GET`, `$_POST` e `$_REQUEST`).
| `$_FILES` | Informazioni tecniche sui file attualmente caricati, per esempio tramite il costrutto `<input type="file">`.
| `$_SERVER` | <a href="/info">Impostazioni del server web</a>, indirizzo IP, configurazione... varia a seconda dell'ambiente (quando si chiama uno script PHP da terminale conterrà valori diversi e per esempio le informazioni sulla richiesta corrente saranno mancanti).
| `$_COOKIE` | Configurato <a href="/cookies">cookies</a>.
| `$_SESSION` | Dati di sessione (<a href="/sessions">session</a>), se esiste ed è stato impostato in passato.
| `$GLOBALS` | **Attenzione, non contiene un underscore nel nome!** Questa è la cosiddetta <a href="global-variable">global-variable</a> e una notazione alternativa per la parola chiave `global`. Se hai una variabile globale `$variabile` nella tua applicazione, puoi anche accedervi con il costrutto `$GLOBALS["variabile"]`. Tuttavia, usare le variabili globali è una soluzione cattiva e impura per progettazione, quindi è meglio non farlo.
| `$_ENV` | Informazioni sull'ambiente corrente dove PHP è in esecuzione.

Elencare tutti i valori esistenti è facile da fare:

```php
foreach ($_SERVER as $key => $value {
	echo $key . ':' . $value . '<br>';
}
```

> Nota: non tutti gli indici devono sempre esistere (per esempio, se lo script esegue cron in modalità CLI, l'indice con l'URL della pagina o l'indirizzo IP della richiesta non esisterà).

Accesso alle variabili
-------------------

Raccomando che tutte le variabili globali (tranne `$_SESSION`) siano di sola lettura. Questo perché contengono dati globali dell'applicazione e altro codice può tenerne conto (per esempio, un'altra libreria installata).

Un altro svantaggio dello stato globale è che non si può sempre fare affidamento sui valori esatti, anche se esistono, quindi si dovrebbe sempre controllare le loro chiavi con il costrutto `isset()`.

Per salvare un nuovo cookie, usate `setcookie()` e non inserite il valore direttamente. Questo perché è di sola lettura.

Lezioni apprese
-------

Mai fidarsi ciecamente dei valori delle variabili superglobali!

L'utente può usare l'URL e le intestazioni inviate per influenzare il modo in cui i valori sono impostati. Tutti gli input devono sempre essere accuratamente convalidati.

Registrare globals - il problema con la vecchia versione di PHP
------------------------------------------

Nella vecchia versione di PHP (fino a `5.4.0`), c'era una direttiva speciale `register-globals` (configurabile in `php.ini`) che faceva sì che tutti i parametri passati in un URL fossero automaticamente registrati come variabili.

Per esempio:

Un utente è arrivato all'URL: `https://example.com/script.php?var=24`.

E PHP ha creato automaticamente una variabile `$var` con il valore `24` all'interno dello script.

Quindi ha funzionato in modo classico:

```php
echo $var;
```

Chiunque potrebbe quindi infilare qualsiasi variabile nello script e cambiarne il contenuto. Ovviamente, la sicurezza non era sempre una priorità. Non proprio.

Altre fonti
------------

Per una descrizione più dettagliata, vedere il <a href="https://www.php.net/manual/en/language.variables.superglobals.php">manuale ufficiale</a>.
