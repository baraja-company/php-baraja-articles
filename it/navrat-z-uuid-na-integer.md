Ritorno da UUID a intero
========================

> id: d842517b-c4f6-4cef-a839-d08ff3804fca
> slug:
> 	cs: navrat-z-uuid-na-integer
> 	it: ritorno-da-uuid-a-intero
> 
> publicationDate: '2021-05-02 17:00:00'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: b7c136f3e3ca8a4563230c557ab1fa9c

Nello sviluppo del software, un programmatore si troverà molto spesso in un vicolo cieco di fronte a una decisione architettonica che avrà un enorme impatto sul futuro del suo lavoro per decenni a venire. Allo stesso tempo, è una decisione che non può essere revocata, e c'è un caro prezzo da pagare per ogni errore. I database sono un tipico esempio di decisione architettonica dove le teste rotolano ad ogni piccolo errore.

Una delle grandi decisioni degli ultimi tempi è stata come memorizzare le chiavi primarie nelle tabelle del database. Anche se questo sembra un problema banale, dietro c'è molto di più di quanto si possa pensare.

Opzioni della chiave primaria
-------------------------

Ci sono fondamentalmente quattro opzioni di base:

- intero
- intero senza segno
- grande int
- <a href="/uuid-performance">UUUID</a>

Integer è semplicemente un numero intero (in caso di `unsigned` allora unsigned, quindi sempre positivo, e in caso di `big int` allora può raggiungere valori estremamente grandi). Un concetto molto semplice. Un UUID è quindi una stringa di testo (per esempio, della forma `c4a760a8-dbcf-5254-a0d9-6a4474bd1b62`) che consiste di diverse parti, ognuna delle quali può avere determinate proprietà e sono utili per costruire enormi applicazioni multiserver o decentralizzate. c'è un grande ecosistema di tecnologie utili intorno agli UUID che risolvono problemi che potresti anche non sapere di avere, o che avrai in futuro.

Usare il martello giusto
-------------------------

Non molto tempo fa (inverno 2020) il mio amico Paul stava spiegando il concetto di applicare la soluzione appropriata a un problema di una data dimensione. Questa è una grande e importante idea che molti sviluppatori amano dimenticare - crea soluzioni enormemente complesse quando non è necessario. In inglese, abbiamo una bella frase per questo **sovra-ingegneria**.

Dimensione e unicità degli UUID
--------------------------

Il vantaggio fondamentale dell'UUID è che se l'applicazione diventa troppo grande e dividiamo il database su molti server web, dove una tabella di database è così grande che non si adatta al disco di una sola macchina, possiamo dividerla su molte macchine fisiche, ognuna delle quali conoscerà la sua parte della tabella e interrogherà i suoi colleghi per il resto. UUID risolve anche il problema fondamentale dell'inserimento di nuove righe, quando nel caso di applicazioni estremamente grandi abbiamo bisogno di scrivere righe in parallelo in molti posti e non vogliamo aspettare che la capacità di scrittura del server principale si liberi.

Il concetto di scrivere in molti posti allo stesso tempo è usato, per esempio, dalle applicazioni di chat. Quando si invia un messaggio tramite Messenger, questo va al server di database di Facebook più vicino, che assegna un UUID e un timestamp al messaggio e lo scrive nel suo database locale. Il tuo amico dall'altra parte del mondo, a sua volta, scrive i messaggi al suo data center locale, e nel frattempo, l'intera infrastruttura cloud assicura la sincronizzazione in tutto il mondo. Sembra figo, vero? :)

Affinché tale scrittura parallela funzioni, il problema della collisione dei record deve essere risolto. Se i singoli database locali usano un semplice numero intero, molto presto due server indipendenti scriveranno due record diversi sotto lo stesso identificatore. Quando questi record sono sincronizzati, si verifica una collisione. Di solito non c'è soluzione - non puoi rinumerare gli ID, perché altre sessioni possono portare a questo.

UUID risolve questo, per esempio, dando ad ogni server un prefisso concordato che inserisce all'inizio di ogni UUID, poi inserisce un timestamp, poi l'identificatore stesso.

> **Fatto interessante:** Quando si scrive una tale quantità di dati, non siamo tanto interessati a quando è stato scritto il record, ma in quale ordine è stato scritto (in modo da non cambiare l'ordine dei messaggi per l'utente, per esempio).

Potreste chiedere come gestire una situazione in cui i server non possono essere d'accordo tra loro su quale prefisso usare. Questo problema si presenta, per esempio, nelle applicazioni decentralizzate o offline. In questo caso, l'UUID può anche essere generato in modo casuale.

La domanda allora è quali sono le possibilità che si verifichi un conflitto quando <a href="https://stackoverflow.com/questions/1155008/how-unique-is-uuid">generano casualmente un UUID</a>. Beh, probabilmente non succederà a te. Ci sono circa `2^122` UUID unici (perché è un numero a `128 bit`). In pratica, la probabilità di un conflitto è circa `0,00000000006 (6 × 10-11)`. In pratica, ciò significa che se generiamo **1 miliardo di UUIDs** ogni secondo per i prossimi 100 anni, la possibilità di conflitto sarà del `50%`. Quindi è più probabile che il conflitto non si verifichi, e UUID è la soluzione definitiva ai vostri problemi di database.

C'è bisogno di una soluzione così robusta?
-------------------------------

Se non lo sai, la risposta è **no**.

Con la chiave primaria impostata su `int` con il flag `unsigned`, ci sono `4.294.967.295` valori possibili (4 miliardi). Per un confronto delle dimensioni degli interi, vedere la <a href="https://dev.mysql.com/doc/refman/8.0/en/integer-types.html">documentazione di MySql</a>.

Nel momento in cui si memorizzano 4 miliardi di record in una tabella, probabilmente si esaurirà prima lo spazio su disco.

Prestazioni di interi e join
----------------------

Gli interi sono davvero molto veloci. Ci sono ottimizzazioni native per loro in MySql. Gli indici funzionano correttamente (in più sono molto più piccoli), occupano solo 4 byte, sono molto veloci da unire e andranno bene per la maggior parte dei casi.

Se avete a che fare con un problema di replica del database, la soluzione migliore potrebbe essere quella di mettere l'intero database nel cloud come MS Azure e interrogarlo esternamente. Anche quando si memorizzano decine di milioni di record, la velocità di accesso via integer a una riga specifica è nell'ordine dei millisecondi (meno di 3 ms su un server ben configurato), e con un indice clustered, il tempo può essere ben sostenibile anche con un gran numero di richieste.

Se avete davvero bisogno di usare gli UUID, fareste meglio a lasciare il mondo MySql e prendere la strada del database Postgres, che a differenza di MySql ha il proprio tipo di dati per <a href="https://www.postgresql.org/docs/9.1/datatype-uuid.html">UUUUIDs</a>. Lavorare con i join è un problema enorme con gli UUID e MySql, e quando si uniscono solo 3 tabelle (con ognuna che ha solo poche decine di migliaia di record), l'intera query può richiedere da diverse centinaia di millisecondi a pochi secondi per essere processata. E questo purtroppo è un problema di MySql che probabilmente non puoi risolvere.
