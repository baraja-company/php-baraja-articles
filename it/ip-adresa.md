Ottenere l'indirizzo IP dell'utente in PHP
==========================================

> id: '1d6d761e-c139-4624-8c32-b0f2c131a831'
> slug:
> 	cs: ip-adresa
> 	it: ottenere-l-indirizzo-ip-dell-utente-in-php
> 
> perex:
> 	- 'Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem.'
> 	- 'Ottenere l''indirizzo IP dell''utente in PHP, memorizzare l''indirizzo IP e bannare l''utente. Indagine su VPN e utente dietro Proxy o NAT.'
> 
> publicationDate: '2020-02-28 10:30:21'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '54a77b5e4cc431b354903e42a27682a6'

In PHP, è molto facile rilevare un indirizzo IP ad un livello base:

```php
echo 'Sai, il tuo indirizzo IP è' . $_SERVER['INDIRIZZO REMOTO'] . '?';
```

> **Attenzione:** Ottenere l'indirizzo IP come chiave del campo `$_SERVER['REMOTE_ADDR']` è possibile solo se PHP è stato chiamato dal browser. In modalità CLI (per esempio, in esecuzione da terminale con cron), l'indirizzo IP non è disponibile (questo ha senso, dato che non viene fatta alcuna richiesta di rete).

Rilevamento affidabile dell'indirizzo IP
-----------------------------

Dopo molti anni di sviluppo, alla fine sono rimasto con questa implementazione:

```php
function getIp(): string
{
    if (isset($_SERVER['HTTP_CF_CONNECTING_IP'])) { // Supporto Cloudflare
        $ip = $_SERVER['HTTP_CF_CONNECTING_IP'];
    } elseif (isset($_SERVER['INDIRIZZO REMOTO']) === true) {
        $ip = $_SERVER['INDIRIZZO REMOTO'];
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
echo 'Sai, il tuo indirizzo IP è' . getIp() . '?';
```

Se l'IP può essere rilevato direttamente, o è solo IPv6, o è in modalità CLI (ad esempio cron), restituisce `127.0.0.1` (localhost).

Le implementazioni che tengono conto degli header `X-Forwarded-For` e `X-Real-IP` sono estremamente pericolose direttamente in PHP, perché i dati possono essere facilmente modificati e un attaccante può spoofare un falso indirizzo IP per, ad esempio, visualizzare l'amministrazione o attivare la modalità Debug del sito (Nette Tracy). D'altra parte, dobbiamo accettare alcune richieste proxy (per esempio, quando proxiamo il traffico attraverso Cloudflare, o quando si eseguono Apache e Ngnix sulla stessa macchina, quando sono chiamati localmente subito dopo l'altro).

Nel caso di accesso diretto dell'utente al server, c'è solo una soluzione corretta, ed è quella di assicurarsi su Apache (tramite l'estensione `RemoteIP`) e su Nginx tramite l'estensione `remote_ip` che `X-Forwarded-For` sia impostato dall'effettivo indirizzo IP del visitatore, e che l'indirizzo IP non possa essere impostato con un header HTTP.

Il campo `$_SERVER['REMOTE_ADDR']` ottiene automaticamente l'indirizzo IP corretto (cioè l'indirizzo IP da cui la richiesta è arrivata direttamente a PHP) e non dobbiamo occuparcene.

Accesso degli utenti tramite proxy
----------------------------

Succede spesso che un utente acceda attraverso un proxy. Poi l'indirizzo IP effettivo è memorizzato nella variabile `$_SERVER['HTTP_X_FORWARDED_FOR']`.

Questo caso può verificarsi, per esempio, quando il routing sul server è risolto usando il metodo `Ngnix -> Apache -> PHP`, dove `Ngnix` serve come reverse proxy prima di `Apache`. In questo caso, PHP vede solo l'indirizzo IP all'interno della rete interna (di solito della forma `127.0.0.*`).

Per esempio, il servizio **Cloudflare** può comportarsi in questo modo, e bisogna fare attenzione se stiamo lavorando con l'indirizzo IP dell'utente reale o del proxy. Per me, il modo migliore è usare la funzione `getIp()` menzionata all'inizio di questo articolo. Possiamo assicurare il rilevamento di Cloudflare verificando l'esistenza della chiave `$_SERVER['HTTP_CF_CONNECTING_IP']`, che viene trasmessa automaticamente in ogni richiesta proxy.

Memorizzare l'indirizzo IP
------------------

Dipende dall'indirizzo IP che avete a disposizione.

- L'indirizzo IPv4 può essere memorizzato in 4 byte, la funzione `ip2long` è usata per questo,
- Tuttavia, per un indirizzo IP IPv6 dobbiamo usare 16 byte e non c'è una funzione di conversione.

Se il tuo server di database non supporta direttamente un tipo di dati per l'indirizzo IP, ti consiglio di memorizzare l'indirizzo IP come `varchar(39)`, dove entrambe le versioni si adattano a una stringa e sono leggibili.

> Quando si memorizza l'indirizzo IP, considerare se ha senso memorizzare anche il nome di dominio rilevato dalla funzione `gethostbyaddr`. Quando si elencano e si cercano, non si possono scoprire i nomi perché ci vuole molto tempo e possono cambiare nel tempo.

Blocco dell'indirizzo IP del visitatore
-----------------------------

La soluzione ideale è quella di creare una lista di indirizzi IP bloccati e confrontare questa lista con l'indirizzo IP corrente su ogni richiesta. Se gli indirizzi corrispondono, la richiesta sarà fermata immediatamente.

```php
$blackList = [
    'first-ip',
    'druha-ip',
];

if (\in_array(getIp(), $blackList, true) === true) {
    echo 'Purtroppo il tuo indirizzo IP è bloccato :-(';
    die; // Richiesta di uscita
}
```

L'esempio presuppone un'implementazione della funzione `getIp()` come nell'esempio precedente.

Una soluzione più potente è controllare l'occorrenza dell'indice nell'array:

```php
$blackList = [
    'first-ip' => true,
    'druha-ip' => true,
];

if (isset($blackList[getIp()]) === true) {
    echo 'Purtroppo il tuo indirizzo IP è bloccato :-(';
    die; // Richiesta di uscita
}
```

Indirizzo IP e nome del server
---------------------------------

L'indirizzo IP del server è solitamente memorizzato nel campo `$_SERVER['SERVER_ADDR']` e il suo nome può essere ottenuto con il costrutto `gethostbyaddr($_SERVER['SERVER_ADDR'])`.

Tuttavia, se il concetto `Ngnix -> Apache -> PHP` è usato e `Ngnix` è nel ruolo di un reverse proxy, il vero indirizzo IP del server non viene visualizzato.

In questo caso, il nome del server può essere trovato nel campo `$_SERVER['SERVER_NAME']`, o usando la funzione `php_uname('n')`. [Documentazione ufficiale della funzione uname](https://www.php.net/manual/en/function.php-uname.php).

Possiamo quindi usare questo trucco per trovare l'indirizzo IP pubblico del server: `gethostbyname(php_uname('n'))`.
