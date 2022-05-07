GitHub Actions - la migliore CI per il 2021
===========================================

> id: c1fe3b77-a506-4f20-bf1e-90ee82817c7d
> slug:
> 	cs: github-actions-nejlepsi-ci-pro-rok-2021
> 	it: github-actions---la-migliore-ci-per-il-2021
> 
> perex:
> 	- 'Protože už nějaký čas vyvíjíte webové aplikace, určitě jste si všimli, že se pro vás celá řada věcí rutinně opakuje, i když by nemusela. Velmi často jde o technickou správu projektů, verzování souborů, automatická kontrola kódu, zpracování různých dat, nebo třeba deploy na server a bezpečnostní věci kolem toho.'
> 	- 'Dato che state sviluppando applicazioni web da un po'' di tempo, avrete probabilmente notato che molte cose sono abitualmente ripetitive per voi, anche se non dovrebbero esserlo. Molto spesso si tratta di gestione tecnica del progetto, versioning di file, revisione automatica del codice, elaborazione di dati vari, o forse distribuzione sul server e roba di sicurezza intorno a questo.'
> 
> publicationDate: '2021-02-07 15:46:39'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: d3d151927a02dde744d1202449c904dc

Dato che state sviluppando applicazioni web da un po' di tempo, avrete probabilmente notato che molte cose sono abitualmente ripetitive per voi, anche se non dovrebbero esserlo. Molto spesso si tratta di gestione tecnica del progetto, versioning di file, revisione automatica del codice, elaborazione di dati vari, o forse distribuzione sul server e roba di sicurezza intorno a questo.

Nella consulenza nelle aziende, mi imbatto molto spesso nel problema in cui la prevenzione è molto sottovalutata. La ragione è spesso che gli sviluppatori percepiscono alcune cose come molto impegnative e che aggiungeranno lavoro per loro. Ma la verità è che di solito è necessario impostare l'intero setup solo una volta e poi raccogliere i benefici a lungo termine.

Cos'è CI (Integrazione continua)
----------

