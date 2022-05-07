PHP e informazioni sulla configurazione del server (phpinfo(), php.ini)
=======================================================================

> id: '6f4a78c1-ca06-4619-9a28-a716a65a12a8'
> slug:
> 	cs: info
> 	it: php-e-informazioni-sulla-configurazione-del-server-phpinfo-php.ini
> 
> perex:
> 	- 'PHP info, informace o nastavení a konfiguraci webového serveru. Změna konfigurace přes php.ini'
> 	- 'PHP info, informazioni sul setup e la configurazione del web server. Cambiare la configurazione tramite php.ini'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '9ad0961571b298fb1f30ea3e67b2973e'

Spesso abbiamo bisogno di scoprire quante più informazioni possibili sul server, la funzione nativa `phpinfo()` è ottima per questo:

```php
phpinfo();

die;	// dopo aver scritto la configurazione, uscire dallo script
```

Questo rende facile vedere la versione installata, le estensioni, le librerie e molto altro.

> Vedi la fine di questo articolo per informazioni sulla configurazione e la modifica delle impostazioni.

Trovare una sezione di configurazione specifica
-------------------------------------

A volte è utile elencare solo informazioni specifiche, quindi possiamo impostare il primo parametro per specificare esattamente ciò che ci interessa:

```php
phpinfo(INFO_MODULES);
```

Le costanti predefinite sono utilizzate per l'impostazione:

| Nome della costante | Valore | Descrizione
|-------------------|-----------|------
| INFO_GENERAL | 1 | Configurazione generale, posizione di php.ini, data dell'ultimo aggiornamento, Web Server, informazioni di sistema e altro.
| INFO_CREDITS | 2 | Crediti PHP, per saperne di più vedi `phpcredits()`.
| INFO_CONFIGURAZIONE| 4 | Direttive di posizione e impostazioni attuali. Maggiori informazioni saranno fornite dalla funzione `ini_get()`.
| INFO_MODULES | 8 | Informazioni sui moduli installati. Per maggiori informazioni, vedere la funzione `get_loaded_extensions()`.
| INFO_ENVIRONMENT | 16 | Informazioni sulla variabile `Environment`, disponibile come `$_ENV`.
| INFO_VARIABLES | 32 | Una panoramica delle impostazioni delle variabili superglobali, note come `EGPCS` (Environment, GET, POST, Cookie, Server).
| INFO_LICENSE | 64 | Informazioni sulla licenza d'uso di PHP e altri termini d'uso.
| INFO_ALL | -1 | Visualizza tutte le informazioni (valore predefinito)

Variabile superglobale `$_SERVER`.
---------------------------------

Possiamo scoprire un bel po' di informazioni sulle impostazioni del server direttamente mentre lo script è in esecuzione (ad esempio l'e-mail al webmaster, l'indirizzo IP corrente del visitatore o l'URL attualmente chiamato).

Elencare tutti i valori esistenti è facile:

```php
foreach ($_SERVER as $key => $value) {
    echo $key . ':' . $value . '<br>';
}
```

> **Attenzione:** Non tutti gli indici devono esistere (per esempio, se lo script esegue cron in modalità CLI, l'indice con l'URL della pagina o l'indirizzo IP della richiesta non esisterà).

Lettura di direttive di configurazione specifiche
-----------------------------------------

Gran parte della configurazione è memorizzata in `php.ini` e non è direttamente accessibile da PHP nel modo normale. Per esempio, la dimensione massima del file da caricare.

Per leggere direttamente la configurazione, usate la funzione `ini_get()` (nota: questa funzione potrebbe non essere abilitata su tutti i server, questo è particolarmente vero per gli host).

Per esempio, se vogliamo scoprire la dimensione massima del file che possiamo caricare, dobbiamo scrivere la nostra implementazione:

```php
/**
 * @autore Jan Barášek
 */
public static function getMaxUploadFileSize(): int
{
    $maxUpload = min(
        ini_get('dimensione massima del post'),
        ini_get('upload_max_filesize')
    );

    if (strncmp($maxUpload, 'M', 1) === 0) {
        return (int) str_replace('M', '', $maxUpload);
    }

    return (int) $maxUpload;
}
```

Restituisce il valore massimo di `upload_max_filesize` e `post_max_size` in MB.

Configurare il server e cambiare le impostazioni
-------------------------------------

Le impostazioni stesse sono memorizzate nel file `php.ini`. La sua posizione può essere facilmente trovata usando la funzione `phpinfo()` o chiamando il comando `php --ini`.

```shell
> php --ini

Configuration File (php.ini) Path: /etc/php/7.1/cli
Loaded Configuration File:         /etc/php/7.1/cli/php.ini
Scan for additional .ini files in: /etc/php/7.1/cli/conf.d
Additional .ini files parsed:      /etc/php/7.1/cli/conf.d/10-mysqlnd.ini,
/etc/php/7.1/cli/conf.d/10-opcache.ini,
/etc/php/7.1/cli/conf.d/10-pdo.ini,
/etc/php/7.1/cli/conf.d/20-calendar.ini,
/etc/php/7.1/cli/conf.d/20-ctype.ini,
/etc/php/7.1/cli/conf.d/20-exif.ini,
/etc/php/7.1/cli/conf.d/20-fileinfo.ini,
/etc/php/7.1/cli/conf.d/20-ftp.ini,
/etc/php/7.1/cli/conf.d/20-gd.ini,
/etc/php/7.1/cli/conf.d/20-gettext.ini
```

In alternativa, il percorso può essere intelligentemente analizzato (funziona sui sistemi Linux):

```shell
php -r "phpinfo();" | grep php.ini
```

Tornerà:

```shell
Configuration File (php.ini) Path => /etc/php/7.1/cli
Loaded Configuration File => /etc/php/7.1/cli/php.ini
```

> **Importante:** Di solito la configurazione è divisa in diversi file a seconda dell'ambiente e dei pacchetti, dove `php.ini` è globalmente valido per tutti, mentre per esempio la configurazione `CLI` è valida solo per la modalità CLI, cioè chiamando un cron o un comando dal terminale.

Configurare il limite di dimensione dei file in upload
----------------------------------------------

Un esempio di una proprietà che è spesso configurata direttamente in `php.ini` è la dimensione massima del file caricato (il default è di solito `2 MB`, che è già basso nel 2018).

All'interno del file di configurazione questo è scritto come segue, per esempio:

```shell
; Maximum allowed size for uploaded files.
upload_max_filesize = 40M

; Must be greater than or equal to upload_max_filesize
post_max_size = 40M
```

*Punto centrale significa commento, seguito da direttive di configurazione specifiche.*
