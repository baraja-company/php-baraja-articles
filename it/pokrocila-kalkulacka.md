Calcolatrice in PHP: elaborare un'espressione matematica come una stringa
=========================================================================

> id: e6798758-031d-4e7c-b24e-1f77cf61558d
> slug:
> 	cs: pokrocila-kalkulacka
> 	it: calcolatrice-in-php-elaborare-un-espressione-matematica-come-una-stringa
> 
> publicationDate: '2020-02-16 17:07:38'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '63f5336bf2bbabe5312121b57e1ce34e'

Immaginate di trovarvi di fronte al compito di elaborare un semplice esempio matematico che un utente inserisce come stringa di testo in una casella di ricerca. Tipicamente, l'utente vuole eseguire una semplice operazione numerica con i numeri. Questo articolo descrive il processo di pensiero e le istruzioni specifiche su come farlo.

Implementazione ingenua
-------------------

Per molto tempo mi sono chiesto se una semplice espressione matematica potesse essere elaborata con qualche trucco per rendere il codice il più corto possibile... e dopo molti anni ho effettivamente la soluzione.

Considerate la soluzione data **solo come esempio**, perché è **estremamente pericolosa** e un utente disonesto può facilmente sottolineare la stringa, che per esempio cancellerà l'intera applicazione o ruberà il database!

```php
// Domanda dell'utente
$query = '5 + 3 * 2';

// Elaborare l'espressione come codice regolare PHP
eval('$risultato = @(' . $query . ');');

// Elencare la variabile con la soluzione dell'espressione
echo $result; // stampa 11
```

Il trucco è che la funzione <a href="/function-eval">eval()</a> esegue la stringa come se fosse nel contesto del codice PHP. È pazzesco, ma funziona. Il wrapper sopprime i messaggi di errore.

Trattare con input più complessi
--------------------------

Oltre al fatto che **elaborare espressioni tramite eval() è estremamente pericoloso**, non fornisce anche una sintassi abbastanza eloquente da soddisfare tutti. Se l'utente fa anche una sola violazione della sintassi, l'intera espressione non potrà essere elaborata.

Pertanto, la soluzione è di **comprendere** e correggere la query dell'utente secondo l'aspetto formale prima (chiamata normalizzazione alla forma canonica), e poi passarla ed elaborarla ulteriormente.

Ho programmato [QueryNormalizer](https://github.com/mathematicator-core/engine/blob/master/src/QueryNormalizer.php) esattamente per questo compito in passato.

L'elaborazione stessa è un compito molto impegnativo, perché è necessario comprendere correttamente i diversi contesti. Per esempio, che le parentesi denotano blocchi annidati e devono essere valutati ricorsivamente. Per esempio, l'espressione `5+2^(1+3/2)` non può essere risolta direttamente perché la frazione deve essere risolta prima, aggiunta al numero tra parentesi, poi risolta per la potenza intera, e infine aggiunta a livello di base.

Per essere in grado di soddisfare questo requisito esigente, l'espressione non può più essere trattata come una stringa ordinaria e dobbiamo **prendere il livello di astrazione**. In sostanza, la matematica è un tipo di linguaggio che descrive le relazioni tra operazioni e numeri, perché abbiamo a che fare con le priorità degli operatori, i diversi significati, i contesti, la ricorsione e anche i tipi di dati. È qui che entra in gioco il processo di **query tokenization**.

> Ho lavorato sul problema della tokenizzazione matematica dal 2015 e ho scritto diversi parser da allora.

Il migliore di questi, che attualmente alimenta il nuovo Mathematicator, è [disponibile opensource su GitHub](https://github.com/mathematicator-core/tokenizer).

Lo scopo della tokenizzazione è di **parare una stringa**, dividerla in gruppi di stringhe più piccole di tipi noti, e poi convertirle in oggetti (tipi di dati). L'array di oggetti convertito è poi convertito da una **logica intelligente in un albero binario** che può descrivere le dipendenze e la ricorsione. Questo è un processo molto impegnativo perché ci sono centinaia di scenari possibili e gli utenti possono essere molto creativi nella loro interrogazione.

Il vantaggio principale dell'array di token è che può essere passato molto facilmente al livello successivo, che per esempio [esegue il calcolo effettivo](https://github.com/mathematicator-core/calculator), o [ridisegna l'albero in LaTeX](https://github.com/mathematicator-core/tokenizer/blob/master/src/TokensToLatex.php).

L'uso può essere elegante come questo:

```php
$tokenizer = new Tokenizer(/* alcune dipendenze */);

// Convertire la formula matematica in un array di token:
$tokens = $tokenizer->tokenize('(5+3)*(2/(7+3))');

// Ora puoi convertire i token in un formato più utile:
$objectTokens = $tokenizer->tokensToObject($tokens);

dump($objectTokens); // Restituisce token digitati con metadati

// Rendering in LaTeX
echo $tokenizer->tokensToLatex($objectTokens);

// Render ad albero di debug (estremamente veloce):
echo $tokenizer->renderTokensTree($objectTokens);
```

Visualizza le procedure
-----------------

Un numero significativo di utenti apprezzerà quando **quando calcola, il programma visualizza la procedura** per mostrare come ha fatto. Questo è effettivamente utile anche per il programmatore, perché almeno può facilmente scoprire dove c'è un errore nel calcolo e correggere l'algoritmo di conseguenza. Quando si combina tutto questo con l'apprendimento automatico basato su test automatizzati, si ottiene qualcosa di incredibile.

Guarda come `QueryNormalizer` è stato in grado di capire la tua query, passare i dati al tokenizer, ha reso la query in LaTeX in base ad essa e poi ha passato l'albero degli oggetti al calcolatore che ha restituito il risultato complessivo.

Příklad: [5+2^(1+3/2)](https://mathematicator.com/search/5%2B2%5E%281%2B3/2%29).

La rappresentazione della procedura è implementata dalla calcolatrice che attraversa l'albero di input e valuta una regola alla volta secondo i token e le regole che contiene. Quando una regola viene valutata, mette le informazioni sul passo in un array. Occasionalmente, un passo può rivelarsi errato e dobbiamo tornare indietro e prendere un percorso diverso nel calcolo, ma c'è un bel po' di magia dietro questo, che rimarrà nascosta per ora e potrete studiarla nell'implementazione.

Conclusione
-----

La procedura di cui sopra descrive come gestire elegantemente le espressioni matematiche in cui abbiamo numeri, operazioni e relazioni con essi. Questo approccio non può, per esempio, modificare espressioni o risolvere equazioni, ma lo vedremo la prossima volta.

*Se hai altre idee su come elaborare la matematica in modo efficiente, sarei felice di sentirle.
