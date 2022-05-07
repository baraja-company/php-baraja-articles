Cookie in PHP
=============

> id: '392dd88b-d2f5-4943-a993-01aaad7ccd32'
> slug:
> 	cs: cookies
> 	it: cookie-in-php
> 
> perex:
> 	- 'Cookies je krátká textová informace v paměti prohlížeče, kam můžeme v PHP zapisovat a označit si uživatele.'
> 	- Un cookie è un breve pezzo di informazione testuale nella memoria del browser dove possiamo scrivere e marcare l'utente in PHP.
> 
> publicationDate: '2019-09-11 10:18:29'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '78f9aad0278acd51b98179e7d29fe3d8'

> **Attenzione:** Questo articolo è stato scritto molti anni fa e alcune informazioni potrebbero essere obsolete o errate. Per favore, tenetelo a mente quando leggete.

I cookie sono piccoli pezzi di informazioni testuali memorizzati nel browser del visitatore di un sito web. Sono sempre trasferiti con ogni pagina che viene ricaricata, e possono essere cancellati, modificati e letti dall'utente in qualsiasi momento, quindi non sono adatti a memorizzare informazioni personali.

*Attenzione: se il tuo sito web utilizza cookie per tracciare gli utenti o componenti aggiuntivi di terze parti (ad esempio il pulsante "Mi piace" di Facebook, il contatore di traffico di Google Analytics, i banner pubblicitari), devi informare l'utente di questo.

> "Un'altra nota: il vostro sito non deve contenere pubblicità o codici di misurazione finché non avete ottenuto il consenso. E questo fa schifo".
>
> -- <a href="https://phpfashion.com/jak-na-souhlas-s-cookie-ve-zkurvene-eu">David Grudl</a>

Leggere un valore da un cookie
--------------------------

Tutti i cookie sono memorizzati nella variabile superglobale `$_COOKIE`, che memorizza ogni chiave come un array.

Per esempio, se abbiamo memorizzato il nome dell'utente attualmente connesso sotto la chiave `user` nel cookie, possiamo recuperarlo facilmente:

```php
echo $_COOKIE['utente'];
```

Attenzione: i cookie potrebbero non esistere sempre (per esempio, se sei un nuovo utente). Pertanto, dovremmo sempre controllare l'esistenza di cookie prima di qualsiasi annuncio e offrire un messaggio di errore alternativo, se necessario.

```php
if (isset($_COOKIE['utente']) && $_COOKIE['utente']) {
    echo 'Utente registrato:' . $_COOKIE['utente'];
} else {
    echo 'Nessuno si è registrato.';
}
```

Ottenere tutti i cookie disponibili
--------------------------------

Poiché tutti i cookie sono memorizzati nella variabile superglobale `$_COOKIE`, possono essere facilmente elencati:

```php
var_dump($_COOKIE);
```

O in alternativa, passare attraverso il ciclo e ottenere tutte le chiavi e i valori:

```php
foreach($_COOKIE as $key => $value) {
    echo $key . ':' . $value;	// scrivere la chiave e il valore
    echo '<br>';				// avvolgere la linea
}
```

Memorizzare il valore in un cookie
--------------------------

La funzione `setcookie()` è usata per memorizzare dati nei cookie.

Il primo parametro è la chiave del cookie, che viene usata per leggerlo dal campo `$_COOKIE`, e il secondo parametro è il dato stesso come stringa.

Con il terzo parametro possiamo (opzionalmente) impostare il periodo di validità per cui il cookie sarà disponibile. Il tempo di disponibilità è dato come <a href="/date">timestamp</a>, quindi se vogliamo impostare un cookie con una validità di 1 ora da questo momento, dobbiamo solo scrivere `time() + 3600`.

```php
$data = 'Alcuni contenuti che vogliamo conservare.';

setcookie('TestCookie', $data);
setcookie('TestCookie', $data, time() + 3600);
```

Memorizzazione di dati più grandi
-------------------

I cookie non sono adatti a memorizzare dati più grandi (i browser di solito permettono di memorizzare solo 4 kB e un massimo di 20 cookie, la dimensione include anche i nomi dei cookie, le impostazioni di validità, ecc.)

È meglio memorizzare dati più grandi sul server e mettere solo un identificatore nel cookie, con il quale possiamo dire a quale utente appartiene. Questo metodo è chiamato `$_SESSION` ed è discusso in un articolo separato.

Se non avete necessariamente bisogno di memorizzare i dati sempre in modo sincrono sul server, potete usare il **<a href="https://jecas.cz/localstorage">localstorage</a>** storage disponibile in javascript. La sua capacità è dell'ordine di MB e i dati non sono soggetti a scadenza.
