Test di conoscenza di base di Nette
===================================

> id: d137c177-503c-4e84-8709-0e65b0ce6060
> slug:
> 	cs: test-nette-zaklady
> 	it: test-di-conoscenza-di-base-di-nette
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '66fb122b33aa8ce41158345bb01769ab'

Soglia di successo: 15 punti

*Si ottiene 1 punto per ogni domanda a cui si risponde correttamente. Per ogni domanda con risposta errata non si ottiene nulla. Se la risposta è solo parziale (e non sarebbe possibile programmare la cosa sulla base di essa), la domanda conta come errata (non è possibile ottenere mezzo punto). Se la soluzione contiene un bug di sicurezza, o un refuso nel codice, o un errore di battitura nel codice, la risposta è considerata errata perché non verrebbe eseguita.*

-----------

1 Spiega la differenza tra i cicli `for`, `while`, e `foreach`. Per ognuno, fai 1 esempio specifico del suo utilizzo che mostri chiaramente il suo vantaggio principale.


2. Abbiamo una variabile di cui non sappiamo quasi nulla (sappiamo solo il suo nome). Come possiamo vedere il suo contenuto? Per esempio, si chiama `$data`.


3. Scrivi i seguenti comandi per lavorare con il repository Git:
- Scaricare le ultime modifiche dal server
- taggare il file `Statistic.php`.
- etichetta tutti i file nel progetto
- etichetta tutti i file nella directory `cron`.
- commettere modifiche con il messaggio "Il mio commit"
- inviare il commit al server


4. Mettiamo una stringa di testo nella variabile. Dare un esempio di una funzione per calcolare il checksum.


5. Scrivere uno snippet di codice che crei un'azione `delete` in `Presenter` che accetti l'ID dell'elemento come un intero e cancelli una riga dalla tabella `question` secondo l'ID specificato. Dopo una cancellazione riuscita, stamperà il messaggio "Question deleted" e reindirizzerà all'azione `list`.

Sotto domanda per un punto extra: Se la cancellazione fallisce per qualche motivo, non lancia un errore fatale, ma informa l'utente con un messaggio (flash message).

6. Quando creo un modulo Nette, diventa un componente. Cos'è un componente Nette?

7. Ho bisogno di creare un semplice modulo Nette per inserire un record in una tabella `question` che contiene una lista di domande. La struttura della tabella è:

| Colonna | Proprietà |
|-----------|----------------------------------|
| id | int(8), senza segno, incremento automatico |
| domanda | varchar(255) |
| is_active | tinyint(1), senza segno, valore predefinito: 1 |

Crea i campi appropriati del modulo per inserire una nuova riga in questa tabella. Dopo aver inserito il record, un FlashMessage deve essere sparato informando del successo dell'inserimento del record + il reindirizzamento alla modifica del record (azione `edit`).

- Convalida che il campo del modulo è stato riempito
- Convalida che il testo della domanda si adatta al varchar secondo la struttura della tabella
- Convalida che una domanda con questo testo non esiste più
- Definisci un'altra tabella personalizzata `group` per contenere informazioni sui gruppi. Quando si crea una domanda, sarà quindi possibile determinare a quale gruppo appartiene la domanda. Dovrete impostare una sessione tra i tavoli (descrivete come si fa e come sarà impostata).
- Quale macro Latte devo usare per rendere il modulo nel template (rendering di default)?

8. Abbiamo un modulo di modifica in `Presenter` che viene creato come componente. Vogliamo passare i valori predefiniti da quello che c'è nel database, cioè abbiamo bisogno di ottenere i dati dalla tabella in qualche modo conveniente.
- Come procederete e di quali elementi del codice abbiamo bisogno?
- Come passerete l'identificatore dell'elemento attualmente modificato al modulo?
- Come si imposta l'elemento del modulo al suo valore predefinito?
- Come possiamo verificare che un utente sta cercando di modificare un elemento che non esiste nel database, e come possiamo informarlo in modo appropriato?

9 Considerate i seguenti dati recuperati da un database (usando un normale Nette Database):

```php
$questions = $this->db->questions()->fetchAll();
```

- Come possiamo elencare il testo di tutte le domande come un elenco puntato?
- Come passiamo i dati dalla tabella al modello Latte?
- Di quali macro di Latte avremo bisogno per elencare gli articoli? Dare un'implementazione specifica per elencare le colonne `id` e `name` nel formato:

	*1024: Come stai?
	*1025: Cosa hai mangiato a pranzo oggi?

10. Elenca un esempio di almeno 3 diversi campi del modulo che sono scritti nel modulo:

```php
$form->add(tady bude příklad);
```

e per ognuno spiegate a cosa serve e quale output restituisce (tipo di dati + esempio).


11. Abbiamo un modulo Nette presentato.
- Come possiamo inviare tutti i campi (nomi e valori)?
- Dare un esempio di output di un campo chiamato "domanda".
- Fornire un'implementazione concreta di codice che attraverserà l'array di valori e chiavi e restituirà una singola stringa contenente l'elenco totale di chiavi e valori, in modo che possiamo, per esempio, memorizzare questa stringa in un database o inviarla via e-mail (memorizzare e inviare non è l'oggetto dell'assegnazione e non sarà valutato).


