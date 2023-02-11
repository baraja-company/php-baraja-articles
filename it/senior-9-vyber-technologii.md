Come selezionare le tecnologie? Quando si passa a JavaScript?
=============================================================

> id: d3ea7ea7-3e2f-455d-a424-cce374bcd1d2
> slug:
> 	cs: senior-9-vyber-technologii
> 	it: come-selezionare-le-tecnologie-quando-si-passa-a-javascript
> 
> publicationDate: '2023-02-11 14:40:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '72807451867478e1a7b6d67f4e5c31b3'

La scelta delle tecnologie giuste è un prerequisito per diventare uno sviluppatore senior. Queste decisioni spesso non sono facili, perché bisogna prendere in considerazione lo stato tecnico attuale dell'applicazione, dove si sta andando a sviluppare, quali sono le conoscenze attuali del team, quali sono le conoscenze comuni nel mercato del lavoro, quali sono i costi di ciascuna tecnologia, quali rischi comporterà per le operazioni, quanto è sicura e stabile la tecnologia e, ultimo ma non meno importante, a cosa saranno interessati gli sviluppatori, ad esempio tra 5 anni quando l'80% del team attuale sarà sostituito.

Ho avuto a che fare con 6 grandi aziende che sviluppano in PHP. Solo 2 di loro stanno cercando di passare a un'altra tecnologia nel lungo periodo, gli altri rimangono. Questo comporta molti problemi. Ad esempio, attualmente sto cercando di trovare uno sviluppatore PHP senior per un progetto aziendale che sto sviluppando per O2, con l'obbligo di spostarsi negli uffici di Praga, e posso notare come il mercato degli sviluppatori PHP si sia liberato negli ultimi 5 anni. Il PHP non è più cool e non sono in molti a volerlo fare. Non ci sono abbastanza giovani.

Intervistando i più giovani, ho percepito che React e in generale le tecnologie "sottili" sono molto popolari al giorno d'oggi. Dal punto di vista dell'architettura applicativa, ha senso se si scopre questa direzione in anticipo e si ha il tempo di adattarsi. Invece della complessa realizzazione di layout e moduli web in Latte, per la quale è necessario uno sviluppatore praticamente mediocre per un compito già leggermente più complesso, in React è sufficiente un giovane che ha iniziato praticamente un mese fa, e che ancora non commette troppi errori nella soluzione futura.

React consente di buttare via gran parte del backend che è stato scritto solo per far esistere il frontend. In breve, rende lo sviluppo più economico e, come bonus, si ottiene una consegna più rapida delle nuove funzionalità, perché gli sviluppatori non devono affrontare più volte i complessi problemi derivanti dal linguaggio di progettazione PHP.

La maggior parte delle applicazioni web non ha più bisogno di un backend, o solo di un backend minimo. Quando si espongono gli endpoint delle API in Node.js (anch'essa una tecnologia costruita su JavaScript), improvvisamente uno sviluppatore che prima si occupava solo di React può scrivere anche pezzi del backend, perché si tratta dello stesso linguaggio.

Da un'analisi più approfondita dei progetti che ho sviluppato negli ultimi 5 anni, ci sono solo alcune cose che mancano in Node.js e che mi spingono ancora a usare PHP per alcune operazioni.

Ovvero:

- Doctrine (e in generale l'accesso ai database relazionali basato su entità oggetto)
- Invio e gestione delle e-mail
- API SOAP (purtroppo a volte è ancora in circolazione)
- Sessioni (è necessario sostituirlo con un token JWT, ad esempio)
- Logica legacy complessa che è stata scritta in PHP e che non è possibile rifattorizzare facilmente
- Elaborazione rapida di strutture di dati complesse in cui i dati devono essere mutati
- Persone già presenti nel team che devono essere riqualificate per fare qualcosa di nuovo

Ma poi è arrivato Node.js, che fa il resto delle cose meglio. Ad esempio:

- La possibilità di scaricare l'app direttamente nel cloud
- Sviluppo molto (forse anche due volte) più economico della stessa funzionalità
- Stessa logica su BE e FE senza dover scrivere due volte il codice
- Endpoint API REST
- Chiamate parallele a più codici contemporaneamente
- Possibilità di inviare una risposta HTTP, ma il codice continua ad essere eseguito
- Cronista
- Le biblioteche lavorano con i servizi cloud
- Tempo di risposta significativamente migliore, perché non è necessario avviare un'applicazione di grandi dimensioni
- Paradigma completamente funzionale (ci si sbarazza delle DI che non servono in JS, per esempio)
- Lavorare con moduli e dati
- Aggiornamenti facili e una comunità di sviluppatori attiva

**Commento su Doctrine:** So che JS offre molte librerie per lavorare con i database. O anche nuovi paradigmi come Mongo. Mi piace la direzione che sta prendendo l'elaborazione dei dati. D'altra parte, credo che i database relazionali tabulari non scompariranno mai. Quando si tratta di un progetto molto grande, in cui si gestiscono decine di milioni di record, è necessaria una tecnologia tradizionale con cui si ha molta familiarità e si sa cosa aspettarsi. Ad esempio, l'idea di voler aggiungere una colonna (proprietà) e di dover rimappare tutte le entità con uno script di migrazione mi spaventa parecchio.
