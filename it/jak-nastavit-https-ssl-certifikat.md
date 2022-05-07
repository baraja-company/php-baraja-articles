Come impostare un certificato HTTPS / SSL - guida completa
==========================================================

> id: '0346a091-2b2c-494f-b2fb-573796d4fb46'
> slug:
> 	cs: jak-nastavit-https-ssl-certifikat
> 	it: come-impostare-un-certificato-https-ssl---guida-completa
> 
> publicationDate: '2019-09-16 09:01:35'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '67b2b3ccce4815204a4bcadce781fa59'

Nel distribuire il protocollo `https` sui siti dei clienti, ho spesso incontrato varie difficoltà che derivavano da una mancanza di comprensione dei problemi e da un'eccessiva complessità dei concetti.

In questo tutorial descrivo in dettaglio i passi per ottenere e distribuire un certificato valido su un server web.

**In ogni sottotitolo, riassumo sempre brevemente il passo per gli utenti avanzati, e in fondo discuto i dettagli per i principianti.**

> **Attenzione:** L'intero processo di distribuzione di un certificato può richiedere più di un'ora ed è spesso intermittente (il sito potrebbe non essere disponibile).

Requisiti di ingresso
-----------------

Le istruzioni presuppongono che abbiamo accesso a un server web Terminal che gira su Linux e che utilizza Apache.

Per Nginx l'intera teoria si applica ugualmente, solo il collegamento del file del certificato è diverso.

## Connessione al server web

Ci connettiamo al server via SSH.

- Su Windows raccomando il programma **Putty**,
- Su Mac o Linux, basta usare il terminale integrato.

Su Mac o Linux, chiamate il comando:

```htaccess
ssh uživatel@server
```

Per esempio, voglio connettermi all'utente `root` sul sito `baraja.cz`:

```htaccess
ssh root@baraja.cz
```

O all'utente `jan` ad un indirizzo IP specifico:

```htaccess
ssh jan@127.0.0.1
```

Dopo aver inviato la tua richiesta, la connessione verrà effettuata direttamente o ti verrà chiesta una password. Non viene visualizzato nulla quando si digita la password, quindi confermate la password con il tasto invio e aspettate che la connessione venga autorizzata.

> **Attenzione:** Se non abbiamo diritti per un'azione, dobbiamo assegnarli. O si passa direttamente all'utente `root` con il comando `sudo su`, o si fa precedere il comando che vogliamo eseguire sotto root dalla parola `root` all'inizio, per esempio `root rm <name>` sotto root cancellerà il file `<name>`. Quando si usa il comando `sudo`, è possibile che periodicamente ci venga richiesta una password.

**Dettagli:**

L'accesso SSH è impostato dal particolare hosting dove avete affittato un server.

- In caso di VPS avrete sempre l'accesso SSH.
- Nel caso dell'hosting, potresti non avere SSH per niente, e la configurazione è fatta in modo diverso (di solito attraverso l'interfaccia web, o contattare il supporto).

## Passare da HTTP a HTTPS

Se stai convertendo un sito esistente da `http` a `https`, devi garantire che tutto il traffico sia reindirizzato al nuovo protocollo `https`.

Nel caso di Apache, questo può essere facilmente ottenuto utilizzando un reindirizzamento nel file `.htaccess`:

```htaccess
<IfModule mod_rewrite.c>
	RewriteEngine On

	RewriteCond %{HTTPS} !on
	RewriteRule .? https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```

Questa configurazione assicurerà che tutte le richieste a `http` saranno reindirizzate con `HTTP code 301` a `https`. Questa configurazione è quella predefinita per il framework Nette, ma si applica anche a tutti gli altri casi.

**Dettagli:**

Il file `.htaccess` contiene la configurazione specifica del server web che influenza ogni richiesta. Di solito si trova nella stessa directory di `index.php`, o in altri file che sono accessibili da Internet.

La sua impostazione è valida solo per il server Apache e può essere disabilitata o limitata per alcuni host. Per informazioni più dettagliate, contattate sempre l'hosting dove ospitate il vostro sito.

-----

A volte succede che `.htaccess` non vuole cooperare bene e la configurazione è estremamente difficile (per esempio per molti domini, ognuno dei quali si comporta in modo diverso). In tal caso, il reindirizzamento a HTTPS può essere gestito direttamente in PHP in un pizzico:

```php
if (empty($_SERVER['HTTPS']) || $_SERVER['HTTPS'] === 'off') {
	$location = 'https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'];
	header('HTTP/1.1 301 Spostato in modo permanente');
	header('Posizione:' . preg_replace('/^(https:\/\/(?:www\.)(.*)$/', '$1$2', $location));
	die;
}
```

Metti lo script in `index.php`. Tuttavia, non consiglio questa soluzione.

