Rifattorizzare un progetto legacy PHP: come recuperare il debito tecnologico
============================================================================

> id: '8f37305a-e9dd-4b1e-9f53-04f0fca648f7'
> slug:
> 	cs: refaktoring-legacy-php-projektu
> 	it: rifattorizzare-un-progetto-legacy-php-come-recuperare-il-debito-tecnologico
> 
> publicationDate: '2021-05-11 20:45:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: b13673b23a86bc82a88636e4a9464b4b

Quando mi consulto con proprietari di progetti esperti e competenti, mi imbatto spesso nella questione della sostenibilità a lungo termine di un progetto digitale. Molti grandi progetti che vanno oltre i 3 anni di sviluppo iniziano a diventare internamente obsoleti e non più sostenibili - ora immaginate un team di sviluppatori con diversi livelli di conoscenza, esperienza e, soprattutto, diligenza.

Per mantenere il vostro progetto digitale al livello TOP tecnicamente, avete bisogno di:

- **Sviluppatori competenti e un project manager** che hanno una grande comunicazione e tutti sanno cosa stanno facendo e come impatta sugli altri,
- **Strumenti funzionali e flusso di lavoro** che tutta la squadra conosce e adatta il suo lavoro agli altri di conseguenza,
- **Compiti automatizzati** che forniscono una routine quotidiana che è così facile da dimenticare che è estremamente importante.

Se state sviluppando un grande progetto, semplicemente non è facile. È importante fare le cose bene, ma è più importante fare le cose giuste.

Cos'è il refactoring e perché ne avete bisogno
---------------------------------------

Il concetto di refactoring comprende un insieme di attività che modificano l'aspetto e la logica interna del codice di un programma senza influenzare l'ambiente circostante. Si può pensare al processo di refactoring come a una pulizia natalizia - rimane lo stesso, ma è molto più organizzato, e le cose non necessarie vengono buttate via. Con il refactoring, è anche importante tenere a mente che non è un evento una tantum, ma un processo a lungo termine che si deve ripetere in cicli regolari. Se non lo fate, vi troverete di fronte a ogni sorta di strani errori, perdite finanziarie, problemi di onboarding e grida di protesta.

Se riesci a mantenere il progetto stabile e aggiornato, otterrai dei grandi benefici:

- Il progetto sarà **più sicuro**. Le patch per i bug e le vulnerabilità di sicurezza sono rilasciate regolarmente nelle librerie che usate. Dovete affrontare la sicurezza, è una delle funzioni vitali di base di un progetto sano.
- Il codice e l'architettura interna diventeranno più semplici e meglio pensati. Sarete in grado di trovare meglio i bug, e molti bug possono anche essere prevenuti.
- Con un codice semplificato, puoi anche portare sviluppatori junior per <a href="https://www.youtube.com/watch?v=1wZtdIb-Ppk">ridurre significativamente</a> il tuo budget complessivo.
- Potreste scoprire che state facendo alcune cose troppo complicate ed eccessivamente complicate - il progetto sarà semplificato nel complesso.

Per rimanere in pista con un grande progetto digitale, bisogna correre. Ma tu vuoi crescere.

Come rifattorizzare in modo sicuro
-------------------------

Ogni refactoring è una grande scommessa che potrebbe non ripagare.

Mi assicuro sempre un ambiente stabile molto prima di intraprendere il refactoring.

Per la stabilità, avete bisogno soprattutto di un **log funzionale degli errori**. Per progetti semplici, avete solo bisogno di <a href="https://tracy.nette.org">Tracy</a>, che registra gli errori in un file HTML. Per progetti più avanzati, entrano in gioco strumenti come <a href="https://sentry.io/welcome/">Sentry</a> o <a href="https://rollbar.com">Rollbar</a>. Personalmente uso Tracy su tutti i progetti, altri strumenti a seconda del tipo di progetto.

