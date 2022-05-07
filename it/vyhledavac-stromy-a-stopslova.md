Algoritmo del motore di ricerca Internet - Alberi e StopLead
============================================================

> id: '11ec73f2-e505-4338-89aa-8307a55b7ba0'
> slug:
> 	cs: vyhledavac-stromy-a-stopslova
> 	it: algoritmo-del-motore-di-ricerca-internet---alberi-e-stoplead
> 
> publicationDate: '2016-09-11 10:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d011d2d6943271ca5e45c3d3cdf03d61

Ci sono 5 milioni di nuove pagine aggiunte a Internet ogni secondo, e questo tasso è in continuo aumento. Per dare un po' di ordine a questo enorme mare di informazioni e per trovare qualcosa in esso, ci sono i motori di ricerca. Il seguente lavoro ha lo scopo di introdurre il tema della ricerca e spiegare l'intero processo dalla creazione di una nuova pagina fino a trovarla in un motore di ricerca.

Il compito di trovare e ordinare un insieme di miliardi di documenti non è facile. Solo Google ha bisogno di 300.000 server web per gestire questo compito in poche ore. In effetti, la ricerca della tua domanda avviene molto prima che tu la chieda. Google ha già memorizzato i risultati di ricerca che chiederai nei prossimi giorni.

Architettura del motore di ricerca
------------------------

<img src="{$baseUrl}/images/fulltext-schema.png" alt="Architettura del motore di ricerca comune per un massimo di 50 milioni di documenti" class="w-100 mb-3">

Un motore di ricerca ben progettato contiene molti componenti, che sono dell'ordine di diverse centinaia. Questo diagramma descrive gli elementi di base che un motore di ricerca deve contenere come minimo per funzionare. Questa è un'architettura vecchia e non più utilizzata che ha funzionato fino al 2005 circa, quando c'erano solo pochi milioni di documenti sul web. Questa architettura non permette una quantità "infinita" di contenuti e anche un possibile downtime dei server di ricerca (ricerca di base). Nel caso in cui un singolo componente fosse fuori uso, l'intero sistema cesserebbe di funzionare e il motore di ricerca non sarebbe disponibile.

Una descrizione completa delle tendenze attuali nella progettazione di nuove architetture va oltre lo scopo di questo articolo, poiché queste sono di solito segreti commerciali di grandi aziende. Lo scopo di questo articolo sarà quello di spiegare come funziona questa architettura di base. I singoli componenti sono spiegati in dettaglio nei capitoli seguenti.

Input di query
------------

Il componente più visibile anche per gli utenti comuni è l'input della query, che attualmente è più spesso rappresentato come una casella di testo. Il campo di input si trova in una pagina speciale che di solito è solo per l'input.

Nella fase successiva, l'utente inserisce la stringa di ricerca e invia il modulo ai server del motore di ricerca, iniziando così la comunicazione con il componente di erogazione, talvolta chiamato anche Ricerca principale. I server di erogazione sono costruiti per gestire carichi enormi da parte degli utenti e devono essere in grado di gestire tutti i cercatori contemporaneamente.

L'architettura di un server di dispatch è di solito un semplice server Apache con un'installazione di base e un'eccellente larghezza di banda di rete. Quando una query entra nel server, viene elaborata attraverso alberi decisionali di regressione e viene creata una "mappa di query" che cattura la semantica completa intorno agli altri significati per rendere chiaro ciò che l'utente sta effettivamente cercando. I metodi di riscrittura delle query sono troppo al di là dello scopo di questo articolo, quindi ci limiteremo a mostrare descrizioni generali di ciò che la riscrittura può e non può essere.

Considerate la seguente query:

```txt
"O pejskovi a kočičce"
```

Questa query sarà convertita in un albero binario che cattura la sua essenza semantica:

<img src="{$baseUrl}/images/fulltext-tree.png" alt="Symbolic parse tree diagram" class="w-100 mb-3">

L'intero scopo di un albero di analisi è quello di catturare come un motore di ricerca guarderà i risultati della ricerca. Questo albero, insieme alla query, viene inviato ai server di ricerca ausiliari (etichettati come ricerca di base in questo documento), dove le singole parole chiave vengono cercate (lettura dell'indice) e poi ordinate secondo le regole (per operazioni).

| Operatore | Operazioni di ordinamento |
|----------|------------------
| E. E. Intersezione.
| OR | Somma |
| NOT | Complemento |

Non c'è un vero e proprio ordinamento dei risultati della ricerca, questo componente esegue solo operazioni binarie veloci su enormi quantità di dati (spesso milioni di record) e fondamentalmente confronta solo gli ID dei singoli documenti.

Come funziona l'effettivo ordinamento dei risultati di ricerca per la pagina dei risultati sarà discusso nelle sezioni seguenti. Il server di output è semplicemente utilizzato per combinare molti dati da diversi server di ricerca per aumentare le prestazioni complessive e distribuire il carico, e poi alimenta i valori rilevati nella pagina dei risultati (questa è chiamata "pagina web"), contenente informazioni composite da decine di server.

Riscrivere in un albero binario
-----------------------

