Ottenere l'indirizzo IP dell'utente in PHP
==========================================

> id: '1d6d761e-c139-4624-8c32-b0f2c131a831'
> slug:
> 	cs: ip-adresa
> 	it: ottenere-l-indirizzo-ip-dell-utente-in-php
> 
> perex:
> 	- 'Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem.'
> 	- 'Ottenere l''indirizzo IP dell''utente in PHP, memorizzare l''indirizzo IP e vietare l''utente. Indagine su VPN e utenti dietro Proxy o NAT.'
> 
> publicationDate: '2020-02-28 10:30:21'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '284c4642c3fa98a026ce5a9e6625bb16'

In PHP è molto facile individuare un indirizzo IP a livello di base:

```php
echo 'Il vostro indirizzo IP è' . $_SERVER['INDIRIZZO_REMOTO'] . '?';
```

> **Attenzione:** Ottenere l'indirizzo IP come chiave del campo `$_SERVER['REMOTE_ADDR']` è possibile solo se PHP è stato chiamato dal browser. In modalità CLI (ad esempio, esecuzione da Terminale con cron), l'indirizzo IP non è disponibile (il che ha senso, dato che non viene effettuata alcuna richiesta di rete).

Rilevamento affidabile dell'indirizzo IP
-----------------------------

Dopo molti anni di sviluppo, alla fine ho scelto questa implementazione:

```php
function getIp(): string
{
    if (isset($_SERVER['HTTP_CF_CONNECTING_IP'])) { // Supporto Cloudflare
        $ip = $_SERVER['HTTP_CF_CONNECTING_IP'];
    } elseif (isset($_SERVER['INDIRIZZO_REMOTO']) === true) {
        $ip = $_SERVER['INDIRIZZO_REMOTO'];
        if (preg_match('/^(?:127|10)\.0\.0\.[12]?\d{1,2}$/', $ip)) {
            if (isset($_SERVER['HTTP_X_REAL_IP'])) {
                $ip = $_SERVER['HTTP_X_REAL_IP'];
            } elseif (isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
                $ip = $_SERVER['HTTP_X_FORWARDED_FOR'];
            }
        }
    } else {
        $ip = '127.0.0.1';
    }
    if (in_array($ip, ['::1', '0.0.0.0', 'localhost'], true)) {
        $ip = '127.0.0.1';
    }
    $filter = filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV4);
    if ($filter === false) {
        $ip = '127.0.0.1';
    }

    return $ip;
}
```

Molto meglio, allora:

```php
echo 'Il vostro indirizzo IP è' . getIp() . '?';
```

Se l'IP può essere rilevato direttamente, o è solo IPv6, o è in modalità CLI (ad esempio cron), restituisce `127.0.0.1` (localhost).

