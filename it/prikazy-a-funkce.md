Comandi, parole chiave e funzioni in PHP
========================================

> id: '96a00928-4410-479d-ada0-612de21cdacd'
> slug:
> 	cs: prikazy-a-funkce
> 	it: comandi-parole-chiave-e-funzioni-in-php
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '4534cc03d2ae66454b9f398139f232a4'

Ogni script PHP è composto da comandi e funzioni, insieme si chiamano **costruzioni**. Quando lo script viene elaborato, queste espressioni linguistiche vengono tracciate (questo processo è chiamato **tokenizzazione**) e in base alle *parole chiave* l'interprete decide come deve comportarsi il processore.

Comandi
--------------------------

I comandi (<a href="https://www.php.net/manual/en/reserved.keywords.php">sono chiamati **keywords** in inglese</a>) sono parti già pre-programmate del linguaggio, sono riservati specificamente al loro scopo, possono essere usati immediatamente in qualsiasi situazione (non richiedono librerie speciali aggiuntive o installazione), e costituiscono la base di tutti gli script.

Per esempio, <a href="/echo">estrarre una stringa nel codice HTML</a>:

```php
echo 'Echo è un comando linguistico per elencare il contenuto';
```

Con i comandi è importante notare che non restituiscono alcun valore e quindi non possono essere usati come variabili. I comandi possono anche essere identificati dal fatto che non contengono parentesi.

Esempi:

```php
echo 'Ciao, mondo!';
echo ('Ciao, mondo!');
```

Se racchiudo il contenuto del comando tra parentesi, il comportamento non cambierà e sarà visto come una **espressione**. È lo stesso che se scrivessimo in matematica:

```php
5 + 3

// o:

(5 + 3)
```

Tecnicamente c'è una differenza, ma in pratica non cambia nulla.

Funzioni
--------------------------

Se ci fossero solo comandi, sarebbe piuttosto noioso. Le funzioni sono un insieme di comandi multipli.

Spesso vogliamo eseguire lo stesso codice in più punti ripetutamente. In questo caso, la copia costante sarebbe troppo lavoro e potrebbe portare ad errori, quindi avvolgiamo tale codice in una funzione che nominiamo e poi la chiamiamo semplicemente per nome.

Quando chiamiamo la funzione, le passiamo i parametri come variabili, essa esegue il codice e restituisce il risultato, che possiamo utilizzare ulteriormente.

```php
$text = 'PHP è il mio linguaggio preferito!';

echo 'Testo "' . $text . '" è un lungo' . strlen($text) . 'personaggi.';
```

PHP ha molte funzioni direttamente predefinite (<a href="/documentazione">vedi la documentazione per una lista completa</a>), ma spesso è conveniente definire le proprie:

```php
function mojeFunkce(int $x, int $y): int
{
   $z = $x + $y;   // aggiunge i parametri di input e li memorizza in una variabile

   return $z;      // restituisce la variabile $z
}

echo mojeFunkce(5, 3); // stampa il numero 8, perché i numeri sono stati elaborati dalla funzione
```

Variabili nelle funzioni
--------------------------

Le variabili locali sono usate all'interno delle funzioni, il che significa che possono essere usate solo all'interno della funzione e non possono essere manipolate (o definite) al di fuori della funzione. Ottengono i loro valori iniziali dai parametri della funzione direttamente nella definizione della funzione.

Esempio:

```php
$z = 5;

function prvniFunkce(int $x, int $y): int
{
   return $x - $y; // questo restituirebbe la differenza di numeri
}

function druhaFunkce(): mixed
{
   return $z; // questo restituisce un errore perché la variabile $z
              // non definito all'interno della funzione
}
```

A volte è utile impostare alcuni parametri come opzionali, questo viene fatto definendo un valore alternativo (predefinito):

```php
echo prictiCislo(5);    // restituisce 6
echo prictiCislo(5, 7); // restituisce 12

function prictiCislo(int $x, int $y = 1): int
{
   return $x + $y;
}
```

La funzione `prictiCislo()` aggiunge il valore della variabile `$y` alla variabile `$x`. Se la variabile `$y` non è definita (specificata come parametro quando la funzione viene chiamata), verrà utilizzato il suo valore predefinito scritto nella definizione della funzione. Il secondo parametro della funzione (la variabile `$y`) è quindi opzionale.

> Ogni funzione può avere un solo output (un ritorno), se si specificano più output in una funzione, verrà restituito quello specificato per primo. Se volete restituire più valori, dovete usare un array.
>
> In PHP (a differenza di altri linguaggi) non dobbiamo specificare affatto un ritorno, nel qual caso la funzione non restituisce nulla (void) su qualsiasi input. Tali funzioni sono per lo più utilizzate per memorizzare i dati o l'output al codice sorgente. In generale, comunque, si raccomanda di restituire sempre qualche valore, almeno un riconoscimento che tutto è andato bene.
