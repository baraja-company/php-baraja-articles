Invio di email (funzioni mail() e SMTP) in PHP
==============================================

> id: '3051b5ea-9408-4aaa-8209-e3c527ef8ad2'
> slug:
> 	cs: odesilani-emailu-mail-smtp
> 	it: invio-di-email-funzioni-mail-e-smtp-in-php
> 
> perex:
> 	- 'Možnosti odesílání e-mailů v PHP, funkce mail(), SMTP, hlavičky, konfigurace a Nette Mailer.'
> 	- 'Opzioni di invio e-mail in PHP, mail(), SMTP, intestazioni, configurazione e Nette Mailer.'
> 
> publicationDate: '2019-11-26 12:00:02'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '53dc8075e16053ea9284299987c64952'

In PHP, abbiamo fondamentalmente 2 modi per inviare e-mail:

- La funzione nativa `mail()`, che ha parecchie limitazioni,
- o tramite un server SMTP.

La funzione `mail()` deve usare il server SMTP, che è un modo molto semplice per inviare posta attraverso il server SMTP.
---------------

L'idea di usare questo è semplice: si chiama la funzione:

```php
mail('jan@barasek.com', 'Oggetto', 'Il testo del messaggio...');
```

E PHP farà l'invio da solo.

Internamente, l'invio funziona leggendo la configurazione da `php.ini` e cercando il server SMTP predefinito attraverso cui consegnare la posta. Quindi questo richiede una configurazione preliminare del server web.

La principale insidia della funzione `mail()` è che il programmatore deve capire tutta la logica da solo. Questo implica, per esempio, buttare via le intestazioni sulla crittografia, collegare i certificati per crittografare i messaggi, e così via.

Nel caso di un fallimento dell'invio, viene restituito un valore `falso`, che dobbiamo catturare ed elaborare noi stessi. Possiamo scoprire l'errore specifico in modo limitato chiamando `error_get_last()`, così per esempio:

```php
if (@mail($to, $subject, $message) === false) {
	throw new \Exception(
		'Non può inviare posta:'
		. (@error_get_last()['messaggio'] ?? '')
	);
}
```

> **TIP:** Notate che non abbiamo specificato l'indirizzo da cui vogliamo inviare la posta e la codifica da usare.
>
> Tutte queste impostazioni devono essere passate attraverso le intestazioni.

Se hai ancora bisogno di usare la funzione `mail()` (per esempio, a causa dell'hosting), ti consiglio di usare il pacchetto `nette/mail` e il servizio `SendmailMailer`, che gestisce bene l'invio della posta.

Server SMTP
-----------

SMTP sta per `Simple Mail Transfer Protocol`, che (come vedrete presto) è molto vero.

SMTP, a differenza di `mail()`, è un protocollo più avanzato con opzioni di configurazione avanzate non solo dal lato PHP, ma anche direttamente sul server di posta.

Il supporto SMTP sugli host è eccellente nel 2018.

SMTP funziona fondamentalmente con PHP che prima stabilisce una connessione al server SMTP (richiede l'estensione `php_openssl.dll` in PHP, che probabilmente avete già attiva), si autentica (verificando che le credenziali di accesso siano corrette) durante la connessione, e poi possiamo comunicare con il server in modo simile a un database - cioè inviare richieste individuali, ma mantenere una singola connessione in ogni momento. Un grande vantaggio di SMTP è il supporto diretto per la crittografia (noto come `TLS`).

Invio di email da localhost - una soluzione semplice
--------------------------------------------------

Ho spesso bisogno di inviare email da localhost quando sto testando un'applicazione appena scritta.

> **Per consiglio:**
>
> Su un Mac, la situazione è semplice perché il server MAMP in qualche modo "magicamente" trova l'account Apple Mail attualmente connesso e i messaggi sono sempre inviati dall'account corrente.

Tuttavia, non si può sempre fare affidamento su questo comportamento ed è una buona idea impostare la propria soluzione. Se hai una connessione internet e un account Google, è molto facile usare un account Gmail a cui puoi collegarti direttamente da PHP e inviare posta attraverso di esso.

Se usi il pacchetto `nette/mail`, la configurazione è semplice:

```neon
mail:
	smtp: true
	host: smtp.gmail.com
	username: janbarasek@gmail.com
	password: *********
	secure: ssl
```

> La password non è la password di accesso al tuo account (sarebbe insicuro e non potresti usare l'autenticazione a due fattori, per esempio).
>
> È necessario utilizzare ciò che viene chiamato "password dell'applicazione", che implementalmente significa che si <a href="https://myaccount.google.com/apppasswords">registra la tua applicazione</a> direttamente nel tuo account Google, a cui viene assegnata una qualche password generata casualmente che si inserisce in PHP e può essere inviata attraverso.
>
> Le istruzioni dettagliate sono <a href="https://support.google.com/accounts/answer/185833?hl=cs">sul sito web di Google</a>.

Configurare la posta su Wedos
---------------------------

Si possono inviare solo 500 email al giorno attraverso l'hosting Wedos, e ho lottato con la connessione SMTP per un po'.

Attraverso il pacchetto `nette/mail` si fa così (una soluzione praticabile):

```neon
mail:
	smtp: true
	host: smtp-*******.wedos.net
	username: jan@barasek.com
	password: ******
	secure: tls
	port: 587
```

Il parametro `host` è diverso per ogni hosting e si trova nell'e-mail che Wedos invia quando registrate l'hosting.

Il "nome utente" rappresenta la casella di posta elettronica da cui saranno inviati i messaggi. La cassetta postale deve esistere. Quando si invia posta in PHP, dobbiamo anche impostare l'invio allo stesso indirizzo (in Nette con il metodo `->setFrom()`).

Se non compiliamo la configurazione in modo accurato e corretto, verranno lanciati vari messaggi di errore e le mail non potranno essere inviate.

Quando il numero di messaggi inviati viene superato, viene lanciata un'eccezione che informa del superamento del limite.