Come menzionato nel capitolo precedente, l'intera ricerca è costruita su alberi binari che ti dicono come saranno i risultati e cosa cercare. Tutta la "logica" e "l'intelligenza" di un motore di ricerca dipende direttamente dalla qualità del programma che crea l'albero, cioè il descrittore.

Un albero costruito correttamente dovrebbe contenere tutte le meta-informazioni necessarie sulla query in una forma tale da poter essere facilmente elaborata anche con l'aiuto della lettura sequenziale del disco e dovrebbe eliminare gli accessi casuali alla memoria, quindi di solito contiene le singole parole di ricerca (dell'utente), le loro trascrizioni (e forme ortografiche) e infine i collegamenti tra le parole, cioè le loro distanze relative nel testo.

Metodi di trascrizione delle query
---------------------

Il modo più elementare per trascrivere una query è quello di analizzarla in singole parole (secondo gli spazi), che saranno lavorate ulteriormente. Il secondo passo è cercare le StopWords e sostituirle con un puntatore variabile (perché, come mostreremo più avanti, non ha senso cercare le StopWords).

Abbiamo la seguente query: "Su un cane e un gatto", la sua trascrizione nel suo stato più elementare si presenta così:

```txt
"O (and) pejskovi (and) a (and) kočičce"
```

Il secondo passo è quello di eliminare le parole che non hanno rilevanza per la ricerca. Naturalmente, per esempio, la parola "circa" o "e" è significativa per noi umani, ma non per una ricerca su database, perché può essere sostituita da un'altra parola e i risultati elencati. L'obiettivo non dovrebbe essere quello di trovare una corrispondenza letterale della query con l'indice, perché questo porterebbe a cattivi risultati, perché spesso le parole nel testo su Internet sono in diverse grafie, e questo metodo cerca di trovare risultati in altre forme rispetto a quelle che abbiamo ottenuto dall'utente. Questa è quindi la prima indicazione di un motore di ricerca "intelligente".

Queste parole senza senso sono spesso chiamate "stopwords", ogni motore di ricerca dovrebbe tenere un indice di tali parole e questo indice dovrebbe essere aggiornato frequentemente (manualmente).

```txt
["pejskovi (and) * (and) kočičce"]
```

A questo punto, stiamo cercando 2 parole diverse ("cane" e "gatto") con un'altra parola in mezzo (internamente, la contrassegniamo con un asterisco).

Lo scopo di questa modifica non è quello di aumentare la qualità della ricerca, ma di aumentare le prestazioni. Questo perché quando cerchiamo una parola che è troppo frequente, c'è un carico rapido, e questa modifica aiuta il processo - soprattutto non cercando una parola che sarà comunque disponibile in quella posizione nella maggior parte dei documenti.

È anche importante notare che non si può sempre fare questa regolazione (omettere una parola), quindi ogni motore di ricerca dovrebbe tenere un indice separato delle frasi che si trovano su Internet per verificare quale regolazione porta a buoni risultati e quale no. Tuttavia, occasionalmente un motore di ricerca farà questo aggiustamento anche se non ha la frase nel suo indice - specialmente quando un utente sta cercando una parola significativa che ci si aspetta abbia pagine significative nelle prime posizioni, il che supera questo "errore" perché gli utenti di solito le richiedono comunque.

L'ultimo passo è lavorare con la lingua inglese e "pulire" la query dai caratteri inutili. Per esempio, sulla query "lavatrice", l'utente di solito si aspetta risultati solo sulla parola "lavatrice" e possiamo ignorare il carattere virgola.

I metodi esatti di come funziona questo sistema non sono noti e ogni motore di ricerca ha i suoi. Si crede che sia per lo più solo un modello statistico che fa questi aggiustamenti basati sulla conoscenza di miliardi di testi nel database.

Anche la trascrizione inglese è una scienza in sé, quindi anche qui verrà data solo una descrizione di base. Nella maggior parte dei casi, è sufficiente aggiungere le sue forme sillabate (secondo il dizionario) ad ogni parola e cercarle insieme alla parola, ottenendo così l'effetto che il motore di ricerca può trovare documenti dove le parole non appaiono solo nella loro forma base (come inserita dall'utente), ma può anche trovare le versioni inflesse - una caratteristica molto utile. Il problema con questo approccio è nell'inflessione di parole oscure e problematiche, e anche nell'inflessione di domande che sono brevi (e quindi non è possibile determinare dal contesto come inflettere la parola). Lasciamo che la parola "giochi" sia un esempio di inflessione problematica.

Data una query inglese, "I love her", un inflessore mal progettato infletterà la parola "games" come, per esempio, "games, gaming, gaming room, ...", quindi è anche necessario indicizzare le parole circostanti come frasi ed eseguire l'inflessione solo in situazioni familiari, o utilizzare una procedura diversa (questo è dove gli algoritmi fonetici spesso entrano in gioco).

L'ultimo passo è quello di inviare l'albero finito ai server di ricerca Base, dove avrà luogo la ricerca effettiva del barile - questo è l'argomento del prossimo capitolo.
