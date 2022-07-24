Immutabilità degli oggetti: un concetto di progettazione fondamentale
=====================================================================

> id: '057467db-4e3b-4e18-9ea5-dfb25feb3800'
> slug:
> 	cs: immutabilita
> 	it: immutabilita-degli-oggetti-un-concetto-di-progettazione-fondamentale
> 
> publicationDate: '2022-07-24 15:00:00'
> mainCategoryId: ae4c1c70-11b3-433e-b1d0-e590155bb8b9
> sourceContentHash: '88c26f35883c860426b4708f0be8761f'

L'immutabilità è uno dei concetti di progettazione più importanti per costruire applicazioni stabili. Il principio di base prevede che una volta scritto uno stato, questo possa essere letto solo in seguito senza la possibilità di modificarlo. Se vogliamo cambiare lo stato, dobbiamo creare una nuova istanza e sostituire l'intero oggetto con un altro.

I tipi di dati possono quindi essere suddivisi in modo molto approssimativo in due grandi categorie:

- **Mutabile** (stato mutabile all'interno di una singola istanza)
- **Immutabile** (stato interno immutabile)

Gli oggetti mutabili possono essere modificati internamente. Cioè, forniscono operazioni che, se richiamate in combinazioni diverse, fanno sì che si ottengano risultati diversi. L'immutabilità cerca di prevenire questo comportamento.

Definizione
--------

> Una classe è **immutabile** proprio se i dati dell'istanza non possono essere modificati in alcun modo dopo la creazione dell'istanza.

Quindi tutti i dati sono fissati nel costruttore. Tutti i tipi di dati scalari sono automaticamente immutabili.

Un vantaggio importante
--------------

La progettazione di applicazioni con stati immutabili offre un vantaggio fondamentale nella sicurezza dell'esecuzione delle operazioni. Se sappiamo che una volta scritti, i dati non possono essere modificati (mutati) in seguito, possiamo, ad esempio, eseguire il debug in modo molto affidabile o suddividere l'applicazione in sottofunzioni senza il rischio di dimenticare nessuno degli stati intermedi.

L'idea di immutabilità si oppone generalmente al principio della memorizzazione degli stati nelle proprietà degli oggetti/entità e descrive piuttosto un approccio funzionale in cui i dati "scorrono" attraverso l'applicazione, come fa ad esempio javascript.

Dal punto di vista delle prestazioni, possiamo automaticamente affermare che gli oggetti immutabili possono essere messi in cache all'infinito, perché non saranno mai obsoleti.

Un esempio reale da PHP
--------------------

L'uso di gran lunga più comune degli oggetti immutabili in PHP è l'oggetto `DateTimeImmutable`, che una volta creato può essere richiamato solo dai metodi di formattazione. Se si modificano le impostazioni interne, il metodo restituirà una nuova istanza. Questa caratteristica è fondamentale quando si usa un ORM che utilizza il cosiddetto identity pattern: ci permette, ad esempio, di garantire che quando leggiamo la data di creazione di un ordine, questa sarà la stessa ovunque nell'applicazione e l'integrità referenziale non sarà danneggiata.

Un esempio concreto di oggetto mutabile:

```php
$date = new DateTime('2021-05-14');
$tomorrow = $date->modify('+1 giorno');
echo $date->format('Y-m-d'); // 2021-05-15
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

La stessa data è stata stampata perché il metodo `modify()` ha modificato solo lo stato interno dell'oggetto `DateTime` e ha restituito la stessa istanza. Pertanto, non c'era la cosiddetta **mutazione dello stato interno**, che è un comportamento di base della programmazione orientata agli oggetti. L'aggiornamento della variabile ha modificato anche quella originale.

E ora un esempio di oggetto immutabile:

```php
$date = new DateTimeImmutable('2021-05-14');
$tomorrow = $date->modify('+1 giorno');
echo $date->format('Y-m-d'); // 2021-05-14
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

L'oggetto `DateTimeImmutable` è immutabile, il che significa che il suo stato interno non cambia mai. Una nuova istanza modificata (anch'essa immutabile) viene memorizzata nella variabile dopo che il metodo `modify()` è stato chiamato. Se non si memorizzasse il nuovo valore nella variabile, non sarebbe utilizzabile in seguito.

Il valore originale non viene mai toccato e viene conservato in modo sicuro.

Quando una classe dovrebbe essere immutabile?
---------------------------

A meno che non ci sia un'ottima ragione per renderla mutabile, scrivere sempre una classe o una funzione come immutabile. Questo semplificherà la progettazione in futuro.

Le classi mutabili dovrebbero cambiare il meno possibile. Raccomando sempre di documentare il comportamento dell'immutabilità.

Forse l'unico inconveniente dell'immutabilità è che deve essere creata una nuova istanza a ogni modifica dell'attributo, con un impatto minore sulle prestazioni. Se la vostra applicazione (come la maggior parte delle applicazioni) tende a visualizzare dati e a modificarli meno frequentemente, questo svantaggio è piuttosto insignificante con le prestazioni dei computer di oggi.

Quali tipi di dati dovrebbero essere immutabili?
------------------------------------

- Tutti gli identificatori e i codici univoci
- La maggior parte delle sessioni di database ManyToOne e OneToOne
- Date, orari, valori del calendario
- Ciclo di array in cui si vuole eseguire la stessa operazione su ogni elemento
- Intervalli, coppie, triple, ...
- Figure geometriche, punti, linee, coordinate GPS, indirizzi fisici, ...
- Diari e registri storici
- Informazioni sugli ordini evasi e sulla maggior parte dei dati finanziari
- Meta dati sull'entità correlata

Ciò che non dovrebbe essere immutabile:

- Oggetti di grandi dimensioni con molte proprietà
- La maggior parte dell'output tabellare di un database, come le entità di Doctrine
- Costruire progressivamente oggetti a partire da parti più piccole