## Trova i file con host virtuali

Nel caso di Apache, dobbiamo trovare il file Virtual hosts.

Di solito si trovano nel percorso `/etc/apache2/sites-available`.

Elenca il contenuto della directory con il comando `ls -al` e trova il file dove è impostato il virtuale per il nostro sito.

I VirtualHost di solito si trovano in file con estensione `.conf`. Il default è spesso in `000-default.conf`.

**Dettagli:**

- La directory viene aperta con il comando `cd`, per esempio `cd /etc/apache2/sites-available`.
- Il contenuto del file viene scritto nel terminale usando il comando `cat <nome>`, o modificato usando i comandi `nano <nome>` o `vim <nome>`.
- Di solito si accede all'editor usando la scorciatoia da tastiera `CTRL + X` o premendo due volte `ESC`.
- I file possono essere parzialmente sfogliati nella GUI con Midnight commander (comando `mc`), che nel caso di Ubuntu è installato semplicemente con `apt-get install mc` o `sudo apt-get install mc`.

## Impostare un host virtuale per la porta 443

All'interno dell'host virtuale dobbiamo prepararne uno nuovo per la porta `443` (un problema comune può essere il blocco della porta 443 sul firewall).

Il contenuto del file potrebbe essere come questo, per esempio:

```htaccess
<IfModule mod_ssl.c>
     <VirtualHost *:443>
         ServerName baraja.cz

         ServerAdmin jan@barasek.com
         DocumentRoot /var/www/baraja.cz/www
         Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains; preload"
         ErrorLog ${APACHE_LOG_DIR}/baraja.cz.ssl.error.log
         CustomLog ${APACHE_LOG_DIR}/baraja.cz.ssl.access.log combined
         SSLEngine on
         SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
         SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
         SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
         SSLProtocol             all -SSLv3
         SSLCipherSuite          ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS
         SSLHonorCipherOrder     on
         SSLCompression          off
         <FilesMatch "\.(cgi|shtml|phtml|php)$">
                 SSLOptions +StdEnvVars
         </FilesMatch>
         <Directory /usr/lib/cgi-bin>
                 SSLOptions +StdEnvVars
         </Directory>
		 BrowserMatch "MSIE [2-6]" nokeepalive ssl-unclean-shutdown downgrade-1.0 force-response-1.0
         # MSIE 7 and newer should be able to use keepalive
         BrowserMatch "MSIE [17-9]" \
         ssl-unclean-shutdown
     </VirtualHost>
</IfModule>
```

Sul file è l'area più importante:

```htaccess
SSLEngine on
SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
```

La comprensione di questa configurazione è assolutamente critica. Se hai bisogno di cercare su Google informazioni più dettagliate, usa le parole `SSLCertificateFile`, `SSLCertificateKeyFile` e `SSLCertificateChainFile` che sono comuni a tutti i server Apache.

> **Attenzione:** Il problema SSL è relativamente vecchio e spesso si usano nomi diversi per la stessa cosa per mantenere la compatibilità all'indietro! È quindi importante capire il principio.

**Dettagli:**

È importante che VirualHost contenga:

- `<IfModule mod_ssl.c>` dice che vogliamo usare SSL
- `<VirtualHost *:443>` che tutte le comunicazioni saranno eseguite sulla porta 443
- `SSLEngine on` che SSL è abilitato per questo VirtualHost
- I file `SSLCertificateFile`, `SSLCertificateKeyFile` e `SSLCertificateChainFile` sono file con chiavi specifiche.

Non c'è bisogno di altro.

## Capire il principio di ciò che stiamo per fare e come funziona il certificato

Nella configurazione finale di VirtualHost, avremo davvero bisogno solo di 3 file `SSLCertificateFile`, `SSLCertificateKeyFile` e `SSLCertificateChainFile`, che possiamo mettere ovunque sul server (nome e posizione non hanno importanza). È importante fornire un percorso di lavoro e che il contenuto dei file sia valido.

Il metodo specifico per ottenere i certificati può variare da CA a CA. L'importante è capire il principio e poi applicarlo al proprio caso.

| File - Significato -
|---------------------------|-------------------------------------|
| Questo certificato **è inviato dall'autorità**.
| `SSLCertificateKeyFile` | **La mia chiave privata generata** |
| `SSLCertificateChainFile` | Ho scaricato dal web tipo **intermedio + root** |

## Ottenere la chiave privata e richiedere il certificato

Sul server, generiamo prima la chiave privata. La parola **privato** è importante, significa che nessun altro lo conosce tranne il server web. Idealmente, non dovrebbe mai lasciare il server e dovrebbe essere collocato in un luogo sicuro. Perdere questa chiave significa una perdita di sicurezza, poiché un attaccante sarà in grado di impersonare un server specifico.

