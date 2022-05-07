Algoritmo dei motori di ricerca su Internet - Indicizzazione e canonicalizzazione
=================================================================================

> id: daa97e66-e77f-4741-ade2-905c1cfdbd56
> slug:
> 	cs: vyhledavac-indexace-a-kanonizace
> 	it: algoritmo-dei-motori-di-ricerca-su-internet---indicizzazione-e-canonicalizzazione
> 
> publicationDate: '2016-09-11 11:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d73a30f413e4b8b77dbceda60b3cf173

Nella lezione di oggi, vedremo come indicizzare e canonizzare i documenti su Internet.

Indicizzazione
--------

Il processo di indicizzazione viene eseguito da un componente chiamato indexer. Questo è un programma appositamente progettato che rende i dati scaricati (i dati che il Crawler ha scaricato) in un tipo di dati speciale per la ricerca - i barili.

Il problema dell'indicizzazione è che non si può sfogliare "intelligentemente" i documenti, ma la lettura sequenziale (leggere l'intero testo dall'inizio alla fine) è inevitabile, quindi è una disciplina impegnativa e i motori di ricerca utilizzano i server più potenti per questa attività. Nessun altro compito nel processo di ricerca è così impegnativo come l'indicizzazione, dove il testo semplice diventa indice.

Prendete, per esempio, una pagina sui gatti, scaricata da Wikipedia. L'indicizzatore ottiene il testo completo della pagina e deve rimuovere le cose non necessarie (ad esempio i menu di controllo dell'utente, le pubblicità, i piè di pagina, ...) e analizzare la pagina per ottenere il testo semplice. Per esempio, il testo potrebbe essere:

> Il gatto domestico (Felis silvestris f. catus) è una forma addomesticata del gatto selvatico che è stato un compagno dell'uomo per migliaia di anni. Come il suo parente selvatico, appartiene alla sottofamiglia dei piccoli gatti, ed è un tipico rappresentante del gruppo. Ha un corpo agile e muscoloso, perfettamente adattato alla caccia, artigli e denti affilati, vista, udito e olfatto eccellenti.

Estratto da [Wikipedia](http://cs.wikipedia.org/wiki/Ko%C4%8Dka_dom%C3%A1c%C3%AD).

Il motore di ricerca ora analizza le singole parole dal testo e le scrive in singoli barili. Tuttavia, non può fare la scrittura del barile direttamente (principalmente perché ogni barile esiste in molte copie e anche perché interferirebbe con la ricerca), quindi crea una lista di requisiti per come dovrebbe essere il futuro barile e li memorizza nella memoria temporanea. Nel nostro caso, una tale lista potrebbe apparire come questa (alcune parole non importanti possono essere ignorate o segnate come stopwords):

| ID del documento | Parola | Posizione | Tipo |
|--------------|-------|--------|-----------|
| 1 | Cat | 1 | Normale |
| 1 | Domestico| 2 | Normale |
| 1 | Felis | 3 | Normale |
| 1 | Is | 7 | StopWord |
| 1 addomesticato 8 normale

Una tale lista è enorme (molto più grande dei testi originali), ma è ancora più veloce da cercare perché non dobbiamo leggere tutti i testi in sequenza, ma possiamo cercare per indice in ogni colonna e le parole di una pagina sono ordinate in righe una dopo l'altra.

Dopo che è passato del tempo (o un certo numero di documenti è stato completato), l'indicizzatore smette di lavorare alla costruzione di questa lista di richieste (per un barile futuro) e ricomincia a leggere i dati e a ricostruire i singoli barili (questa lista include vecchi record che sono in un barile già funzionante). Se vengono aggiunti nuovi record da indirizzi noti, questo processo li aggiornerà, mentre per i nuovi documenti li includerà.

In questo modo, l'indicizzatore passerà di nuovo attraverso la lista e lentamente creerà nuovi barili che contengono tutti gli elementi (avranno lo stesso aspetto mostrato nell'esempio nei capitoli precedenti) e quando tutti i barili saranno completati, saranno inviati ai singoli server di ricerca. L'aggiornamento dei barili sui server richiede tempo (principalmente a causa dell'enorme quantità di dati che vengono spostati), quindi durante questa operazione i server non sono disponibili e i dati vengono aggiornati su diversi server in momenti diversi. Questo risulta, per esempio, che alcuni utenti possono ottenere risultati diversi perché ogni utente cerca su un server di erogazione diverso (a causa della decomposizione del carico). Una volta completato l'aggiornamento, tutto torna alla normalità e tutti gli utenti trovano tutti i documenti allo stesso modo.