12. Per ogni condizione, decidi se il risultato è VERO o FALSO:
- `1 > 0`
- `1 == 1`
- `1 == "1"`
- `1 === "1"`
- `1 == vero`.
- 1 = vero
- 1 = falso
- `1 == "1" && 1=== true`


13. Con quali tipi di dati abbiamo familiarità in PHP?
- Dare almeno 5 esempi di nomi di tipi di dati
- Per ogni esempio, elenca almeno 3 possibili valori che possono essere memorizzati nel tipo di dati (se il tipo di dati non può memorizzare così tanti possibili valori, scrivilo)
- Per ogni tipo di dati, date un esempio tipico del suo uso (cosa vi si memorizza in pratica)
- Qual è la differenza di condizione tra `==` (due uguali) e `===` (tre uguali)?
- Spiegare lo svantaggio di usare `==` nelle condizioni e come specificamente `==` risolve questo problema (esempio in cui `==` può fallire e `==` salva la situazione)


14. Abbiamo una tabella di coordinazione (coordinations table) che elenca tutte le coordinazioni tra 2 persone. Uno di loro organizza il coordinamento e l'altro è un ospite. Scrivi una selezione di database che restituisca tutte le righe con coordinamenti che mi coinvolgono (sono l'organizzatore del coordinamento, o sono l'ospite del coordinamento). La tabella ha le colonne `id`, `id_user_organizer` (id organizzatore), `id_user_quest` (id ospite). Il mio ID è memorizzato nel solito modo in `Presenter`.


15. Gruppo di domande su Latte:
- Cos'è il Latte?
- Qual è la differenza tra `variabile`, `macro`, `filtro` e `n:attributo`? Cosa viene usato dove?
- Come faccio a creare un riferimento a un'azione `default` di `DashboardPresenter`?
- Elencando una tabella di domande, come posso generare un collegamento a una modifica specifica (azione `QuestionPresenter`, `edit`) di una domanda per passare l'ID della domanda attualmente elencata? Scrivere il codice Latte specifico.

Scritto simbolicamente (esempio in PHP, tradurre in Latte):

```php
foreach ($questions as $question) {
   echo $question->id; // ID domanda
   echo $question->question; // testo della domanda
}
```

- Come scrivere uno spazio HTML fisso?


16. A cosa servono i servizi in Nette?
- Come si istanzia?
- Cosa deve fare un servizio per essere utilizzato?
- Come si carica un servizio in Presenter?
- Per esempio, considerate il servizio `StatisticManager`, che ha un metodo pubblico `getStatistics()` che non accetta parametri. Come posso caricare questo servizio in Presenter e chiamare il metodo pubblico `getStatistics()` nell'azione predefinita e passare il risultato al modello?
- Qual è la differenza tra `oggetto`, `classe`, `servizio`?
- Cosa sono il `modello`, l' `entità` e l' `oggetto valore`?
- Tutti i servizi hanno un gestore principale che li conosce e possono essere presi in quel gestore. Come si chiama questo manager? Come faccio a registrarvi un nuovo servizio?


17. Neon
- Cosa sono i file al neon?
- Quali tipi ci sono e per cosa sono ordinati?
- Cosa contengono i file Neon? Quali dati sono memorizzati in essi?
- Dai un esempio di come registrare il seguente campo (la ricetta è in PHP, dovrà essere tradotta) in modo che possa essere usato come parametro:

```php
$imageGenerator = [
   "punti" => [
      480: [910, 30, 1845, 1150],
      600: [875, 95, 1710, 910],
      768: [975, 130, 1743, 660]
   ]
];
```

- Fate un esempio di registrazione di un servizio in Neon, al quale passiamo il parametro `imageGenerator` che abbiamo registrato nel compito precedente, in modo che il servizio lo riceva nel costruttore e possa utilizzarlo nel servizio (nel senso di configurazione). Per il servizio, fornire un'implementazione di esempio del costruttore in modo che il primo parametro di input sia trattato come il tipo di dati per l'array.


18. Oggetti in generale
- Cosa sono i "metodi", le "proprietà" e le "costanti"? Qual è la differenza tra loro?
- Sia i metodi che le proprietà hanno 3 stati di accessibilità di base (`pubblica`, `privata`, `protetta`), spiega la differenza e un esempio specifico di utilizzo e chi può vedere cosa e quando.
- Ho una classe `course` in cui c'è una proprietà privata `currentCourse` in cui viene memorizzato il corso corrente. Come rendere la proprietà di sola lettura e non scrivere dall'esterno?


19. Quando creo tabelle in un database che sono logicamente correlate (per esempio, una tabella per un utente e poi una tabella per i suoi articoli), ho bisogno di trattare che i dati siano collegati correttamente.
- Cosa assicura che i dati nelle tabelle siano collegati correttamente nel database?
- Cos'è l'integrità referenziale e qual è il suo ruolo nel database?
- Che tipo di sessioni abbiamo? Qual è lo scopo di ogni tipo?
- Quali parametri impostiamo per le sessioni per gestire la cancellazione o la modifica dei dati in modi diversi? Fai 3 esempi e usi specifici + una descrizione di come funziona.


20. Qual è lo scopo delle fabbriche (`OOP design pattern`)?
- Dare un esempio di utilizzo.
- I servizi Nette sono fabbriche?
- Qual è lo scopo della dependency injection?
- Qual è la differenza tra `DI` e `DIC`?
