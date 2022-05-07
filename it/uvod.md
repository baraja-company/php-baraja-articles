Introduzione a PHP
==================

> id: '66b7fd22-6195-4999-ad8d-b799afdc1876'
> slug:
> 	cs: uvod
> 	it: introduzione-a-php
> 
> perex:
> 	- 'Úvodní tutoriál do jazyka PHP, možnosti jazyka, informace o tvorbě webu v PHP.'
> 	- 'Tutorial introduttivo al linguaggio PHP, opzioni del linguaggio, informazioni sullo sviluppo web in PHP.'
> 
> publicationDate: '2019-09-29 19:25:06'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: db6ba8fe6d5ca28aab62d7302c1d7527

Questo è un corso online per imparare il PHP, progettato per l'istruzione generale. È disponibile gratuitamente su questo sito. I testi non devono essere usati in nessun corso a pagamento senza una conferma scritta. Potete utilizzare i campioni utilizzati in tutto il sito nella vostra applicazione senza ulteriori restrizioni. [Termini e condizioni](https://baraja.cz/vseobecne-obchodni-podminky).

Aggiornamento da HTML
--------------

"Aggiornamento"? Suona più come una rotazione dei media, e in effetti lo è.

Non c'è nessun aggiornamento. PHP mantiene tutte le funzionalità e le caratteristiche di un documento HTML puro, aggiunge solo nuove opzioni di scrittura e distribuzione. Fantastico, vero?

Sapete già dallo sviluppo di pagine HTML statiche che il codice consiste di tag, che sono definiti come parole chiave racchiuse tra parentesi quadre (per esempio, `<b>Hello!</b>` esprime il testo in grassetto **Hello!**).

PHP è inserito nella pagina HTML come tag `<?php` e `?>`, all'interno del quale è scritta altra logica applicativa. È importante che PHP abbia la sua propria sintassi (le regole con cui il codice è scritto) e, a differenza di HTML, non è tollerante agli errori.

Esempi specifici di come farlo e cosa significa ogni tag saranno dati più avanti. Per cominciare, è importante capire i principi generali di come funziona PHP sul server e come viene elaborato il codice.

Il flusso di comunicazione tra l'utente e il server
---------------------------------------

Per una normale pagina HTML, funziona più o meno così:

> L'utente invia una richiesta (chiede una specifica pagina HTML), il server guarda il disco e rimanda esattamente ciò che è memorizzato. Non succede niente di speciale, e non aspettatevi niente di più. Le pagine saranno solo statiche, senza possibilità di interazione con il server.

Se aggiungiamo PHP, però, inizieranno ad accadere meraviglie:

> L'utente richiede nuovamente la pagina. Il server apre il file sul disco, ma vede che non contiene solo HTML puro, ma anche tag speciali che indicano uno script PHP. Quindi prima li valuta e poi invia ciò che PHP ha generato.

La valutazione del codice PHP viene fatta di default ogni volta che la pagina viene caricata, in futuro si imparerà a memorizzare nella cache il codice (memorizzarlo compilato per un'elaborazione più veloce).

La differenza tra l'elaborazione di script PHP e C/C++
------------------------------------------

Potresti aver imparato ad usare il linguaggio `C` o `C++` a scuola. PHP è direttamente basato sulla sintassi del linguaggio `C`, e all'interno del kernel viene usato il linguaggio `C`, quindi è bene conoscere alcuni punti in comune e al contrario le differenze.

Il principio di base dei comuni programmi compilati (che vengono eseguiti localmente direttamente sul vostro computer o smartphone) è quello di caricare la logica dell'applicazione nella memoria operativa, che comunica direttamente con il sistema operativo, che riceve gli input dall'utente e poi visualizza l'output del programma. L'importante è che il programma venga eseguito in modo isolato dall'inizio alla fine.

PHP inizia con ogni richiesta di rendering della pagina, ricarica tutto il codice e i dati ogni volta, e poi esce. Gli script PHP hanno quindi una vita letteralmente da yuppie e tipicamente esistono solo per decine di millisecondi.

Il vantaggio di questo approccio è un più alto livello di isolamento - se qualcosa va storto, tutto viene ricaricato con il prossimo caricamento della pagina. D'altra parte, questo approccio ha requisiti di prestazione più elevati perché dobbiamo connetterci ripetutamente al database, leggere i file dal disco, e così via, per esempio.

In futuro, imparerai che puoi mantenere gli script PHP caricati nella memoria operativa utilizzando l'estensione `OP Cache`, che la maggior parte dei nuovi server (da PHP 7.1 in poi) hanno impostato nella configurazione di base.

Scaricare script PHP stranieri
--------------------------

Una domanda relativamente comune che discutiamo con gli studenti è come scaricare script PHP stranieri dal server e guardare il loro codice sorgente. Questa domanda è preceduta dalla considerazione che il codice HTML di una pagina può essere facilmente visualizzato in un browser web.

La risposta è che **gli scriptPHP non possono essere scaricati**. Questo perché il codice PHP viene prima valutato sul server web, e poi il codice HTML generato (o altro output) viene inviato al browser dell'utente. Pertanto, solo l'output dello script PHP può essere scaricato, non lo script stesso.

Come funziona la generazione in HTML?
---------------------------------

Non funziona letteralmente così, si naviga in pagine HTML. Lo script PHP deve essere sempre in un file PHP.

Fino ad ora, la maggior parte di voi era abituata a creare enormi cartelle piene di file con estensione **.html**. Ora ci saranno molti meno file. Nel caso estremo, può essere un singolo file.
