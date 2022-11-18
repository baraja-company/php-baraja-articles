Cosa cambierei di Nette se fossi David Grudl
============================================

> id: bad8fe4f-92a4-4396-8698-7ec42bb2aef8
> slug:
> 	- null
> 	it: cosa-cambierei-di-nette-se-fossi-david-grudl
> 
> cs: co-bych-zmenil-na-nette
> publicationDate: '2022-11-18 17:45:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '953e6c78b9dff185bbf8e3e9582d24c3'

Utilizzo attivamente Nette Framework da quasi 6 anni per lo sviluppo di software commerciale. Durante i primi anni ero molto entusiasta e il framework rispondeva perfettamente a tutte le esigenze che avevamo come team, e in pratica non c'era motivo di cercare un altro strumento.

Dal 2019 circa, sto iniziando a vedere un calo dell'entusiasmo iniziale all'interno di Nette, dove cominciano a mancarmi alcune delle funzioni avanzate. Spesso si tratta di cose che esulano dal framework stesso, quindi non mi aspetto che vengano mai implementate, ma d'altra parte uno sviluppatore deve prendere una decisione su come continuare lo sviluppo in futuro e magari scegliere una tecnologia diversa.

Livello database
-----------------

Considero Nette Database uno dei più grandi errori del framework, purtroppo.

Ogni settimana abbiamo colloqui in azienda in cui giovani appena usciti dalla scuola, o anche persone intermedie che stanno passando da un altro framework, si candidano e scoprono Nette Database come parte della loro esplorazione di Nette. Come livello di database non è male, ma il problema è l'uso pratico in progetti reali.

Questo perché Nette Database non si sforza di spingere gli sviluppatori a usare tipi di dati rigorosi fin dall'inizio, a dare un nome alle colonne e alle relazioni, a separare i metodi di query (repository) dai modelli e ad altre piccole sfumature che nel complesso fanno molto.

Per molti anni ho usato la libreria Dibi, che ha avuto un ruolo enorme ai suoi tempi. Permetteva di interrogare il database in modo molto semplice e unificava l'interfaccia. D'altra parte, i tempi sono cambiati e quando i database restituiscono oggetti non tipizzati, PHPStan non è contento per principio.

Personalmente, vorrei incorporare il supporto nativo per le entità e i repository in Nette Database, oppure fornire il supporto ufficiale per Doctrine all'interno di Nette. Se si programmano applicazioni al livello di una grande azienda, bisogna fare sul serio con lo sviluppo, e Doctrine in particolare ha molto senso. Allo stesso tempo, si vuole educare subito i nuovi arrivati a una tecnologia migliore e non si vuole insegnare loro le vecchie abitudini.

Tracy e registrazione degli errori
---------------------

Considero ancora Tracy il miglior debugger runtime per PHP.

Quando nel 2017 sono entrato in un'agenzia digitale senza nome e ho visto lo stato tecnico dei loro progetti Nette, sono rimasto inorridito. Quante volte hanno avuto siti scritti in puro PHP senza alcun tentativo di registrare gli errori. Una soluzione abbastanza semplice era quella di installare Tracy nel progetto, chiamare `Debugger::enable()` e non fare altro. I bug venivano registrati automaticamente in una directory che doveva essere recuperata manualmente ogni pochi giorni.

Per quanto riguarda Tracy, mi sembra che sia un prodotto finito che David ha poche ragioni per migliorare. D'altra parte, vedo ancora un margine di miglioramento in una nuova direzione.

Ad esempio, perché Tracy non include funzioni a pagamento per le aziende o gli individui che hanno a cuore la sicurezza?

Tracy potrebbe implementare un'interfaccia web personalizzata di tipo Sentry in cui si crea un account e i bug di produzione vengono inviati automaticamente tramite un'API in cui è possibile tenerne traccia tra i vari progetti. L'applicazione potrebbe anche richiedere singoli siti e rilevare automaticamente l'indisponibilità e avvisare il proprietario in altri modi (dato che Tracy non è in grado di rilevare logicamente un crash del server). A volte mi capita anche che Tracy venga abilitato per sbaglio in un sito di produzione e che si possa scoprire tramite DIC come accedere al database o altrove. Tracy dovrebbe avvertirvi anche di questo.

Tracy potrebbe anche implementare altre piccole cose che conosco da Node.js. Ad esempio, per la visualizzazione del codice sorgente, potrebbe essere possibile scegliere l'intervallo di caratteri in cui la riga viene evidenziata in modo speciale, perché la possibilità di evidenziare un token specifico all'interno di una riga. Allo stesso tempo, sarebbe bello aggiungere il supporto per una seconda linea di evidenziazione (magari blu) per mostrare il contesto nella parte successiva dell'applicazione.