Circa un mese prima del refactoring, inizio a tracciare i bug e creo un foglio di calcolo dei bug conosciuti su cui concentrarmi in seguito. È sempre importante sapere quali bug sono stati introdotti dal refactoring e quali il progetto conteneva già in passato. Questo aiuterà poi a difendere meglio i risultati del lavoro al cliente.

Una volta che so, grazie al log, che sto lavorando su un ambiente relativamente stabile, possiamo passare al lavoro duro. Altre voci includono descrizioni di metodi in base a quanto rischio si nasconde dietro di loro.

Correzione dello stile di codifica e dello standard di codifica
-----------------------------------

La maggior parte dei progetti non ha uno stile coerente di formattazione del codice. Questo è un grande errore. Un progetto ben scritto sembra il progetto di una persona, dove tutte le cose sono scritte allo stesso modo.

Gli errori di formattazione di base sono ben corretti dallo strumento <a href="https://doc.nette.org/en/3.1/code-checker">Nette Code Checker</a> che eseguo regolarmente su tutto il progetto.

Coding Style, d'altra parte, copre come il codice è formattato e l'indentazione delle singole espressioni del linguaggio. Lo standard <a href="https://www.php-fig.org/psr/psr-12/">PSR-12</a> è molto usato nel mondo PHP, e molti altri standard sono basati su di esso. Raccomando di usare.

Alcuni progetti combinano goffamente <a href="/spazi-e-tabs">tabs e spazi</a>. Per prevenire efficacemente la rottura della convenzione degli spazi bianchi (e altri errori), crea un file `.editorconfig` nella radice del progetto che dica al tuo editor come formattare il codice appena scritto.

Personalmente uso questa configurazione:

```txt
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{php,phpt}]
indent_style = tab
indent_size = 4
```

Inoltre, correggi tutti i <a href="/apostrofi-e-note di trasporto">note di trasporto in apostrofi</a>. Può essere fatto automaticamente.

Puoi anche controllare la formattazione del tuo codice automaticamente, ho preparato una <a href="https://github.com/baraja-core/sandbox/blob/0db1f41068b6cedf79500c6a8b0ac26eae94a8eb/.github/workflows/coding-style.yml">dimostrazione completamente funzionale su GitHub</a> per questo. Se avete anche bisogno di correggere automaticamente il codice, questo può essere fatto con lo stesso strumento. PhpStorm è anche molto bravo a correggere automaticamente il codice direttamente, anche in blocco su tutto il progetto.

Come fissare lo stile di codifica per grandi progetti
----------------------------------------------

Per i grandi progetti, raccomando di fare la formattazione automatica del codice un modulo alla volta, poiché questo può creare un numero enorme di conflitti in Git. Pertanto, la migliore strategia è quella di correggere quante più linee possibili con un singolo grande commit, spingere quel commit direttamente al master, e poi distribuire il cambiamento agli altri rami.

Se i conflitti sono troppo grandi, ha senso formattare il codice sul master prima, e poi di nuovo su ogni grande ramo dove ci sono molti cambiamenti. Molte linee cambiate si annulleranno a vicenda (fate un fast-forward), quindi risolverete pochissimi conflitti, o a volte nessun conflitto.

Se non fate nulla, il codice sarà ancora cattivo. Le riscritture iterative nel corso di molti anni sicuramente non funzionano, perché ogni fusione introduce la necessità di correggere decine di linee dove nessun cambiamento è effettivamente avvenuto, e complica inutilmente sia la revisione del codice che le potenziali revisioni dei cambiamenti.

PhpStan - analisi statica del codice e correzione degli errori di tipo
------------------------------------------------------

Prima di imbarcarsi in una correzione logica su larga scala, è molto importante rivedere e fissare le **funzioni di base del ciclo di vita del progetto**. Queste includono cose così ovvie che ci si aspetta da qualsiasi progetto, ma che a volte non vengono soddisfatte.

Per esempio:

