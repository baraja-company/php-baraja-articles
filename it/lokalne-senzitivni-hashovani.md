Hashing sensibile a livello locale
==================================

> id: '76e09294-08ea-4dde-a431-2116595d9f04'
> slug:
> 	cs: lokalne-senzitivni-hashovani
> 	it: hashing-sensibile-a-livello-locale
> 
> publicationDate: '2022-07-11 10:45:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: c23def9586aa05d73c4f1cf1fd1dda43

Il principio della maggior parte delle funzioni di hashing per le impronte digitali dei documenti è che restituiscono sempre lo stesso risultato per ogni singolo input. Questo si chiama comportamento deterministico. Allo stesso tempo, una piccola variazione dell'input causerà una grande variazione dell'output (restituendo di fatto un hash completamente diverso). Questo è particolarmente utile quando si vuole verificare se il documento di input è cambiato e, in caso affermativo, si ottiene qualcosa di completamente diverso. Un esempio di tale funzione è MD5 o SHA1.

Se vogliamo dedurre il contenuto dell'input originale dall'hash, non c'è un modo semplice per farlo e non abbiamo altra scelta che forzare brutalmente ogni opzione finché non otteniamo un risultato. Questo perché, a causa dell'ampia variazione dell'output, non possiamo determinare con le iterazioni se ci stiamo avvicinando o meno all'obiettivo.

Alcune funzioni di hashing, come BCrypt, per lo stesso input restituiscono ogni volta un output diverso, in modo da renderle resistenti all'hashing delle password. Infatti, l'output contiene direttamente il sale, il che rende impossibile un cosiddetto attacco a dizionario. Questa funzione è utile solo per l'hashing delle password, ma è decisamente inadatta per la revisione dei documenti.

Controllo della somiglianza dei documenti e ricerca della duplicazione dei contenuti
-----------------------------------------------------------

Immaginiamo di risolvere l'algoritmo di un motore di ricerca web che vuole verificare quanto è cambiata una pagina web. In alternativa, si vuole verificare rapidamente se il testo di una pagina è simile a quello di un'altra pagina, o addirittura completamente duplicato.

Un modo per verificarlo è confrontare ogni pagina con l'altra, operazione che richiede molte risorse di sistema. Un'altra opzione è quella di calcolare un hash SHA1, ma questo esegue l'hash del contenuto dell'intero documento e se la pagina cambia di un solo carattere, si ottiene un hash diverso, quindi non è adatto a questi scopi.

Pertanto, una possibile soluzione è quella di calcolare l'hash da aree diverse del documento in modo diverso e poi cercare i cambiamenti nell'output della funzione di hashing.

Il principio dell'hashing locale sensibile
----------------------------------

Abbiamo scaricato il codice HTML di una pagina Web per la quale vogliamo calcolare un hash. Il documento viene suddiviso in diverse regioni (celle) da un algoritmo che deve essere configurato correttamente.

Ogni regione viene sottoposta a un hash indipendente utilizzando un algoritmo comune e concatenando il risultato in un'unica stringa.

L'output può essere, ad esempio, `3791744029724361934` (questo hash del contenuto è stato fornito dallo strumento Ahrefs).

Ad esempio, se sappiamo che il contenuto dell'intestazione rappresenta i primi 5 caratteri e il piè di pagina gli ultimi 5, sappiamo che il centro della stringa rappresenta il contenuto della pagina. Se il contenuto del piè di pagina cambia in tutte le pagine (ad esempio, il webmaster aggiunge un nuovo link, cambia la data di aggiornamento e così via), quando viene scaricata la nuova versione della pagina, solo alcuni caratteri dell'hash nell'area di destra cambiano leggermente e sappiamo che solo il piè di pagina è cambiato, ma il contenuto rimane invariato. In questo modo possiamo ignorare tale modifica e non dobbiamo esaminare l'intero sito.

In che modo Google ottimizza il crawling del web?
----------------------------------------

I bot di Internet devono risparmiare dove possono. Il crawling del Web è molto costoso e gli aggiornamenti costano tempo di calcolo.

Ad esempio, osservando il comportamento dell'algoritmo robot di Google, ho notato che risponde solo a grandi cambiamenti nei contenuti. Se la pagina cambia poco, viene strisciata nel modo normale. Ma quando, ad esempio, il piè di pagina e l'intestazione di un sito cambiano in modo significativo, viene valutata come una riprogettazione e viene esaminata la maggior parte del sito in una sola volta per ottenere il nuovo look il prima possibile.

Rileva anche i duplicati tra i siti in modo simile. Quando un gruppo di pagine è molto simile o ha lo stesso contenuto, ottiene un hash molto simile, che il robot può utilizzare per verificare rapidamente che i documenti sono simili e può selezionare quello canonico e ignorare gli altri.

Implementazione della funzione di hashing
-----------------------------

Non esiste un'implementazione già pronta della funzione di hashing locale-sensibile in PHP. Allo stesso tempo, non conosco nessun pacchetto liberamente disponibile che implementi bene questa funzione.