Per generare la chiave, usate il comando:

```shell
openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.key -out yourdomain.csr
```

Per generarlo, dobbiamo avere il programma `openssl` installato sul server, che possiamo ottenere per esempio eseguendo `sudo apt install openssl`.

Ci possono essere diversi tipi di chiavi, in questo caso generiamo una chiave RSA lunga 2048 byte (`rsa:2048`).

L'output del comando è costituito da 2 file (che nominerete in base al vostro dominio):

- `yourdomain.key` - questa è la chiave privata. Salvare il percorso di questa chiave in Apache VirtualHost in `SSLCertificateKeyFile`.
- `yourdomain.csr` - questa è una `richiesta di certificato`, o una richiesta di emissione di un certificato.

Il contenuto della richiesta deve sempre essere presentato all'AC per l'approvazione. Questo si fa di solito tramite l'interfaccia web nell'amministrazione del sito dove si vendono i certificati. L'approvazione della richiesta varia a seconda del tipo di certificato. Il più delle volte è fatto automaticamente da un robot e richiede da 5 minuti a 8 ore. Nel caso di certificati costosi, dove si verifica anche la proprietà fisica del sito e della società operativa, la verifica viene fatta manualmente e può richiedere diversi giorni.

Se hai fretta, c'è un'agenzia di certificazione `Let's encrypt` che autorizza le richieste automaticamente con una validità di 3 mesi.

> **Attenzione:** In alcuni casi, la CA offre la generazione automatica di richieste. Questa non è una pratica raccomandata perché conosce la chiave privata. Se lo fate, dovete sempre ottenere la chiave privata dall'agenzia e metterla in un file sul server come se steste generando la richiesta.

## Ottenere una chiave per una specifica CA/autorità di sicurezza

Nel frattempo, in attesa che la richiesta venga approvata e che il certificato venga rilasciato, ci assicuriamo la **chiave pubblica** della CA. In Apache VirtualHost questo è rappresentato dal file `SSLCertificateChainFile` (in inglese è chiamato `Intermediate and Root CA`). Questa chiave è pubblica in linea di principio e dice al browser web chi è la CA. Questo file è di solito scaricato dal sito web della CA o ci viene consegnato in una e-mail.

Dovreste sempre scaricare la chiave corretta secondo l'algoritmo che scegliete. Nel caso di RapidSSL, per esempio, può essere scaricato qui: https://knowledge.digicert.com/generalinformation/INFO1548.html

## Ottenere il certificato

Sulla base della richiesta, l'autorità di certificazione ci ha rilasciato un certificato. Nel caso di Apache VirtualHost, è rappresentato dal file `SSLCertificateFile`.

È importante che il certificato abbia una validità limitata e dobbiamo ripetere questo processo (idealmente prima della scadenza). Puoi trovare facilmente la data di scadenza in Chrome cliccando sul lucchetto verde nel browser e visualizzando i dettagli del certificato, nel caso sia comunicato dalla CA.

Dopo la data di scadenza, il certificato non è più valido e il sito si rifiuta di connettersi e gli utenti ricevono un messaggio di errore sulla violazione della sicurezza.

## Controlla e apporta modifiche

La correttezza delle impostazioni di Apache può essere parzialmente verificata con il comando `apache2ctl -S`.

Affinché le modifiche abbiano effetto, Apache deve essere riavviato, per esempio con il comando

```shell
sudo service apache2 restart
```

Se non viene lanciato nessun messaggio di errore, andiamo subito a verificare la funzionalità tramite un browser web (consiglio Google Chrome, che mostra il maggior numero di dettagli).

In caso di errore, chiamiamo il comando `apache2ctl -S`, che può mostrare il percorso dei log, dove possiamo vedere approssimativamente cosa è sbagliato. È importante fare tutti i passi di questo manuale nell'ordine corretto e non scambiare nessuna delle chiavi.

## Supporto futuro

Non dimenticare di segnare la data di scadenza sul tuo calendario in modo da poter rinnovare il tuo certificato in tempo. Alcune CA inviano un'e-mail di notifica, ma questo non è sempre affidabile.

Puoi fare il processo di rinnovo e ottenere un nuovo certificato in parallelo mentre quello attuale è in esecuzione, e poi semplicemente sostituirlo allo stesso tempo.

Per il rinnovo automatico, consiglio [Certbot](https://certbot.eff.org), che può monitorare automaticamente la validità e rinnovare i certificati. Il rinnovo è fatto, per esempio, da un cron che chiama un comando una volta al mese di notte per emettere un nuovo certificato e lo distribuisce immediatamente.

Se non capite la gestione dei certificati, è una buona idea ospitare con un'azienda affermata che vi fornirà un hosting funzionale, incluso un certificato. Questo vi farà risparmiare un sacco di problemi.
