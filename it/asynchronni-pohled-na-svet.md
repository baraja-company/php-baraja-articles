Visione del mondo asincrona
===========================

> id: '7ed25269-659f-4d17-a642-d07480cbfafa'
> slug:
> 	- null
> 	it: visione-del-mondo-asincrona
> 
> cs: asynchronni-pohled-na-svet
> publicationDate: '2022-10-15 11:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '24d4ec106e7febec5ee84cdf86bd8f28'

Da tempo ho notato che il nostro mondo ha una natura asincrona e decentralizzata. Quando ci si rende conto di questo e si inizia a pensare a come utilizzarlo a proprio vantaggio, è facile che emerga un concetto completamente nuovo di come guardare alla soluzione di problemi complessi. In questo post vorrei spiegare alcune delle idee che sto già utilizzando. Non le prendo da una fonte particolare, è più una combinazione di esperienze multiple e di idee personali. Questi principi non sono validi per tutti i casi.

Definizione dell'ambiente e degli obiettivi
-------------------------

Quasi tutti i progetti che ho affrontato (da solo o in gruppo) avevano un obiettivo ben definito. Questo approccio ha molto senso perché è importante sapere cosa vogliamo. D'altra parte, credo che definire un obiettivo specifico sia impossibile in un ambiente complesso, e spesso durante l'implementazione ci si accorge che in realtà si vuole arrivare da un'altra parte.

Quindi, invece di un obiettivo specifico, costruisco un'area di obiettivi diversi in cui ci sono alternative, o anche un obiettivo completamente nuovo e indipendente può essere l'obiettivo giusto, se ha senso.

Esempi dalla pratica:

- Ho bisogno di espandermi gradualmente in altri mercati. Uno dei sotto-obiettivi che ho scoperto durante l'implementazione potrebbe essere la traduzione automatica del web.
- Devo ritirare 6 spedizioni in diverse località di Praga e non conosco il percorso più breve. Non voglio scoprirlo in modo complicato, quindi mi dirigo verso il punto più lontano e cambio il mio piano verso altri percorsi lungo la strada. Ad esempio, quando cambio ad Anděl, decido di prendere il tram che arriva per primo, il che influisce dinamicamente sulla pianificazione del percorso successivo.
- Devo scrivere questo post, ma non ho un'ora di tempo di fila. Pertanto, posso scriverlo in parti mentre viaggio in metropolitana, come singoli capitoli. In futuro farò la fusione finale in questo modulo.
- Non so come funziona l'algoritmo per calcolare il riscatto della merce per il mio cliente, che devo riprogrammare. Quindi sottoporrò la domanda a più persone contemporaneamente e nel frattempo risolverò qualcos'altro. In modo asincrono, nell'arco di 2 giorni, arriveranno più risposte diverse, in base alle quali prenderò una decisione solo in un secondo momento.
- Ho bisogno di liberarmi di PHP sul server per un lungo periodo, quindi sto gradualmente riscrivendo una pagina alla volta in React. Sto gradualmente spostando i siti nel cloud e li sto costruendo su Nect generato staticamente.

Nessuna delle destinazioni è una destinazione finale. Se si lavora con una superficie e non con un punto, è molto più probabile che lo si colpisca. Allo stesso tempo, la motivazione funziona bene in questo caso, perché la cosa peggiore che possa accadere è raggiungere l'obiettivo e poi non avere qualcosa a cui guardare in futuro.

È anche importante dire che per avere una mentalità simile è necessario avere un ambiente abbastanza stabile in cui si possano commettere errori. Questo approccio non può garantire una scadenza specifica per risolvere un singolo compito. D'altra parte, può ottimizzare la risoluzione del maggior numero di compiti paralleli nel minor tempo possibile, anche se non necessariamente nell'ordine in cui sono arrivati nella coda.

Il principio potrebbe essere paragonato al funzionamento dei calcoli nella programmazione parallela, ad esempio. In breve, si scompongono i compiti in singoli thread che risolvono diversi sottocompiti contemporaneamente e, una volta ottenute tutte le risposte, si può comporre la soluzione.

Distribuzione di nuove versioni del software
--------------------------------

Mi capita spesso di lottare con questo problema ovunque.

Il modo in cui lo sviluppo del software viene solitamente gestito è quello di avere una versione stabile che viene utilizzata in produzione dagli utenti, poi un ambiente di test in cui viene effettuato il nuovo sviluppo e, di tanto in tanto, si spostano le novità dall'ambiente di test alla produzione, che viene chiamata release. Questo processo è relativamente sicuro e lento.

Poiché sono passato gradualmente a un'architettura micro-frontend, in cui il frontend dell'applicazione (ciò che l'utente vede) è completamente separato dal backend (dove avvengono i calcoli e l'elaborazione dei dati), il processo di rilascio può funzionare in modo diverso.

Ho ancora un ambiente di produzione che funziona sempre. Da qui, viene creato un nuovo ramo per ogni attività che si occupa di una caratteristica specifica. Poiché utilizzo Vercel (un ottimo provider cloud) per ospitare il frontend, posso avere un URL personalizzato per ogni ramo di sviluppo.

