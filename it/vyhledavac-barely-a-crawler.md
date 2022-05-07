Algoritmo del motore di ricerca Internet - A malapena un crawler
================================================================

> id: '5ad042bf-7fda-4b2e-a38c-762169d0b594'
> slug:
> 	cs: vyhledavac-barely-a-crawler
> 	it: algoritmo-del-motore-di-ricerca-internet---a-malapena-un-crawler
> 
> publicationDate: '2016-09-11 12:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '5daf3854688c39f9e0071a93ab11dd4e'

Nella lezione di oggi, parleremo dei barili di dati, della loro struttura, di StopSlovas e infine descriveremo i crawler.

Barili di dati
-------------

Questo è un tipo di dati speciale che risiede su più server contemporaneamente in più copie. Questi sono di solito file ad alta densità di dati di centinaia di GB e sono lenti da leggere (che è il motivo per cui sono divisi in parti) e praticamente impossibili da modificare. Se vogliamo fare anche un minimo cambiamento, dobbiamo ricomputare tutto il barile. Per esempio, il motore di ricerca Seznam può ricalcolare i barili di dati al massimo una volta ogni pochi giorni o settimane, mentre Google ricalcola una volta ogni poche ore (e solo alcune parti, mai tutto insieme).

I barili contengono parole e le loro occorrenze su Internet. Si può dire che si tratta di dati grezzi per i risultati di ricerca in una forma non ancora ordinata (a volte sono parzialmente ordinati per importanza relativa) e preparati in anticipo. La ricerca stessa ha luogo molto prima che l'utente chieda la query di ricerca ed è qui che vengono preparati tutti i risultati. La ricerca stessa sarebbe impossibile in tempo reale, quindi il motore di ricerca fondamentalmente legge i risultati già preparati qui (la ricerca è fatta dal componente indexer, che sarà discusso in un capitolo separato).

I barili contengono tutte le informazioni di base necessarie per costruire risultati di ricerca per ogni parola che si trova su Internet, quindi i problemi di prestazioni devono essere affrontati quando si legge in sequenza. In pratica, i barili tendono ad essere memorizzati in lotti su più server e anche nella RAM per un accesso più veloce. Per esempio, il motore di ricerca Google non legge affatto i barili dal disco, ma li tiene completamente in RAM e ne conserva solo una copia su disco nel caso in cui i dati vengano persi dalla RAM.

I grandi motori di ricerca con risorse sufficienti conservano anche i cosiddetti "barili d'oro", che contengono le pagine più importanti su Internet e vengono cercate in caso di un carico di ricerca pesante. Per esempio, se diversi server di ricerca si bloccano, i barili d'oro si occupano temporaneamente delle ricerche. Il motore di ricerca potrebbe non trovare tutti i risultati, ma almeno la ricerca non sarà completamente interrotta, ma solo il numero di documenti ricercati sarà temporaneamente limitato. È anche necessario aggiornare regolarmente i barili, poiché il numero di pagine su Internet è in costante aumento. Dato che i barili sono di solito nell'ordine dei gigabyte di dimensione, non è possibile eseguire una manutenzione incrementale, ma è necessario ricostruire il tutto una volta ogni volta stabilito. Pertanto, il motore di ricerca mantiene un barile ausiliario dove tutti i dati sono sotto forma di una grande tabella da cui vengono costruiti i barili - per esempio, Google costruisce i barili circa ogni 12 ore. La grande sfida è proprio che i barili devono essere almeno parzialmente ordinati e riflettere lo stato reale di Internet. Così, finché il motore di ricerca non aggiorna i barili, l'utente non troverà nuove pagine web.

Struttura a barile
----------------

In pratica, un barile è memorizzato come un grande file di testo che contiene l'ID di ogni documento in cui la parola di ricerca si verifica e la posizione dell'occorrenza della parola attualmente ricercata.

Esempio di un barile molto piccolo con tre pagine:

```txt
[1:5,8,12,27,30;5:1,3,6;12:4,8,10]
```

Questo barile di esempio contiene pagine con identificatori `1`, `5` e `12`. I numeri associati indicano la posizione dell'occorrenza della parola nel documento. Un tale barile cattura solo le occorrenze di una singola parola, quindi ogni parola su Internet ha il suo barile. Quando si cercano più parole chiave, è necessario leggere più barili (uno diverso per ogni parola) e poi eseguire le operazioni secondo l'albero binario (più nei capitoli precedenti).