Il processo di indicizzazione è importante per ogni motore di ricerca, e quello che lo fa più spesso e più attentamente è quello con la visione più aggiornata di Internet. Google esegue questa operazione ogni poche ore, Seznam una volta alla settimana (e ha un milione di volte meno dati).

Canonicalizzazione dei documenti
--------------------

Nel progetto originale del motore di ricerca full-text, non c'era bisogno di nulla come la canonicalizzazione perché Internet era un mezzo che creava costantemente nuovi contenuti. Nel corso del tempo, tuttavia, la duplicazione (cioè, lo stesso contenuto che appare in più URL diversi) si è verificata, e i motori di ricerca devono adattarsi a questo. Un esempio tipico è Wikipedia, che ha molti articoli. Alcuni autori di altre pagine riprendono questi testi (in parte o anche completamente) e creano così dei doppioni. Nella maggior parte dei casi, tuttavia, questo non ha importanza, perché la pagina di origine ha un rango (qualità del link) molto più alto di quella plagiata, ma a volte può succedere che degrada l'originale a spese del pirata.

I motori di ricerca hanno dovuto adattarsi a questo vizio ed è stato coniato il termine "canonizzazione", che può essere inteso come la "rimozione" di certe pagine dall'indice. Tra l'altro, questo rende gli indici più piccoli, e il motore di ricerca non deve scansionare lo stesso contenuto tutto il tempo inutilmente.

I duplicati sono internamente divisi in 2 grandi categorie da ogni motore di ricerca:

Duplicati naturali
-------------------

Questi sono creati dal comportamento naturale di Internet e dalle sue caratteristiche.

Per esempio, l'URL `http://mathematicator.cz` è probabile che abbia lo stesso contenuto dell'URL `http://www.mathematicator.cz` o `http://mathematicator.cz/index.php` perché questo è il comportamento standard del server Apache (e di Internet in generale).

Se viene trovato un duplicato naturale, il motore di ricerca crea un "set canonico", che è un gruppo di pagine da cui il motore di ricerca seleziona un rappresentante che spicca nella ricerca. Se un link conduce a una qualsiasi pagina dell'insieme canonico, il suo rango sarà automaticamente passato al rappresentante principale.

È spesso una buona idea aiutare il motore di ricerca a creare questo set e impostare correttamente il reindirizzamento sul sito, il che farà sì che il motore di ricerca guardi meglio il sito e che il rappresentante principale sia selezionato meglio.

Duplicati che portano al plagio
----------------------------

L'esistenza del plagio di per sé non ha molta importanza, ma gli utenti ne sono particolarmente infastiditi perché continuano a trovare gli stessi risultati per la stessa domanda. Vi è mai capitato di trovare diverse pagine con un testo identico per una query? Questo è esattamente il comportamento che i motori di ricerca cercano di prevenire.

Il problema più grande è determinare quale pagina è la fonte originale - e farlo a macchina. Di nuovo, i motori di ricerca mettono tutte le pagine simili in un insieme canonico e selezionano il rappresentante principale da questo insieme. Se le fonti provengono da domini diversi, la situazione non può essere guardata come una duplicazione naturale (e qualsiasi candidato scelto), ma tutte le pagine devono essere valutate qualitativamente e la migliore selezionata oggettivamente - e idealmente la fonte originale del contenuto.

I motori di ricerca spesso decidono in base al rango dell'intero dominio e alla forza della rete di link a un dato documento, ma anche questo è un approccio piuttosto inaffidabile. Il secondo fattore è di solito anche il tempo di creazione (indicizzazione) del documento. Ogni motore di ricerca tiene quindi traccia di quali pagine generano frequentemente nuovi contenuti e visita frequentemente queste pagine, in modo da notare idealmente la nuova pagina immediatamente e quindi non scegliere un'altra pagina come proxy.

Una descrizione dettagliata dei metodi di funzionamento di questa selezione va oltre lo scopo di questo articolo e potrebbe essere il soggetto di un intero libro.
