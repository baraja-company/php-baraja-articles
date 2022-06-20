Condizioni in PHP - IF() {...} - Opzioni di ramificazione
=========================================================

> id: '3e4b81bb-a8bb-4e99-8842-e76f1658a371'
> slug:
> 	cs: if
> 	it: condizioni-in-php-if-opzioni-di-ramificazione
> 
> perex:
> 	- 'Podmínky, logické výrazy, booleovská logika a možnosti větvení PHP scriptů. Vyhodnocování logických výrazů a operátory. Možnosti skládání výrazu.'
> 	- 'Condizioni, logica booleana, logica booleana e opzioni di ramificazione degli script PHP. Valutazione di espressioni e operatori booleani. Opzioni di piegatura dell''espressione.'
> 
> publicationDate: '2019-11-26 11:55:09'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '0d8a00928bcfa899d4e6dbf6c067394e'

> **Nota:** Questo articolo potrebbe essere un po' complicato per alcuni principianti, poiché presuppone una conoscenza di base di PHP. Se siete interessati al funzionamento delle condizioni, leggete <a href="/condizioni">le condizioni nel corso per principianti</a>.

| Supporto: | Tutte le versioni: PHP 4, PHP 5, PHP 7
|-------------------|----------|
| Breve descrizione: | Convalida di una o più dichiarazioni
| Tipo: | <a href="/statements-and-functions">Dichiarazione, costrutto</a> (non una funzione)

Descrizione
-----

Spesso è necessario determinare se un'uguaglianza è valida o se un'affermazione è vera: a questo servono le condizioni. PHP utilizza la seguente sintassi, come molti altri linguaggi (in particolare il C):

```php
if (/* dichiarazione logica */) {
    // costruire
}
```

Ogni espressione logica ha un valore di `TRUE' (vero) o `FALSE' (falso), non ci sono altre opzioni.

Esempio di confronto tra la variabile `$x` e la variabile `$y`:

```php
$x = 10;
$y = 5;

if ($x > $y) {
    // Questa parte dello script viene eseguita se la condizione è vera
} else {
    // Questa parte dello script verrà eseguita se la condizione non è applicabile
}
```

Il costrutto di condizione ha un contenuto obbligatorio tra parentesi tonde, in cui viene fornita l'espressione da testare, composta da operatori (panoramica di seguito); più espressioni possono essere collegate utilizzando operatori logici (panoramica di seguito).

Inoltre, la condizione contiene due blocchi di istruzioni opzionali.

- Se la condizione è valida
- Se la condizione non è soddisfatta

Per motivi pratici, bisogna sempre includere almeno il primo blocco di istruzioni quando la condizione è vera, altrimenti il test dell'espressione non avrebbe senso.

Uso del punto e virgola e delle parentesi
--------------------------

In generale:
- Le parentesi tonde** sono utilizzate per separare le espressioni logiche (altre parentesi tonde possono essere inserite per ottenere espressioni più complesse).
- La parentesi **complessiva** viene utilizzata per delimitare un blocco di comandi e funzioni.
- Il **mezzo** non viene usato per indicare una condizione (il blocco di comandi è delimitato da una parentesi composta), ma per separare i singoli comandi all'interno della condizione).

L'unica notazione possibile con un punto e virgola (tranne quando si usa il costrutto **endif**):

```php
if ($x > $y);
```

Tuttavia, una condizione di questo tipo non ha senso, perché in entrambi i casi il risultato del confronto verrà scartato e non verrà eseguita alcuna istruzione appartenente alla condizione.

Notazione alternativa
--------------------------

In alcune circostanze, il costrutto `if' può essere utilizzato con l'omissione delle parentesi composte. Questo può essere ottenuto solo nei seguenti casi:

- Se si esegue una sola istruzione nella condizione.
- Se si usano i due punti e l'endif al posto delle parentesi composte;
- Se usiamo la notazione "in linea".

Per informazioni più dettagliate, consultare i 3 capitoli seguenti.

> **`1. Un solo comando ~ sintassi abbreviata`**

Se si crea una condizione in cui si vuole eseguire un solo costrutto (istruzione), si può utilizzare la classica notazione a parentesi composta:

```php
if ($x > 10) { $y = $x; }
```

Oppure possiamo omettere le parentesi:

```php
if ($x > 10) $y = $x;
```

Tuttavia, questo comportamento si applica solo a un singolo comando nelle immediate vicinanze della condizione.

Un esempio migliore (solo il costrutto `$y = $x` viene eseguito in modo condizionale, il resto viene sempre eseguito):

```php
$x = 5;
$y = 3;
$z = 10;
if ($x > $y)
$y = $x;
$x = 3;
```

> **`2. Colon e endif;`**

```php
if (/* espressione */):
    konstrukt;
    konstrukt;
    konstrukt;
endif;
```

Tuttavia, questa notazione è stata a lungo considerata obsoleta perché riduce l'orientamento quando più condizioni sono immerse in se stesse.

**Nota: ** Vorrei far notare che questo stile piace anche ad alcune persone, come Yuh (<a href="https://www.jakpsatweb.cz/php/php-tahak.html#vetveni">vedi il suo articolo</a>). Dio non voglia che lo usiate da qualche parte.

> **`3. Espressione ternaria ~ notazione "in linea" a riga singola`**

A volte è utile eseguire un semplice confronto in linea con qualche altra azione (ad esempio, insieme alla definizione di una nuova variabile). Se si vuole eseguire una sola istruzione, l'intera procedura può essere ridotta a una sola riga, pur mantenendola il più semplice possibile.

```php
$x = 5;
$isBiggerThanTwo = ($x > 2 ? true : false);

// o anche più breve:

$isBiggerThanTwo = ($x > 2);

// o senza parentesi:

$isBiggerThanTwo = $x > 2;
```

Tabelle degli operatori
-----------------

All'interno della condizione vengono utilizzati due tipi di operatori:

- **Comparativi** ~ confrontano una relazione specifica,
- **Logica** ~ combinare più espressioni per creare condizioni complesse.

Operatori a confronto
-----------------------

| Operatore | Significato |
|----------|----------|
| `==` | Equals
| `===` | Equivale e ha lo stesso tipo di dati
| `!=` | Non è uguale
| `>=` | Eguaglia o è maggiore
| `<=` | Eguaglia o è minore
| `>` | Maggiore
| `<` | Meno

Esempio (valido quando `$x non è 5`):

```php
if ($x != 5) { ... }
```

Operatori logici
--------------------

| Operatore | Alternativa | Significato | Vero quando:
|-----------|--------------|----------------|--------------------------------------------
| `&&` | AND | e allo stesso tempo | entrambi i valori sono veri
| OR o almeno uno dei valori è vero.
| XOR | OR esclusivo | almeno uno dei due è vero o falso, ma mai tutti e due.
| `!` | *non lo fa* | negazione di un'espressione | `vero` quando `falso` e viceversa
| `()` | *non* | negazione dell'espressione| dipende dalle circostanze

Un esempio più complesso:

```php
$x = 5;
$y = 3;
$z = 8;
if ($x > 0 && !($y != 2 && $z == $x) || $z > $y) { ... }
```

Omissione degli operatori logici e di confronto
---------------------------------------------

Spesso possiamo permetterci di omettere uno dei due operatori (o addirittura entrambi), ma non dobbiamo mai dimenticare le regole di utilizzo corretto per far funzionare l'espressione risultante.

In generale, quando si testa un'espressione senza operatore, si verifica se il suo valore è `TRUE` o non vuoto (ad esempio, se contiene un numero non nullo, una stringa non vuota, ...).

Esempi:

```php
$x = 5;
$y = 3;
$z = 8;

if ($x) { ... }         // passa perché $x non è vuoto
if ($x && $y) { ... }   // passa perché $x e $y non sono vuoti
if (!$x) { ... }        // fallisce perché VERO è negato
if (isset($z)) { ... }  // passa perché la variabile $z esiste
```

Ma possono verificarsi situazioni difficili, soprattutto quando:
- Chiedo `se ($x)` e la variabile `$x` contiene zero (`0`), allora la condizione non è soddisfatta.
- Oppure la variabile `$x` contiene la stringa ``0`` (il numero zero), perché trabocca a zero e quindi l'espressione non è vera.
- C'è una funzione nella condizione che restituisce sempre una stringa non vuota, perché allora la condizione è sempre vera.
- Oppure, se stiamo controllando l'input dell'utente e questi restituisce `'false'' come stringa, anche in questo caso la condizione è vera perché la stringa è non vuota.

Vi consiglio una soluzione semplice ed efficace: chiedere il numero di caratteri che vengono restituiti. Se la stringa è vuota (o la variabile non esiste), vengono restituiti zero caratteri e la condizione non è soddisfatta. Un semplice esempio:

```php
$x = '0';
if ($x) { ... }			// la condizione non si applica normalmente
if (strlen($x)) { ... }	// la condizione è valida perché $x contiene 1 carattere
```

Successivamente, possiamo verificare l'esistenza di una variabile utilizzando la funzione `isset()`.

Confronto tra stringhe
-----------------

Scoprire che le corde sono identiche è facile:

```php
$a = 'Gatto';
$b = 'gatto';

if ($a === $b) {
    // Se le stringhe sono uguali
} else {
    // Se le stringhe sono diverse
}
```

È importante tenere sotto controllo i tipi di dati, nel caso in cui una voce possa essere equivalente a un'altra.

Ad esempio, la stringa vuota `$a = '';` è diversa dalla stringa `NULL`: `$b = NULL;`. Questa distinzione è necessaria, ad esempio, per i database, dove esiste una differenza tra un valore inesistente e uno vuoto.

```php
$a = '';
$b = null;

if ($a == $b) {
    // Sarà valutato come VERO perché
    // il tipo di dati viene convertito.
}

if ($a === $b) {
    // Esegue una validazione molto più rigorosa
    // e non passerà perché si tratta di un diverso
    // contenuto e un tipo di dati diverso, quindi
    // questo codice non verrà mai eseguito.
}
```

È anche una buona idea ignorare i caratteri bianchi (invisibili) come spazi, tabulazioni e interruzioni di riga quando si confrontano le stringhe. È utile, ad esempio, quando si inserisce una password e la si passa a una funzione di hashing:

```php
$password = '81dc9bdb52d04dc20036dbd8313ed055'; // 1234
$userPassword = '1234';

if (md5(trim($userPassword)) === $password) {
    // La funzione trim() elimina automaticamente gli spazi.
}
```

Valore sconosciuto (inesistente)?
--------------------------

A volte può accadere che il valore non esista (non è né `TRUE` né `FALSE`), ma si tratta principalmente di un valore ottenuto dal database (per esempio, si chiede una colonna che non esiste); in questo caso verrà restituito il tipo di dato `NULL`.

In generale, `NULL` viene valutato come `FALSE`, cioè la condizione non è applicabile. Tuttavia, questo comportamento non è sempre conveniente, poiché un valore inesistente non significa necessariamente che non esista un record.

> Esempio dalla pratica: Abbiamo un profilo utente e interroghiamo la pagina web dell'utente. Non tutti gli utenti devono avere una pagina web, quindi in questo caso viene restituito `NULL`, ma l'utente esiste ancora. Quindi, in questo caso, dovremmo usare la funzione `isset()` per verificare la (non) esistenza della variabile e non fare una conclusione basata su un valore specifico.