Le implementazioni che tengono conto degli header `X-Forwarded-For` e `X-Real-IP` sono estremamente pericolose direttamente in PHP, perché i dati possono essere facilmente modificati e un aggressore può creare un falso indirizzo IP per, ad esempio, visualizzare l'amministrazione o attivare la modalità Debug del sito (Nette Tracy). D'altra parte, dobbiamo accettare alcune richieste proxy (ad esempio, quando il traffico proxy passa attraverso Cloudflare, o quando Apache e Ngnix sono in esecuzione sulla stessa macchina, quando vengono chiamati localmente subito dopo l'altro).

Nel caso di accesso diretto dell'utente al server, c'è solo una soluzione corretta, ovvero assicurarsi su Apache (tramite l'estensione `RemoteIP`) e su Nginx tramite l'estensione `remote_ip` che `X-Forwarded-For` sia impostato dall'indirizzo IP effettivo del visitatore e che l'indirizzo IP non possa essere impostato con un'intestazione HTTP.

Il campo `$_SERVER['REMOTE_ADDR']` ottiene automaticamente l'indirizzo IP corretto (cioè l'indirizzo IP da cui la richiesta è arrivata direttamente a PHP) e non dobbiamo occuparcene.

Accesso utente tramite proxy
----------------------------

Spesso accade che un utente acceda attraverso un proxy. Quindi l'indirizzo IP effettivo viene memorizzato nella variabile `$_SERVER['HTTP_X_FORWARDED_FOR']`.

Questo caso può verificarsi, ad esempio, quando l'instradamento sul server viene risolto utilizzando il metodo `Ngnix -> Apache -> PHP`, dove `Ngnix` funge da reverse proxy prima di `Apache`. In questo caso, PHP vede solo l'indirizzo IP della rete interna (di solito della forma `127.0.0.*`).

Ad esempio, il servizio **Cloudflare** può comportarsi in questo modo e occorre prestare attenzione se si sta lavorando con l'indirizzo IP dell'utente effettivo o del proxy. Per me, il modo migliore è usare la funzione `getIp()` menzionata all'inizio di questo articolo. Possiamo garantire il rilevamento di Cloudflare verificando l'esistenza della chiave `$_SERVER['HTTP_CF_CONNECTING_IP']`, che viene trasmessa automaticamente in ogni richiesta proxy.

Rilevamento VPN / Proxy
-------------------

Non esiste un rilevamento affidabile dell'utilizzo di proxy o VPN, ma in un ambiente reale è possibile filtrare almeno una parte del traffico.

Ci sono diversi modi per farlo: Prendere un intervallo di indirizzi IP e confrontare l'indirizzo IP da cui proviene la richiesta.

Da alcuni fornitori di VPN, gli elenchi di indirizzi IP sono disponibili in modo non ufficiale (si veda ad esempio https://gist.github.com/JamoCA/eedaf4f7cce1cb0aeb5c1039af35f0b7), nel caso dei nodi di uscita Tor in modo ufficiale (https://blog.torproject.org/changes-tor-exit-list-service, ma i ponti Tor non ci sono).

Un'altra opzione è quella di fare una richiesta online da qualche parte, che può sia ritardare il caricamento della pagina se il servizio non è funzionante, sia "trapelare" gli indirizzi IP dei visitatori a una terza parte. A partire dal 2023, sconsiglio vivamente questo approccio, in quanto inizia a essere più orientato alla protezione e alla manipolazione dei dati degli utenti.

Questa interrogazione online può essere "ingenua" e basta vedere chi possiede l'intervallo o se si tratta di un proxy/VPN (alcuni servizi possono restituirlo, ma di default non fa parte delle "informazioni IP", ad esempio da un servizio whois).

(La maggior parte) delle volte viene utilizzata una sorta di classificazione della reputazione, in cui alcuni indirizzi IP "cestinano" più di altri. Statisticamente, ci sono più schifezze provenienti da vari proxy, VPN e Tor che da indirizzi IP domestici (tranne forse gli indirizzi IP domestici "infetti"). Tale valutazione reputazionale è offerta da alcune liste di blocco DNS, come ad esempio https://en.m.wikipedia.org/wiki/Comparison_of_DNS_blacklists e la colonna "Listing goal", oppure è fornita direttamente da aziende come Cloudflare sotto forma di "bot management", ecc.

Molto dipende dall'obiettivo del rilevamento.

Memorizzazione dell'indirizzo IP
------------------

Dipende dall'indirizzo IP disponibile.

- L'indirizzo IPv4 può essere memorizzato in 4 byte; a tale scopo si utilizza la funzione `ip2long`,
- Tuttavia, per un indirizzo IPv6 dobbiamo utilizzare 16 byte e non esiste una funzione di conversione.

Se il server del database non supporta direttamente un tipo di dati per l'indirizzo IP, si consiglia di memorizzare l'indirizzo IP come `varchar(39)`, in modo che entrambe le versioni siano una stringa e siano leggibili.

> Quando si memorizza l'indirizzo IP, si consideri se ha senso memorizzare anche il nome di dominio rilevato dalla funzione `gethostbyaddr`. Non è possibile scoprire i nomi durante la quotazione e la ricerca, perché i tempi sono molto lunghi e possono cambiare nel tempo.

Blocco dell'indirizzo IP del visitatore
-----------------------------

La soluzione ideale è creare un elenco di indirizzi IP bloccati e confrontare questo elenco con l'indirizzo IP corrente a ogni richiesta. Se gli indirizzi corrispondono, la richiesta viene interrotta immediatamente.

```php
$blackList = [
    'primo-ip',
    'druha-ip',
];

if (\in_array(getIp(), $blackList, true) === true) {
    echo 'Purtroppo il tuo indirizzo ip è bloccato :-(';
    die; // Richiesta di uscita
}
```

L'esempio presuppone un'implementazione della funzione `getIp()` come nell'esempio precedente.

Una soluzione più efficace consiste nel verificare la presenza dell'indice nell'array:

```php
$blackList = [
    'primo-ip' => true,
    'druha-ip' => true,
];

if (isset($blackList[getIp()]) === true) {
    echo 'Purtroppo il tuo indirizzo ip è bloccato :-(';
    die; // Richiesta di uscita
}
```

Indirizzo IP del server e nome del server
---------------------------------

L'indirizzo IP del server è solitamente memorizzato nel campo `$_SERVER['SERVER_ADDR']` e il suo nome può essere ottenuto con il costrutto `gethostbyaddr($_SERVER['SERVER_ADDR'])`.

Tuttavia, se si utilizza il concetto `Ngnix -> Apache -> PHP` e `Ngnix` svolge il ruolo di reverse proxy, il vero indirizzo IP del server non viene visualizzato.

In questo caso, il nome del server può essere trovato nel campo `$_SERVER['SERVER_NAME']` o utilizzando la funzione `php_uname('n')`. [Documentazione ufficiale della funzione uname](https://www.php.net/manual/en/function.php-uname.php).

Si può quindi usare questo trucco per trovare l'indirizzo IP pubblico del server: `gethostbyname(php_uname('n'))`.