- Tutte le classi, i metodi e le funzioni devono esistere
- L'ereditarietà delle classi non deve essere interrotta
- Le classi devono implementare l'interfaccia o l'interfaccia antenata utilizzata. Allo stesso tempo, non si devono ereditare le classi "finali".
- Non dovete chiamare funzioni non sicure come `eval()`, `shell_exec()`, `var_dump()` e così via. E se li chiami comunque, deve essere esplicitamente dichiarato nel commento
- Dovete sempre catturare le eccezioni e non lasciare che l'intera applicazione vada in crash

La soluzione a questo problema è installare <a href="https://phpstan.org">PhpStan</a> nel progetto e fissarlo **almeno al livello 1**. Sì, è difficile, e sì, è un sacco di lavoro. Ma se non lo si fa, ogni refactoring diventa una roulette russa, e lo sviluppatore spera solo che il danno sia il minimo possibile.

Il livello base di PhpStan non richiede, per esempio, che i tipi di dati e altre cose formali esistano ovunque, cosa che affronteremo in futuro. D'altra parte, potrebbe non essere il caso che una funzione o un metodo restituisca un certo tipo di dati ma asserisca qualcos'altro nel typehint.

Rettore - riparazione iterativa sicura
-----------------------------------

Un noto detto di programmazione dice che prima di aggiungere un nuovo errore al sistema (come lanciare un'eccezione), dovremmo prima aggiungere quell'errore come avvertimento, e solo dopo trasformarlo in un errore fatale se non si manifesta per molto tempo.

È lo stesso con i tipi di dati. Quando rifattorizzo il codice sconosciuto, lascio che strumenti automatici come <a href="https://getrector.org">Rector</a> aggiungano prima le annotazioni di commento al codice, che non rompe nulla, ma aiuta a chiarire cosa sarà dove in seguito. Questi commenti sono poi ascoltati da PhpStan, che può essere usato per verificare con sicurezza che non abbiamo rotto nulla e che si tratta di una modifica sicura.

Generalmente aggiungo commenti alle proprietà e agli argomenti in un commit, poi aspetto forse un mese, e quando tutto funziona a lungo termine, ricorro alla riscrittura a tipi di dati fissi in posti dove non c'è stato un problema a lungo termine.

Vedere l'articolo <a href="https://tomasvotruba.com/blog/2019/07/29/how-we-completed-thousands-of-missing-var-annotations-in-a-day/">Come abbiamo completato migliaia di annotazioni @var mancanti in un giorno</a>.

Per ulteriori informazioni su <a href="https://github.com/rectorphp/rector">Rector, vedere GitHub</a>.

Aggiornamento delle dipendenze
----------------------

Prima di addentrarci nell'aggiornamento delle dipendenze dei pacchetti o anche delle versioni di PHP, è importante ricercare e imparare a usare <a href="/github-azioni-best-ci-for-2021">test automatizzati</a>.

Se i test funzionano, possiamo andare allo strumento <a href="/composer">Composer</a> per eseguire l'aggiornamento. Se puoi, arruola l'aiuto di un <a href="https://dependabot.com">Dependabot</a> robot che aggiorna automaticamente il `composer.json` del tuo progetto e può controllare i cambiamenti di compatibilità.

Se avete una grande serie di cambiamenti in arrivo, fateli sempre lentamente e incrementateli versione per versione. Non aggiornare mai molti pacchetti in una volta sola. Dopo ogni aggiornamento, scansionate l'intero progetto con PhpStan e correggete i bug. È un processo lungo che richiede diverse ore, ma la posta in gioco è alta.

Dove andare dopo
-------

I passi successivi sono individuali a seconda del tipo e dello stato del progetto. In generale, il codice ben progettato scritto da sviluppatori senior è mantenuto ordini di grandezza meglio del codice comprato a buon mercato da un junior che ha lavorato in un'agenzia di media che ha lo sviluppo web più come un'attività secondaria, per così dire.

Incrociamo le dita! Sarà dura, ma la supererete.