Una volta testata una nuova funzionalità, questa viene immediatamente unita e distribuita in produzione in un processo asincrono. Pertanto, può accadere che ogni giorno vengano distribuite 15 nuove versioni.

Molto di questo si ricollega a un'idea che mi è stata trasmessa all'LMC: "Una funzione che fornisci a un cliente domani, anche se in uno stato imperfetto, ha per lui un valore molto più alto di una funzione perfettamente debuggata ma che sarà disponibile solo tra un anno". Ma questo può funzionare solo se si dispone di un modo per distribuire rapidamente le notizie - tipicamente lo sviluppo web.

Ciò che mi piace molto del framework Next.js (costruito su Recto) e di Vercel è il principio che si collega il repository GitHub direttamente a Vercel, e a ogni commit c'è una compilazione automatica, i test e poi il deploy in produzione quando tutto è a posto. Così lo sviluppatore non deve preoccuparsi di nulla e l'applicazione viene distribuita ogni ora senza alcuno sforzo. È molto importante formalizzare le cose di routine e poi automatizzarle.

Pubblicazione di contenuti
----------------

Ho suddiviso decine di argomenti e post con me in Apple Notes. Per ogni argomento, mi vengono continuamente nuove idee da annotare e ordinare per tag. Quando vengono generate più note per un argomento, le converto in capitoli e poi converto il gruppo di capitoli in articoli e post su FB.

Caratteristiche di questo principio:

- Non so in anticipo quanti articoli pubblicherò mai,
- Ma so anche che potrebbero essere molti,
- Sono in grado di garantire che ogni post sia ricco di informazioni, approfondimenti e idee maturate nel tempo (questo post ha richiesto circa 3 mesi per maturare),
- È sostenibile e mi piacerà perché non dovrò scrivere una tonnellata di testo in un unico momento rischiando di dimenticare qualcosa.

E poi il processo di pubblicazione.

Pubblico molti contenuti in silenzio sul web, in modo che Google li noti per primo e la pagina venga indicizzata. Quando ha una buona copertura di parole chiave e mi soddisfa nel complesso, l'argomento va sui social media, per esempio.

Per molti argomenti, so che non saranno di interesse e che è più probabile che vi infastidiscano. Ma allo stesso tempo voglio che siano online perché qualcuno potrebbe cercarli in futuro. In questo caso, l'articolo rimane solo sul web. Un esempio di questi articoli sono i vari articoli in sovrimpressione, che collegano l'intera area tematica in modo da coprire il maggior numero possibile di argomenti sul sito. Gli articoli di copertina sono spesso più tecnici e noiosi. Oppure sono categorie generate dalla macchina, in cui organizzo più articoli in argomenti, coprendo così il maggior numero possibile di parole chiave, che qualcuno potrebbe poi voler cercare.

Conoscenza, educazione, test
------------------------------

Mi piace giocare con tecnologie per le quali non è chiaro fin dall'inizio a cosa serviranno un giorno.

Come la traduzione automatica. A prima vista, non ha senso tradurre decine di articoli in lingue straniere per lettori con cui non sono in contatto. D'altra parte, può essere uno dei passi per prendere una decisione in futuro per la quale devono essere soddisfatti i prerequisiti.

In generale, nessuno sa come sarà il futuro. Perciò mi sembra estremamente sensato coprire il maggior numero possibile di possibilità che si vogliono comprendere, almeno superficialmente, e magari affrontare in futuro. È bene pensare ai passi non solo come a un compito risolto, ma come a un processo evolutivo infinito che non ha una destinazione finale.

Questo approccio funziona per la realizzazione di progetti personalizzati?
--------------------------------------------------------

Il più delle volte, no.

È necessario che il cliente sia un vostro partner e che entrambi vogliate fornire la migliore soluzione possibile, anche se sapete in anticipo che non scoprirete quale sia fino alla fine.

Nel nostro settore si chiama sviluppo agile e questi principi si basano su di esso. D'altra parte, ho osservato che lo sviluppo agile, così come lo conosco, non mi si addice direttamente e mi piace proporre delle alternative ricurve.

Con l'agilità, vedo molto il principio dell'impegno in termini di chi risolve cosa all'interno di un particolare sprint. Per me ha più senso costruire l'ambiente in modo da avere un enorme arretrato di compiti che vengono risolti in base a ciò che è più necessario in questo momento o che ho la capacità mentale di fare. Quando sono in viaggio, ad esempio, tendo ad affrontare compiti di pensiero più evolutivi che posso svolgere all'aperto senza computer. Quando sono di nuovo in ufficio, affronto compiti più intensivi dal punto di vista degli algoritmi e della matematica, perché posso concentrarmi completamente e investire molta energia mentale.

Non consiglio questo principio se state creando un e-shop nuovo di zecca o se la vostra attività sta fallendo. Avete bisogno di un ambiente stabile che funzioni da solo, e sarete solo dei gioielli. Ma allo stesso tempo sapete che se non fate nulla, un giorno il vostro ambiente stabile finirà.