CI/CD è uno strumento che può automatizzare i compiti di routine che hanno una base simile e continuano a ripetersi nei progetti. L'uso più comune di CI è per l'esecuzione di test automatici, la revisione del codice, il deploy automatico (distribuzione di un'applicazione su un server web), la gestione del progetto o forse i controlli di sicurezza.

Pensate a CI come a un sistema operativo virtuale che esegue comandi predefiniti al terminale o esegue programmi specifici. L'output di CI è un successo o un fallimento, che viene visualizzato direttamente nel progetto, ma anche inviato agli amministratori che possono reagire ad esso. Una buona pratica è quella di eseguire i lavori di CI dopo un evento specifico (per esempio, un commit al repository, una richiesta di merge/pull ricevuta, un tag/versione/release, o un cron timeloop eseguito).

Come funziona specificamente CI
------------

La base di CI è un file di configurazione in cui si definisce il suo comportamento e la risposta agli eventi. Nel caso di GitHub, basta creare una directory di aiuto `.github/workflows` e creare qualsiasi file con estensione `.yml` all'interno. GitHub lo analizzerà con ogni commit e lo eseguirà come necessario.

Immaginiamo di voler definire un nuovo file di configurazione con un compito specifico. Poiché questo è un compito che sarà eseguito direttamente da GitHub o da un altro ambiente sulla macchina virtuale, abbiamo bisogno di definire le cose di base dell'ambiente, tra cui:

- Il nome del compito (per esempio, il nome del test)
- Eventi di trigger (su quale evento far scattare il task in base, per esempio, ogni volta che si fa un push al repository o si riceve una richiesta di pull)
- La definizione dei compiti stessi (ci possono essere molti compiti in esecuzione in parallelo)

Nel caso dei lavori, definiamo ulteriormente:

- Nome del lavoro
- Ambiente di esecuzione (tipicamente il nome del sistema operativo)
- Passi per costruire il contenitore

Il contenitore di cui sopra è già la prima preparazione per la **completa dockerizzazione del progetto**. L'uso di base di CI è relativamente facile e non c'è bisogno di capire Docker a tutti, basta conoscere i comandi nel terminale.

Come parte dei passi specifici del compito, abbiamo bisogno di eseguire i singoli comandi che costruiranno l'installazione del sistema operativo che abbiamo appena installato. Quindi, nel caso di PHP, sarà importante installare solo il linguaggio PHP (e specificare la versione), clonare il repository del progetto sul runner, installare le dipendenze del progetto (più spesso [Composer](/composer)), ed eseguire il test stesso.

Perché usare GitHub Actions (CI)?
--------

C'è una superstizione comune nella comunità IT che Microsoft sia la società malvagia che produce roba di bassa qualità. Nel caso di GitHub Actions, tuttavia, questo non è affatto vero, perché hanno, oggettivamente, la migliore piattaforma CI del mondo.

Il vantaggio fondamentale di GitHub CI è la semplicità e l'eleganza della notazione. Con poche righe, potete configurare il vostro ambiente di test e gestirlo automaticamente, anche senza conoscenze preliminari. Allo stesso tempo, vedo la velocità di elaborazione di compiti specifici come un enorme vantaggio. Per esempio, un test su codestyle (formattazione del codice) e PhpStan (controllo dei tipi di dati, logica e correttezza generale) può essere valutato da GitHub Actions su un progetto regolare in meno di 50 secondi, mentre GitLab, per esempio, valuta lo stesso test in quasi 8 minuti! Altre soluzioni come Travis sono relativamente costose e la maggior parte degli sviluppatori non apprezza comunque i vantaggi delle loro caratteristiche speciali.

Azioni preparate
-----

GitHub ha un grande database di azioni e pacchetti già pronti che puoi usare subito. Questi sono tipicamente sistemi operativi, linguaggi di programmazione o librerie della comunità.

Potete trovare un database di azioni proprio nel [Marketplace](https://github.com/marketplace?type=actions), per esempio la mia azione preferita è [Inviare SMS in caso di fallimento via Twillio](https://github.com/marketplace/actions/twilio-sms).

Esempi finiti
------

Moltissimi esempi [pubblico all'interno dei miei repository] (https://github.com/baraja-core) dove si può vedere in modo trasparente quale configurazione specifica uso.

Per esempio, nel febbraio 2021 ho usato questa configurazione per il controllo del codestyle in un progetto:

```yaml
name: Coding Style

on:
  push:
    branches:
      - master
  pull_request:
    types: [ assigned, opened, synchronize, reopened ]
  schedule:
    - cron: '1 * * * *'

jobs:
  nette_cc:
    name: Nette Code Checker
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 7.4
          coverage: none

      - run: composer create-project nette/code-checker temp/code-checker ^3 --no-progress
      - run: php temp/code-checker/code-checker --strict-types --no-progress

  nette_cs:
    name: Nette Coding Standard
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 8.0
          coverage: none

      - run: composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
      - run: php temp/coding-standard/ecs check src
```

Questo file di configurazione installa l'ultima Ubuntu (`runs-on: ubuntu-latest`), clona il repository del progetto (`uses: actions/checkout@v2`), installa PHP (`uses: shivammathur/setup-php@v2`) versione 7.4 (`php-version: 7.4`), installa il pacchetto code-checker e poi esegue il lavoro tramite PHP.

Qualche altra nota per gli utenti che non sono così esperti:

- CI avvierà una macchina virtuale con un sistema operativo completo ogni volta che viene eseguito, che può essere utilizzato per chiamare comandi in Terminale proprio come, per esempio, il vostro server
- Per controllare i risultati del test, vengono utilizzati i cosiddetti codici di ritorno, definiti da Linux stesso. In particolare, quando un programma restituisce `0` (zero), significa successo e qualsiasi altro numero significa fallimento. Per esempio, gli script di shell funzionano allo stesso modo
- Quando vogliamo scrivere un test personalizzato o una logica più complessa, possiamo usare PHP, per esempio, ed eseguirlo come un lavoro CLI (il comando `php file.php`), che verrà eseguito e potrà fare qualsiasi cosa abbia i diritti di fare (lavorare con i file, comunicare su internet, emettere sullo schermo, ...)
- Mentre CI è in esecuzione, tutti gli output sono registrati sullo schermo e possono essere visualizzati direttamente nel browser web
- CI non è interattivo, anche se Terminal può eseguire operazioni interattive con l'utente, CI non supporta questo e deve essere progettato come un compito che a volte inizia e a volte finisce
- Viene definito un timeout di 1 ora per l'esecuzione del contenitore CI, se tutto non viene elaborato in quel tempo (per esempio, il programma si blocca), l'intero sistema verrà terminato forzatamente e verrà riportato un errore
- GitHub può eseguire automaticamente i lavori di CI tramite cron, tuttavia molto spesso non li esegue al momento giusto, ma li esegue con un ritardo
- Puoi anche avvolgere tutta la logica CI in un contenitore Docker ed eseguirlo localmente su tutte le macchine o su un server, per esempio
- Se hai bisogno di accedere a informazioni segrete (ad esempio la password del database), c'è un'opzione per definire variabili segrete direttamente in GitHub e CI le ha disponibili in fase di esecuzione
- Il deploy su un server web è sempre meglio farlo via SSH
- Il tempo totale di esecuzione dei lavori di CI è limitato, di solito a 2 mila minuti al mese (molto spesso questo sarà sufficiente per voi, non l'ho mai usato fino perché un lavoro viene eseguito per un massimo di un minuto), oppure è possibile aumentare il tempo a pagamento
- Puoi anche ospitare CI runner sul tuo server e bypassare il limite di minuti, ma molto probabilmente il tuo server non sarà molto più veloce perché Microsoft ha investito molto in Azure dove GitHub Actions è ospitato e gira su server molto potenti
- Molto spesso è utile installare pacchetti specifici solo per CI (tipicamente PhpStan e varie caratteristiche di sicurezza), quindi mettili nella sezione `composer.json` del file `require-dev` in modo che non vengano installati in un progetto specifico come dipendenza obbligatoria

App e bot di GitHub
-----

A differenza di altri host di repository, GitHub supporta le cosiddette GitHub Apps e i bot di automazione. Questo è un grande database di bot già pronti che possono eseguire operazioni automatizzate sui vostri progetti (anche senza CI) e quando incontrano qualcosa, per esempio, archiviano un problema o inviano una richiesta di pull con una correzione.

Lo uso personalmente e ve lo raccomando:

- Dependabot](https://dependabot.com) - controllo automatico delle dipendenze in Composer per esempio, quando esce una nuova versione dei pacchetti che usi ed è compatibile, invia automaticamente una richiesta di pull
- Codecov](https://github.com/marketplace/codecov) - controlla la copertura del codice con test automatici e graficizza quali parti del codice non sono ancora state coperte
- Snyk](https://github.com/marketplace/snyk) - controlla automaticamente le vulnerabilità di sicurezza
- Imgbot](https://github.com/apps/imgbot) - ottimizzazione automatica delle dimensioni delle immagini

Molte altre applicazioni possono essere trovate nel [Marketplace](https://github.com/marketplace?type=apps).

Altri consigli e trucchi utili
-----

Sono diventato estremamente affezionato all'uso dei compiti di automazione in GitHub.

Ho controlli automatici impostati in tutti i miei repository, così quando invio qualsiasi commit (o chiunque altro nella comunità), ottengo un feedback in tempo reale sulla qualità del codice (codestyle e PhpStan), la sicurezza e altro sono stati violati.

Ho alcuni test che vengono eseguiti automaticamente ogni ora. Per esempio, se c'è una vulnerabilità di sicurezza o un CVE, lo saprò al massimo in un'ora, anche a livello di progetti specifici e cosa è successo esattamente.

Nel corso del tempo, dopo aver testato diverse configurazioni, sono giunto alla conclusione che considero le seguenti dipendenze per Composer come la configurazione minima ideale:

- `phpstan/phpstan` - controllo del codice per la correttezza e la logica
- `tracy/tracy` - segnalazione di errori e log di alto livello
- `phpstan/phpstan-nette` - Estensioni e specialità Phpstan in Nette
- `spaze/phpstan-disallowed-calls` - una brillante libreria di Michal Spacek che gestisce il (dis)uso di cose pericolose nel codice (dai dump di variabili dimenticate alla capacità di eseguire una stringa utente come comando di shell)
- `roave/security-advisories` - controlla l'installazione di versioni non sicure dei pacchetti (se viene trovata una vulnerabilità di sicurezza in un particolare pacchetto o viene rilasciato un CVE, questo pacchetto impedirà l'installazione della versione corrotta)

Idealmente, dovreste avere la configurazione di sicurezza di base impostata per l'esecuzione automatica almeno ogni 8 ore e avere gli errori inviati alla vostra email. Perché ogni volta che l'installazione di un progetto fallisce o viene scoperta una nuova vulnerabilità, voglio saperlo il prima possibile e reagire immediatamente.

Ricorda che tutto funziona automaticamente, quindi puoi sempre essere sicuro che anche se usi processi complicati per controllare la sicurezza e altre cose, non ti costa alcuno sforzo extra e hai solo bisogno di una configurazione iniziale ben eseguita.

Perché c'è un enorme potenziale nella prevenzione e nell'automazione!
