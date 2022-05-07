Compositore - panoramica completa delle caratteristiche avanzate
================================================================

> id: a74d8d59-91ce-4602-ad52-80cf89a647bd
> slug:
> 	cs: composer
> 	it: compositore---panoramica-completa-delle-caratteristiche-avanzate
> 
> perex:
> 	- Composer je pokročilý správce balíků a závislostí pro vaše PHP aplikace. Článek popisuje jeho všechny výhody a možnosti použití.
> 	- Composer è un gestore avanzato di pacchetti e dipendenze per le tue applicazioni PHP. Questo articolo descrive tutti i suoi vantaggi e usi.
> 
> publicationDate: '2020-03-10 20:18:19'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '68340d4b4d3c8a6ed143ede176fbf04e'

Come già sapete, [Composer](https://getcomposer.org/) è un robusto gestore di pacchetti e dipendenze per PHP, attraverso il quale è possibile gestire elegantemente centinaia di progetti in una sola volta e distribuire il codice una volta scritto a tutte le applicazioni contemporaneamente.

Questo tutorial serve come una guida dettagliata e completa per gli sviluppatori. Copriremo tutte le tecniche avanzate importanti per lavorare con Composer, oltre a spiegare i dettagli tecnici e le dipendenze rilevanti.

Installazione di Composer
-------------------

Indipendentemente dalla piattaforma, si scarica da [il sito ufficiale di Composer](https://getcomposer.org/).

Internamente, viene scaricato il file PHP `composer-setup.php`, che quando viene eseguito in modalità CLI può installare Composer. È anche importante sapere che Composer non funziona senza PHP, quindi prima verifica di avere PHP in esecuzione sul tuo computer (deve essere disponibile solo per Terminal).

Su Mac e Linux, Composer funziona subito dopo l'installazione e basta chiamare il comando `composer -v` per verificare rapidamente che Composer sia installato correttamente.

Su Linux, il seguente comando può essere usato per installare: `/usr/bin/php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"`

Su Windows, è una buona idea installare lo strumento [Git Bash for Windows](https://gitforwindows.org/), che permette di aprire un terminale specifico che si comporta quasi come Linux e permette di lavorare nello stesso ambiente di Linux.

L'installazione per i server è la stessa che su un ambiente locale. Assicurati solo di avere la versione corretta di PHP che Composer usa internamente.

Comandi disponibili
----------------

Composer implementa una serie di comandi.

L'uso è: `composer <comando>`, per esempio: `composer update`.

Panoramica per `1.10.0`:

| Comando | Descrizione / Significato |
|-----------------------|----------------|
| `about` | Mostra brevi informazioni su Composer.
| Crea un archivio con il contenuto del pacchetto Composer selezionato.
| Apre la home page del pacchetto selezionato, dell'autore o di altre pagine correlate in un browser web. Spesso può contenere la documentazione su come usarlo.
| `cc` | Cancella la cache interna di Composer con le versioni dei pacchetti scaricati in passato.
| Controlla se i requisiti di installazione sono soddisfatti per la piattaforma corrente.
| Cancella la cache interna di Composer.
| Cancella la cache interna di Composer.
| Imposta una direttiva di configurazione.
| Crea un nuovo progetto basato sul pacchetto selezionato e crea automaticamente una cartella in cui collocare il progetto.
| Mostra quali pacchetti hanno causato l'installazione del pacchetto selezionato.
| Diagnostica il sistema per identificare errori comuni. L'elaborazione dell'output spetta allo sviluppatore, è solo un elenco.
| Genera un nuovo <a href="/autoloading-trid">autoloader</a>.
| Genera un nuovo <a href="/autoloading-trid">autoloader</a>.
| Esegue i binari e gli script da Vendor.
| Esplora come fare/modificare le tue dipendenze.
| Permette di eseguire comandi globali di Composer dalla variabile `$COMPOSER_HOME`.
| | `help` | Stampa l'aiuto per i comandi.
| Apre la pagina iniziale di un pacchetto specifico nel browser.
| Installa tutte le dipendenze del progetto secondo il file `composer.lock`, se esiste ed è valido. Se c'è un problema, le informazioni da `composer.json` vengono usate e `composer.lock` viene ripristinato al suo stato originale.
| Mostra informazioni sui pacchetti attualmente installati nel progetto. Visualizza i nomi di tutti i pacchetti, le loro versioni correnti e una breve descrizione.
| Crea una funzione di base `composer.json` nella directory corrente.
| Installa tutte le dipendenze del progetto secondo il file `composer.lock`, se esiste ed è valido. Se si verifica un problema, le informazioni da `composer.json` vengono usate e `composer.lock` viene riportato al suo stato originale.
| Mostra una lista di tutti i pacchetti, le loro versioni e la licenza corrente.
| Visualizza una lista di comandi disponibili.
| | `outdated` | Mostra una lista di tutti i pacchetti per i quali c'è una versione più recente disponibile per l'installazione e che soddisfano le dipendenze. Per ogni pacchetto, verrà visualizzata l'ultima versione compatibile che Composer suggerisce per l'installazione.
| Mostra quali pacchetti e dipendenze impediscono l'installazione del pacchetto o della versione richiesta.
| Rimuove il pacchetto dalla sezione di configurazione `require` o `require-dev`.
| Aggiunge il pacchetto richiesto a `composer.json` e lo installa. Se le dipendenze non possono essere soddisfatte, si ritorna allo stato originale.
| Esegue gli script definiti in `composer.json`.
| Esegue gli script definiti in `composer.json`.
| Cerca i pacchetti per parola chiave o query di ricerca.
| Aggiorna il file interno `composer.phar` all'ultima versione.
| `selfupdate` | Aggiorna il file interno `composer.phar` all'ultima versione.
| | `show` | Visualizza informazioni dettagliate sui pacchetti attualmente installati.
| `status` | Mostra un riassunto delle modifiche locali apportate ai pacchetti manualmente, basato su un confronto con il sorgente del pacchetto da cui è stato originariamente installato. |
| Mostra i suggerimenti per i pacchetti. I suggerimenti possono includere vari tipi di azioni, come l'installazione di aggiornamenti di sicurezza e così via.
| Aggiorna l'intero progetto secondo le dipendenze, in modo che siano sempre tutte soddisfatte da `composer.json`. Se ha successo, aggiorna `composer.lock`, dove scrive le versioni attualmente installate.
| `upgrade` | Alias a `update`. |
| Alias di `update`.
| `validate` | Controlla `composer.json` e `composer.lock` per errori di sintassi.
| Mostra quali pacchetti hanno causato l'installazione del pacchetto attualmente selezionato, incluse tutte le dipendenze.
| `why-not` | Mostra quali pacchetti e versioni impediscono l'installazione del pacchetto o della versione selezionati.

Creare e definire un progetto
----------------------------------

Ogni progetto gestito da Composer è definito da un file `composer.json` nella sua radice che definisce tutte le dipendenze. Il file può essere creato manualmente per un progetto esistente o automaticamente quando si crea un progetto.

Poiché tutto in Composer è un pacchetto, il progetto stesso può essere basato su un pacchetto. Così, per esempio, se state creando decine o centinaia di progetti molto simili, ha senso mettere la loro configurazione e struttura di base in un pacchetto separato su cui basare l'installazione.

Un esempio di tale pacchetto è il mio [Baraja Sandbox](https://github.com/baraja-core/sandbox), che è basato su Nette 3.0 puro e aggiunge una dipendenza di base al mio [Package Manager](https://github.com/baraja-core/package-manager), che uso per tutti i progetti e la gestione delle dipendenze nella configurazione di Nette.

La sandbox viene poi installata semplicemente con il comando:

```shell
$ composer create-project baraja/sandbox <your-project-name>
```

In base al nome del progetto, Composer creerà automaticamente una cartella per installare il progetto (copiare il contenuto del pacchetto e installare le dipendenze).

Nella cartella `vendor`, Composer gestisce poi tutti i pacchetti (i loro file fisici sono lì) e genera un autoload di classi, che idealmente mettiamo direttamente in `index.php` come linea:

```php
require __DIR__ . '/vendor/autoload.php';

// Il codice dell'applicazione stessa
```

Installare pacchetti e dipendenze aggiuntive
-------------------------------------

All'interno di un progetto funzionale possiamo installare nuovi pacchetti e aggiungere dipendenze molto facilmente.

Ci sono 2 modi per farlo:

- Con il comando `composer require ...`,
- Aggiungendo una dipendenza direttamente al file `composer.json` nella sezione `require`, e poi usando il comando `composer update`.

Prova a installare per esempio PackageManager: `composer require baraja-core/package-manager`, o [Doctrine](https://github.com/baraja-core/doctrine): `composer require baraja-core/doctrine`.

Se il pacchetto scelto non può essere installato, si possono chiedere ragioni specifiche e Composer elencherà le dipendenze che lo impediscono. Spesso è sufficiente fissare una dipendenza da una particolare versione o rimuovere il codice incompatibile. Per informazioni dettagliate, usate il comando: `composer why baraja-core/doctrine`.

Aggiornamento dei progetti e dei pacchetti
-----------------------------

Un progetto ben progettato è sviluppato in modo da poter scaricare facilmente gli aggiornamenti nel tempo e avere sempre le ultime versioni di tutti i pacchetti. Il vantaggio principale è che si ottengono tutti i bug risolti, spesso miglioramenti delle prestazioni e un sacco di nuove funzionalità. Inoltre, il passaggio graduale renderà l'aggiornamento meno complicato dopo molto tempo, dato che risolverete i problemi al volo su scala minore ed eviterete le incompatibilità.

Per aggiornare tutti i pacchetti e le dipendenze, usate il comando `composer update`.

Il processo di aggiornamento stesso può fallire in alcuni casi. La ragione è di solito o una dipendenza interrotta o un pacchetto incompatibile.

Per informazioni dettagliate sul perché un pacchetto non può essere installato, usa il comando: `composer why-not baraja-core/doctrine`. Se abbiamo già il pacchetto e solo la versione specifica non funziona (non vuole installarsi), possiamo anche chiedere la versione specifica: `composer why-not baraja-core/doctrine:v1.0.20`.

All'interno del file `composer.json`, possiamo anche elencare la dipendenza da uno specifico runtime. Questo è particolarmente utile quando stiamo sviluppando un progetto con più persone e vogliamo verificare che abbiano tutte le estensioni installate.

Tipicamente, viene controllata la versione di PHP (deve essere `7.1.0` o successiva):

```json
{
   "require": {
      "php": ">=7.1.0"
   }
}
```

Forse altre estensioni del sistema:

```json
{
   "require": {
      "php": ">=7.1.0",
      "ext-json": "*",
      "ext-session": "*",
      "ext-PDO": "*"
   }
}
```

Queste regole sono poi prese in considerazione quando si installano pacchetti o aggiornamenti. Questo aiuta a prevenire problemi che diventerebbero evidenti in fase di esecuzione. Tipicamente, per esempio, un pacchetto gateway di pagamento ha bisogno di comunicare con le API, quindi deve ammettere dipendenze dalle estensioni `curl` e `json`.

Risoluzione dei problemi delle dipendenze
-----------------------------

Spesso, le violazioni delle dipendenze si verificano a causa di una cattiva versione di PHP. In questo caso Composer lancia un messaggio per esempio `la tua versione di PHP (7.3.11) sovrascritta dalla versione "config.platform.php" (7.1) non soddisfa questo requisito.

Molto spesso l'errore è causato dalle impostazioni direttamente nel progetto `composer.json`, dove si trova la seguente sezione:

```json
"config": {
   "platform": {
      "php": "7.2"
   }
}
```

La modifica **deve essere fatta direttamente nel file**. Nel caso di un progetto globale (prima dell'installazione o con una dipendenza globale), puoi forzare la versione di Composer con `composer config -g platform.php 7.2.14` (lo switch `-g` significa `globale`).

In alcuni casi, vogliamo installare le ultime versioni dei pacchetti e ignorare le impostazioni dell'ambiente locale. In questo caso, possiamo usare il comando avanzato:

```shell
$ composer update --ignore-platform-reqs
```

**Usa lo switch `ignore-platform-reqs` a tuo rischio e pericolo, può causare danni al progetto!** Non usarlo se non ne capisci le conseguenze.

Chiamate manuali di Composer, parametri e gestione della memoria
------------------------------------------------------

Composer è in realtà uno script PHP avvolto in un cosiddetto PHAR, che è una versione compilata di un'applicazione PHP. Conoscere queste informazioni può essere messo a frutto, per esempio per parametrizzare meglio la chiamata stessa.

In progetti davvero grandi, a volte succede che finiamo la RAM e abbiamo bisogno di allocarne molta di più, o di aumentare il tempo di elaborazione degli script.

Per esempio, su Windows potete usare questo comando per questo:

```shell
$ php -d memory_limit=-1 C:/xampp/htdocs/composer.phar update
```

Lo switch `memory_limit=-1` dice a Composer di non essere suscettibile al limite di RAM e di consumare tutta la memoria di cui ha bisogno.

Script utente personalizzati dopo l'azione Composer
--------------------------------------------

Dopo aver eseguito le azioni di Composer, puoi invocare l'esecuzione automatica di script definiti dall'utente che eseguono azioni specifiche sul progetto o, per esempio, generano una configurazione dopo il deployment. Io uso principalmente questo approccio su un server locale per offrire uno strumento di configurazione automatica del database, generare uno schema di database e così via.

Registriamo gli script richiesti nella sezione `scripts` di `composer.json`:

```json
"scripts": {
   "post-autoload-dump": "Baraja\\PackageManager\\PackageRegistrator::composerPostAutoloadDump"
}
```

In questo caso il metodo statico `composerPostAutoloadDump` nella classe `Baraja\PackageManager\PackageRegistrator` viene chiamato automaticamente. Composer ha questa classe disponibile perché ha eseguito per primo la generazione delle classi <a href="/autoloading-trid">autoloader</a>.

Se vogliamo solo eseguire gli script e non eseguire azioni inutili con Composer, il comando `composer dump` è molto utile, poiché genera solo un nuovo autoloader (che consiglio di tenere sempre aggiornato) e poi esegue immediatamente gli script. Se volete provare a usare gli script, ho preparato un pacchetto pronto [Baraja PackageManager](https://github.com/baraja-core/package-manager) che implementa script intelligenti e un'interfaccia interattiva per il framework Nette.

Versioning Vendor in Git?
------------------------

Una domanda che discutiamo spesso con gli sviluppatori è se fare una versione del contenuto della cartella `vendor` in Git, o se farla ri-generare ad ogni installazione.

In generale, la soluzione più pulita sembra essere quella di **non fare alcuna versione** del contenuto e installare tutto ogni volta. Ma la realtà è che di tanto in tanto uno sviluppatore smetterà di sviluppare un pacchetto, o lo rimuoverà del tutto. Il download costante dei pacchetti complica anche le installazioni e gli aggiornamenti locali, e rallenta anche i deploy, e a volte può causare brevi interruzioni del sito quando le nuove versioni dei pacchetti vengono scaricate in modo errato.

Vedo la sospensione di Vendor come una forma di "sicurezza". Se i file sono fisicamente nel sistema di versioning, abbiamo almeno una garanzia rudimentale sul server di produzione che i pacchetti funzioneranno e il loro codice è esattamente come è nell'installazione in esecuzione locale. Inoltre, Vendor occupa spesso solo unità di MB, il che vale certamente la garanzia di un sito funzionante date le capacità del disco di oggi.

**Nota pratica:** Per il sito medio che gestisco, `vendor` occupa non più di `30 MB`, che è una capacità accettabile per Git. Quando cloniamo un repository con junior, non dobbiamo poi occuparci di addestrarlo su come far funzionare il sito, e semplicemente "funziona subito".

Pacchetti Composer personalizzati
-----------------------

Puoi creare i tuoi pacchetti all'interno di Composer, sia pubblici (registrati a [Packagist](https://packagist.org/)) che privati (devi avere un tuo server di pacchetti, per esempio [Satis](https://getcomposer.org/doc/articles/handling-private-packages.md)).

La questione della creazione, del mantenimento, dello sviluppo e del versionamento dei pacchetti è molto complessa e sarà oggetto di un articolo separato.

Nel frattempo, potete leggere l'articolo [Semantic Versioning](https://semver.org/lang/cs/), che ho tradotto per voi.
