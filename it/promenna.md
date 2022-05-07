Variabili in PHP
================

> id: b89774e7-143c-4a8c-8dc6-3b3d2c78d5b7
> slug:
> 	cs: promenna
> 	it: variabili-in-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '23e4d537f18a3b2e1946c79be1aacf52'

Questa pagina è un riassunto completo di come funzionano le variabili in PHP. Il testo è scritto in uno stile leggermente tecnico e potrebbe non essere pienamente compreso dai principianti. Se sei interessato alle basi complete, leggi <a href="/first-script">il tutorial del principiante</a> e <a href="/principles-of-prominent-script">principi di scrittura delle variabili</a>.

Descrizione
-----

Una variabile è una posizione virtuale nella memoria operativa, è definita da `nome` e `tipo di dati`. All'interno del tipo di dati, la variabile ha poi qualche `contenuto`.

Internamente, PHP rappresenta le variabili come una cosiddetta tabella hash, cioè tutte le variabili sono memorizzate in una tabella che è molto veloce da cercare, quindi il tempo richiesto per accedere ad ogni variabile è *quasi* costante.

Esempi di scrittura:

```php
$a = 10;
$b = 'cat';
$c = true;
```

Ogni riga del campione denota la definizione di una variabile. Il nome di ogni variabile inizia con il simbolo del dollaro `$` seguito dal nome stesso. Il segno di uguale può essere usato per assegnare un valore alla variabile.

Internamente, le variabili saranno immagazzinate in memoria come una tabella hash:

| Nome | Tipo | Abbreviazione | Valore |
|-------|---------|---------|---------|
| `$a` | integer | int | `10` |
| `$b` | string | string | `cats` |
| `$c` | boolean | bool | `true` |

Tipi di variabili
---------------

Le variabili sono classificate secondo i diritti di accesso e l'uso:

- <a href="/local-variable">variabili locali</a> (valide nel contesto, cioè all'interno di funzioni e metodi),
- <a href="/global-variable">variabili globali</a> (valide in tutto lo script),
- <a href="/superglobal-variable">Variabili superglobali</a> (valide in tutto l'ambiente server - tipicamente dati utente),
- <a href="/promenna-variabile">variabili</a> (una variabile speciale che contiene un riferimento (link) a un'altra variabile).

* Le variabili globali e le variabili variabili variabili non dovrebbero essere usate perché contribuiscono all'illeggibilità del codice e al comportamento "magico" (inaspettato) dell'applicazione.

Contenuti consentiti delle variabili
--------------------------

Le variabili possono contenere tutto ciò che il loro attuale tipo di dati permette. Se il tipo di dati non è specificato, sarà determinato automaticamente, in base al contenuto (questo non è raccomandato, perché può portare a errori).

I tipi di dati funzionano in modo indipendente, quindi possiamo usare quasi tutti i tipi. Tuttavia, se vogliamo eseguire qualche operazione di fusione, dobbiamo sempre garantire la conversione in un formato.

Un buon esempio è per esempio l'addizione e la moltiplicazione dei numeri:

```php
$x = 5;       // intero
$y = 3;       // intero
$z = $y + $y; // la variabile $z sarà composta in base a più variabili
```

In questo caso, PHP si trova di fronte alla questione di quale tipo di dati avrà la variabile appena creata `$z`. Se sono dello stesso tipo di dati e l'operazione è possibile, il tipo di dati viene ereditato.

Tuttavia, a volte possiamo eseguire un'operazione con più tipi di dati:

```php
$x = 1;       // intero
$y = 3.14159; // float
$z = $y + $y; // float
```

In questo caso uniamo interi e float. L'output sarà un numero decimale, quindi viene usato float. In questo caso, PHP farà qualcosa chiamato **ripartizionamento dinamico**.

Tuttavia, non possiamo sempre fare affidamento su questo comportamento. Per esempio, come volete unire un numero e una stringa?

```php
$x = 256;     // intero
$y = 'Ehi! Ehi!'; // float
$z = $y + $y; // ???
```

Tipi di dati (panoramica dei più importanti)
--------------------------------------

PHP è un linguaggio interpretato, quindi ha alcune peculiarità rispetto ad altri linguaggi di programmazione, una di queste è che non dobbiamo specificare i tipi di dati per le variabili, cioè ogni variabile cambia dinamicamente il suo tipo di dati in base al suo contenuto (salvo diversa indicazione). Tuttavia, è bene conoscere almeno i tipi di dati di base, specialmente utili quando si ottimizzano le applicazioni o si lavora con i database.

La notazione può apparire così:

```php
$x = (int) 25; // crea una variabile di tipo intero
```

<a href="/datove-typy">Panoramica dei tipi di dati</a>.

Ereditarietà del tipo di dati
-----------------------

Che tipo di dati avrà la variabile `$x` se conosciamo solo questo pezzo di codice sorgente?

```php
$x = $y;
```

Dipende dal tipo di dati della variabile `$y` da cui sarà ereditato sia il valore che il suo tipo di dati. In questo caso, non conosciamo la variabile `$x`, quindi non possiamo continuare a valutare il codice e verrebbe lanciato un messaggio di errore.

Sovrascrittura dinamica
---------------------

Abbiamo le seguenti 2 variabili:

```php
$x = 10;
$y = '10';
```

Qual è la differenza tra il contenuto delle variabili `$x` e `$y`?

La variabile `$x` è un numero, `$y` è una stringa (contenente un "1" e uno "0"), quindi se ci limitiamo a memorizzare la variabile in memoria e non eseguiamo alcuna operazione che influisca sul valore. Per esempio, le seguenti 2 voci restituiranno lo stesso risultato:

```php
echo $x + 5;	// stampa 15
echo $y + 5;	// stampa 15
```

Nel secondo caso, si verifica la cosiddetta **sovrascrittura dinamica**, cioè la variabile converte il suo tipo di dati in modo che un'operazione computazionale possa essere eseguita su di essa. Non ci si può sempre fidare di questo comportamento, ed è più un comportamento correttivo per correggere gli script dei principianti scritti male. Se possibile, scrivete sempre i numeri con un tipo di dati per memorizzarli, perché questo aumenta la loro precisione e li rende più facili da usare in futuro.

> **Nota:** È importante notare che non possiamo convertire i tipi di dati in modo completamente arbitrario, quindi questo non è sempre possibile. Se si sovrascrive un tipo di dati a un altro (incompatibile), o la conversione potrebbe non avvenire affatto, o il contenuto originale potrebbe essere corrotto o completamente distrutto e sostituito con un altro. Per esempio, se si riscrive una stringa in un intero (e del testo che non è un numero viene memorizzato nella variabile), il valore `1` sarà memorizzato nella variabile invece del valore numerico.

Rappresentare stringhe come array
------------------------------

Tutte le stringhe sono memorizzate internamente come un array di caratteri. Cioè, ogni carattere ha il suo **indice** e può essere referenziato. Se non si specifica un indice, si lavora con tutta la stringa.

```php
$x = 'Programmiamo in PHP!';
$n = 3;

echo $x;		// questo stampa l'intero contenuto della variabile $x
echo $x[0];		// questo stampa il carattere nullo della variabile $x
echo $x[$n];	// questo stampa l'ennesimo carattere della variabile $x
```

> **Nota:** Numeri PHP da zero, cioè il carattere zero è 'P' e il primo carattere è 'r'.
>
> > Inoltre, i caratteri sono scambiati per byte. Per esempio, il carattere "no" nella codifica UTF-8 è lungo 2 byte, quindi l'indice del carattere nella stringa non corrisponderà alla posizione reale durante lo scorrimento e verranno utilizzati 2 indici per memorizzare il carattere.

L'esistenza di un elemento dell'array dovrebbe sempre essere verificata con la funzione `isset()`:

```php
if (isset($x[$n])) {
    echo $x[$n];
}
```

In alternativa, potete scriverlo bene con un operatore ternario:

```php
echo $x[$n] ?? '';
```

Copiare le variabili
---------------------

Abbiamo la seguente variabile:

```php
$q = 'Lorem ipsum, ...';
```

E poi copiare il suo valore nella variabile successiva:

```php
$qi = $q;
```

Fortunatamente, non verrà fatta alcuna copia e PHP memorizzerà semplicemente un riferimento al valore in una **tabella hashash**. Il valore sarà effettivamente copiato solo quando il valore di una delle variabili dovrebbe cambiare. Questo comportamento è gestito da un componente generalmente chiamato **Garbridge collector**.