Quando si visualizza uno snippet di codice sorgente, non avrei paura di aggiungere un pulsante in grado di rendere il resto del codice in ajax, in modo da poterlo esplorare direttamente nel browser dal calltack. Questo è ciò che fa Symfony ed è una grande caratteristica. Se a ciò si aggiungesse un'analisi statica di base, con la possibilità di strisciare i metodi, le classi e l'ereditarietà, si risparmierebbe molto lavoro di debug per l'intero team.

Se il calltack attraversa un pacchetto, sarebbe bello mostrare la versione del pacchetto con cui sto lavorando per un determinato file. Spesso mi capita di riscontrare un errore in un pacchetto che sto sviluppando e potrei anche sapere visivamente che si tratta di una versione precedente.

Routing statico
----------------

All'interno di Nette Router, vorrei definire un nuovo tipo chiamato router statico. Sarebbe un discendente del router di base, ma sarebbe in grado di accettare solo stringhe letterali, che non sarebbero espressioni regolari, ma un percorso diretto. Molte pagine funzionano in modo statico (contatti, vari articoli, sorprendentemente spesso anche la homepage) e il router deve valutare ogni volta le regole regex per la corrispondenza e la composizione degli URL.

Se un percorso statico fosse usato in un template di Latte, per esempio, potrebbe essere tranquillamente tradotto in una stringa statica `{$basePath}/path` al momento della compilazione del template, così non ci sarebbe bisogno di compilare i 100 URL che sono nel layout, per esempio, a ogni richiesta.

Capisco che posso ottenere la stessa funzionalità con la cache sul lato del modello, ma apprezzerei molto di più una soluzione di sistema.

Nel framework Next.js, mi sono completamente innamorato del concetto di routing statico. Non esiste una cosa come `n:href`, in breve bisogna conoscere l'URL in anticipo, il che costringe a non fare tanti reindirizzamenti, a pensare in anticipo ai formati degli URL e a mantenerli persistenti.

Interfaccia PSR
------------

È un peccato che Nette non implementi in modo nativo l'interfaccia PSR. Come sviluppatore di pacchetti, questo vi porta fuori strada se volete definire una dipendenza di Composer da Nette. Da un lato Nette vi risolve molti problemi, dall'altro sapete che non riuscirete più a sostituirlo. Più di una volta mi sono trovato a preferire Symfony, Laravel o un'interfaccia generica completamente diversa, a causa della mancanza di un'interfaccia generica.

La mancanza di supporto per gli standard generici per la futura portabilità è la ragione principale per cui ho abbandonato completamente Nette nel 2022 e sviluppiamo le nuove applicazioni in team esclusivamente in modo generico.

Livello API
----------

E se si volesse utilizzare Nette come backend per gestire le richieste API REST? Si costruisce un presentatore e si invia la risposta come `$this->sendJson()`? Installerete una libreria di terze parti? O sei David Grudl che risolve la necessità di una libreria mancante scrivendone subito una nuova?

Perché Nette non può implementare qualcosa di così comune come l'API REST direttamente dalla scatola? Oggi quasi tutte le applicazioni hanno bisogno di comunicare tramite un'API, ad esempio per fornire dati al frontend.

Per i nuovi arrivati, questo li porta ancora una volta a usare Latte e gli snippet integrati, invece di scoprire che esiste un'opzione migliore, ovvero costruire l'intero frontend separatamente e fornire solo i dati.

Fiducia in ciò che accadrà tra 10 anni
-------------------------

L'ultimo punto è molto pragmatico e deriva da un dibattito che sentiamo di tanto in tanto dai dipartimenti aziendali del team.

Cosa succederebbe se David Grudl smettesse di sviluppare Nette? E se qualcuno smettesse di svilupparlo? A cosa passeremmo? E quanto sarebbe impegnativo?

Molti sviluppatori (spesso privi di esperienza aziendale) amano ignorare questa preoccupazione, affermando che non c'è nulla di male. Come sì, ma non lo è.

Quando si è alle prese con la questione se investire tempo in Nette o in React e Node.js per formare gli sviluppatori junior della propria azienda, purtroppo vince la seconda opzione.

Nette non ha ancora perso il treno, ma nelle decine di colloqui che ho avuto negli ultimi sei mesi, lo sento molto forte.