L'intersezione si fa di solito passando attraverso uno dei documenti e cercando nell'altro. Se i barili sono grandi, potrebbero non essere letti completamente, quindi i motori di ricerca spesso riescono a leggerne solo una parte e poi devono restituire il risultato, quindi ha senso ordinare i barili almeno parzialmente, in modo che le pagine più importanti (che l'utente probabilmente sta cercando) siano all'inizio del barile e tutto il resto sia alla fine del barile.

StopLead
---------

Si potrebbe pensare che per alcune parole i barili potrebbero essere davvero enormi e quindi difficili da lavorare. Per esempio, la parola `a` o la parola `1` si trova in più della metà dei documenti, eppure non hanno alcun significato per il ricercatore (perché vengono cercate raramente e richiedono più parole circostanti). Tali parole sono chiamate Stopwords.

Il problema con le StopWords è che non possono essere ricercate tutte, e i motori di ricerca mostrano quindi solo una realtà distorta di ciò che Internet appare effettivamente per la parola ricercata.

Un esempio di ricerca con StopSlovy è la query `Prague 1`.

Il motore di ricerca dà solo `3.020.000 risultati` per questa query. In realtà, però, ci sono molti più siti di questo tipo. Tuttavia, non tutti sono ricercati per motivi di prestazioni.

Cerca le opzioni di accesso con StopSlovy:

- Non indicizzarli affatto e non fare canne per loro (lo fanno i piccoli motori di ricerca)
- Indicizzarli, ma immagazzinare solo le pagine rilevanti nei barili (questo è fatto da Google e Seznam, per esempio)
- Indicizzarli come parole regolari e creare barili completi (questa soluzione ha il problema che i barili possono facilmente superare la dimensione di un normale disco rigido e quindi non possono essere salvati e possono richiedere secondi per essere letti - questa soluzione non è quindi usata in pratica o solo per motori di ricerca piccoli e specializzati)

In generale, non esiste un approccio universalmente corretto e anche Google non lo usa perfettamente. Questo è un problema così grande che potrebbe richiedere una vita intera per essere risolto. Per gli scopi di questo articolo, tuttavia, questa descrizione generale è sufficiente.

Crawler (robot, anche ragno)
---------------------------

Un crawler è un programma che striscia sistematicamente Internet e tiene traccia di quali parole appaiono su quali pagine e raccoglie anche informazioni ausiliarie (chiamate anche meta informazioni). Un crawler di solito gira su molti server, perché la scansione di Internet è impegnativa in termini di larghezza di banda della rete e quindi molte pagine devono essere scansionate simultaneamente. Google stima che fino a 30.000 pagine sono scansionate simultaneamente in qualsiasi momento.

Prima di aprire una pagina, Crawler guarda un database con gli indirizzi a cui intende andare in futuro. Se è già stato all'indirizzo, aggiornerà solo i suoi record (va sui siti principali ogni poche ore, sui siti di notizie ogni pochi minuti, sui siti regolari una volta al mese al massimo).

Se il robot arriva a un nuovo indirizzo che non conosce, guarderà i link contenuti ad altri documenti (pagine). Per ogni link, calcola la sua importanza, e se è un link importante, arriva anche alla pagina dopo.

Per esempio, List scansiona solo alcuni livelli di link su ogni sito in questo modo, mentre Google cerca di scansionare l'intero sito, compresi i documenti non importanti, rendendolo il motore di ricerca più completo su Internet.
Il robot cerca anche di classificare la pagina mentre striscia e calcola il suo "rank", che è una rappresentazione numerica di quanto la pagina sia importante su Internet. In passato, Google ha usato PageRank, che ha un range da 0 a 10. Google calcola il PageRank 10 solo per se stesso e per siti come microsoft.com o w3c.org. Oggi calcola il valore in modo diverso (intelligenza artificiale e matematica vettoriale).

Il più alto PageRank nella Repubblica Ceca è Seznam.cz con un valore di 7. Il rango ha una grande influenza su quanto spesso un robot visiterà la pagina e quanto sarà in alto nei risultati di ricerca. I ranghi sono calcolati solo dai backlink che puntano alla pagina desiderata.

Il crawler non fa nient'altro con i dati, si limita a scaricarli da Internet e a memorizzarli in un database dove attende il processo di indicizzazione.
