Condizioni in PHP - IF() {...} - opzioni di ramificazione
=========================================================

> id: '3e4b81bb-a8bb-4e99-8842-e76f1658a371'
> slug:
> 	cs: if
> 	it: condizioni-in-php---if-...---opzioni-di-ramificazione
> 
> perex:
> 	- 'Podmínky, logické výrazy, booleovská logika a možnosti větvení PHP scriptů. Vyhodnocování logických výrazů a operátory. Možnosti skládání výrazu.'
> 	- 'Condizioni, logica booleana, logica booleana e opzioni di ramificazione degli script PHP. Valutazione di espressioni e operatori booleani. Opzioni di piegatura dell''espressione.'
> 
> publicationDate: '2019-11-26 11:55:09'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '759a244b4fe2dc119c475aee42259578'

> **Nota:** Questo articolo potrebbe essere un po' incasinato per alcuni principianti poiché presuppone una comprensione di base di PHP. Se sei interessato a come funzionano le condizioni, leggi <a href="/condizioni">sulle condizioni nel corso per principianti</a>.

| Supporto: | Tutte le versioni: PHP 4, PHP 5, PHP 7
|-------------------|----------|
| Breve descrizione: | Convalida di una o più dichiarazioni
| Tipo: | <a href="/statements-and-functions">Statement, construct</a> (non una funzione)

Descrizione
-----

Spesso avete bisogno di determinare se un'uguaglianza vale o se un'affermazione è vera, a questo servono le condizioni. PHP usa la seguente sintassi come molti altri linguaggi (specialmente C):

```php
if (logický výrok) {
    // costruire
}
```

Ogni espressione logica ha un valore di `TRUE` (vero) o `FALSE` (falso), non ci sono altre opzioni.

Esempio di confronto se la variabile `$x` è maggiore della variabile `$y`:

```php
$x = 10;
$y = 5;

if ($x > $y) {
    // Questa parte dello script verrà eseguita se la condizione è vera
} else {
    // Questa parte dello script verrà eseguita se la condizione non si applica
}
```

Il costrutto di condizione ha un contenuto obbligatorio tra parentesi tonde, in cui viene data l'espressione da testare, composta da operatori (panoramica sotto), espressioni multiple possono essere collegate usando operatori logici (panoramica sotto).

Inoltre, la condizione contiene due blocchi di dichiarazione opzionali.

- Se la condizione è valida
- Se la condizione non è soddisfatta

Per ragioni pratiche, includete sempre almeno il primo blocco di dichiarazioni quando la condizione è valida, altrimenti testare l'espressione non avrebbe senso.

Uso del punto e virgola e delle parentesi
--------------------------

In generale:
- Le parentesi **arrotondate** si usano per separare le espressioni logiche (altre parentesi tonde possono essere immerse per ottenere espressioni più complesse).
- La parentesi **complessa** è usata per delimitare un blocco di comandi e funzioni.
- Il **mezzo** non è usato per indicare una condizione (il blocco di comandi è delimitato da una parentesi composta), ma per separare i singoli comandi all'interno della condizione).

L'unica notazione possibile con un punto e virgola (tranne quando si usa il costrutto **endif**):

```php
if ($x > $y);
```

Tuttavia, tale condizione non ha senso perché in entrambi i casi il risultato del confronto sarà scartato e nessuna istruzione appartenente alla condizione sarà eseguita.

Notazione alternativa
--------------------------

In certe circostanze, il costrutto `if` può essere usato con l'omissione delle parentesi composte. Questo può essere ottenuto solo nei seguenti casi:

- Se eseguiamo solo una dichiarazione nella condizione.
- Se usiamo i due punti e un endif al posto delle parentesi composte;
- Se usiamo la notazione "in linea".

Per informazioni più dettagliate, vedere i seguenti 3 capitoli.

> **`1. Un solo comando ~ sintassi abbreviata`**

Se state creando una condizione in cui volete eseguire solo un costrutto (dichiarazione), potete usare sia la classica notazione di parentesi composta:

```php
if ($x > 10) { $y = $x; }
```

Oppure possiamo omettere le parentesi:

```php
if ($x > 10) $y = $x;
```

Tuttavia, questo comportamento si applica solo a un comando nelle immediate vicinanze della condizione.

Un esempio migliore (solo il costrutto `$y = $x` viene eseguito condizionatamente, il resto viene sempre eseguito):

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
if (výraz):
    konstrukt;
    konstrukt;
    konstrukt;
endif;
```

Tuttavia, questa notazione è stata a lungo considerata obsoleta perché si riduce nell'orientamento quando più condizioni sono immerse in se stesse.

> **Nota:** Vorrei notare che questo stile piace anche ad alcune persone, come Yuh (<a href="https://www.jakpsatweb.cz/php/php-tahak.html#vetveni">vedi il suo articolo</a>). Dio non voglia che tu lo usi da qualche parte.

> **`3. Espressione ternaria ~ notazione "in linea" su una sola riga`**

Occasionalmente è utile fare un semplice confronto in linea con qualche altra azione (per esempio, insieme alla definizione di una nuova variabile). Se vogliamo eseguire una sola istruzione, l'intera procedura può essere ridotta a una sola riga, pur mantenendola il più semplice possibile.

```php
$x = 5;
$xJeVetsiNezDva = ($x > 2 ? TRUE : FALSE);

// o anche più breve:

$xJeVetsiNezDva = ($x > 2);
```

Tabelle degli operatori
-----------------

All'interno della condizione vengono utilizzati due tipi di operatori:

- **Comparativo** ~ confrontano una relazione specifica,
- **Logico** ~ combina più espressioni per creare condizioni complesse.

Operatori comparativi
-----------------------

| Operatore, significato.
|----------|----------|
| ``==` | Equals
| `===` | È uguale e ha lo stesso tipo di dati
| `!=` | Non è uguale
| E' uguale o maggiore
| `<=` | È uguale o inferiore
| Maggiore
| Meno

Esempio (valido quando `$x non è 5`):

```php
if ($x != 5) { ... }
```

Operatori logici
--------------------

| Operatore | Alternativo | Significato | Vero quando:
|-----------|--------------|----------------|--------------------------------------------
| `&&` | AND | e allo stesso tempo | entrambi i valori sono veri
| `||` | OR | o | almeno un valore è vero
| `^^` | XOR | OR esclusivo | almeno uno è vero o falso, ma mai entrambi
| `!` | *doesn't* | negazione dell'espressione | `true` quando `false` e viceversa
| La negazione dell'espressione dipende dalle circostanze.

Un esempio più complesso:

```php
$x = 5;
$y = 3;
$z = 8;
if ($x > 0 && !($y != 2 && $z == $x) || $z > $y) { ... }
```

Omettere gli operatori logici e di confronto
---------------------------------------------

Spesso possiamo permetterci di omettere uno dei due operatori (o anche entrambi), ma non dobbiamo mai dimenticare le regole di uso corretto per far funzionare l'espressione risultante.

In generale, quando si verifica un'espressione senza operatore, si verifica se il suo valore è `TRUE` o non vuoto (per esempio, contiene un numero non nullo, una stringa non vuota, ...).

Esempi:

```php
$x = 5;
$y = 3;
$z = 8;

if ($x) { ... }         // passa perché $x non è vuoto
if ($x && $y) { ... }   // passa perché $x e $y non sono vuoti
if (!$x) { ... }        // fallisce perché TRUE è negato
if (isset($z)) { ... }  // passa perché la variabile $z esiste
```

Ma possono sorgere situazioni difficili, specialmente quando:
- Chiedo `se ($x)` e la variabile `$x` contiene zero (`0`), allora la condizione non è soddisfatta.
- Oppure la variabile `$x` contiene la stringa ``0`` (il numero zero), perché trabocca a zero e quindi l'espressione non è vera.
- C'è una funzione nella condizione che restituisce sempre qualche stringa non vuota, perché allora la condizione è sempre vera.
- Oppure se stiamo controllando l'input dell'utente e lui restituisce `'false'` come stringa, allora di nuovo la condizione è vera perché la stringa non è vuota.

Consiglio una soluzione semplice ed efficace per questo: chiedere il numero di caratteri che vengono restituiti. Se la stringa è vuota (o la variabile non esiste), allora vengono restituiti zero caratteri e la condizione non è soddisfatta. Esempio semplice:

```php
$x = '0';
if ($x) { ... }			// la condizione non si applica normalmente
if (strlen($x)) { ... }	// la condizione è valida perché $x contiene 1 carattere
```

Poi, possiamo verificare l'esistenza di una variabile usando la funzione `isset()`.

Confronto delle stringhe
-----------------

Scoprire che le corde sono identiche è facile:

```php
$a = 'Cat';
$b = 'cat';

if ($a === $b) {
    // Se le stringhe sono uguali
} else {
    // Se le stringhe sono diverse
}
```

È importante tenere d'occhio i tipi di dati nel caso in cui la voce possa essere equivalente a un'altra.

Per esempio, la stringa vuota `$a = '';` è diversa dalla stringa `NULL`: `$b = NULL;`. Abbiamo bisogno di fare questa distinzione, per esempio, per i database dove c'è una differenza tra un valore che non esiste o che è vuoto.

```php
$a = '';
$b = null;

if ($a == $b) {
    // Sarà valutato come TRUE perché
    // il tipo di dati viene convertito.
}

if ($a === $b) {
    // Esegue una validazione molto più rigorosa
    // e non passerà perché è un diverso
    // contenuto e un diverso tipo di dati, quindi
    // questo codice non verrà mai eseguito.
}
```

È anche una buona idea ignorare i caratteri bianchi (invisibili) come spazi, tabulazioni e interruzioni di riga quando si confrontano le stringhe. Questo è utile, per esempio, quando si inserisce una password e la si passa a una funzione di hashing:

```php
$password = '81dc9bdb52d04dc20036dbd8313ed055'; // 1234
$userPassword = '1234';

if (md5(trim($userPassword)) === $password) {
    // La funzione trim() cancella automaticamente gli spazi.
}
```

Valore sconosciuto (inesistente)?
--------------------------

A volte può succedere che il valore non esista (non è né `TRUE` né `FALSE`), è principalmente un valore ottenuto dal database (per esempio, stiamo chiedendo una colonna che non esiste), in questo caso verrà restituito il tipo di dati `NULL`.

In generale, `NULL` viene valutato come `FALSE`, cioè la condizione non si applica. Tuttavia, questo comportamento non è sempre conveniente, poiché un valore inesistente non significa necessariamente che non ci sia un record.

> Esempio dalla pratica: Abbiamo un profilo utente e interroghiamo la pagina web dell'utente. Non tutti gli utenti devono avere una pagina web, quindi in questo caso viene restituito `NULL`, ma l'utente esiste ancora. Quindi in questo caso, dovremmo piuttosto usare la funzione `isset()` per testare la (non) esistenza della variabile e non trarre una conclusione basata su un valore specifico.
