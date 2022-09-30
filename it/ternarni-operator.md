Operatori ternari in PHP (?:) - condizioni in una riga
======================================================

> id: '1191be9b-ff9d-4725-b0b4-17b60de2b935'
> slug:
> 	cs: ternarni-operator
> 	it: operatori-ternari-in-php-condizioni-in-una-riga
> 
> perex:
> 	- Ternární operátor umožňuje zapsat jednoduchou podmínku do jednoho řádku jako výraz.
> 	- L'operatore ternario consente di scrivere una condizione semplice in una riga come espressione.
> 
> publicationDate: '2019-11-26 11:59:18'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: '20ff690401289869eae2a2584fae7568'

L'operatore ternario consente di abbreviare una semplice condizione in una sola riga, in un punto in cui il parsing è inutile, complesso o del tutto inappropriato.

TL;DR
------

Gli operatori ternari sono un'abbreviazione delle condizioni `if' e `else', quindi non è necessario usarli. Sono utili per ripetere sempre pezzi di logica di verifica. Utilizzare sempre il formato `(condizione? logica positiva : logica negativa)`, comprese le parentesi. Utilizzare per le verifiche brevi per rendere il codice più chiaro e più breve.

Definizioni di base
------------------

Spesso abbiamo una condizione del tipo, ad esempio:

```php
$a = 5;
$b = 3;

if ($a > $b) {
     echo 'È più grande';
} else {
     echo 'È più piccolo';
}
```

Quindi, se vogliamo scrivere una semplice frase, dobbiamo usare 4 righe di codice, che potrebbero essere ridotte a una.

```php
$a = 5;
$b = 3;

echo 'È' . ($a > $b ? 'più grande' : 'più piccolo');
```

In genere, l'operatore ternario viene scritto utilizzando 3 parti (ecco perché si chiama "ternario"):

```php
(condition ? 'sì' : 'da')
```

Gli operatori ternari sono usati molto spesso nella pratica, ad esempio per indicare le righe pari di una tabella:

```php
$pole = [3, 1, 4, 1, 5, 9, 2];

for ($i = 0; $pole[$i]; $i++) {
     echo '<tr class=""' . ($i % 2 ? 'da' : 'anche') . '">';

          // Questo è in qualche modo lavorare con un foglio di calcolo...
          // per esempio: echo '<td>' . $campo[$i] . '</td>';

     echo '</tr>.';
}
```

Esempio di utilizzo dell'operatore ternario
------------------------------------

Molto spesso abbiamo bisogno di verificare l'esistenza di un valore variabile e di utilizzarlo immediatamente, se necessario. Se non esiste, vogliamo restituire il valore predefinito.

L'approccio classico consiste nel procedere in questo modo:

```php
$a = 5;
$b = 8;

$c = $a ? $a : $b;
```

Se la variabile `$a` esiste, usa `$a` come valore, altrimenti `$b`.

Tuttavia, a volte si ottiene il valore da una funzione:

```php
$a = 5;
$b = 3;
$default = 42;

$c = my_function($a, $b) ? my_function($a, $b) : $default;
```

Questo metodo di chiamata è molto inefficiente in termini di risorse di sistema. Innanzitutto, la funzione deve essere chiamata e, se esiste, viene richiamata per ottenere il valore, che viene memorizzato nella variabile `$c`.

Questo potrebbe essere gestito meglio tramite una variabile di aiuto:

```php
$a = 5;
$b = 3;
$helper = my_function($a, $b);
$default = 42;

$c = $helper ? $helper : $default;
```

Uso inappropriato
------------------

L'operatore ternario non vale sempre la pena di essere usato, perché tende a creare confusione quando si scrivono condizioni complicate e annidate.

Guardate voi stessi un esempio:

```php
$valid = true;
$lang = 'Francese';

$x = $valid
     ? ($lang === 'Francese' ? 'oui' : 'sì')
     : ($lang === 'Francese' ? 'non' : 'da');
```

Sapreste a colpo d'occhio che la variabile `$x` conterrebbe il valore `oui`?

Dopo un po' di pratica si potrebbe, ma non è la risposta giusta. :)

Verifica della (non) esistenza del valore e utilizzo del valore predefinito
--------------------

Gli operatori ternari sono più potenti nella verifica di routine della (non) esistenza dei valori e nell'uso di altri valori predefiniti.

Ad esempio, si vuole verificare se esiste una categoria principale per un articolo e, in caso contrario, emettere un messaggio di sostituzione:

```php
echo $mainCategory ?? 'La categoria non esiste';
```

L'operatore `??` (due punti interrogativi) controlla se la variabile `$mainCategory` esiste e non è `null`. Funziona come la funzione `isset()`.

Convalidare il vuoto di un valore
-----------------------------

Un costrutto molto spesso utile per verificare che il valore di uscita non sia vuoto (cioè non `null`, `0`, `false` o `''*(stringa vuota)*).

Questo può essere scritto come segue:

```php
$a = 5;
$b = 3;
$default = 42;

$c = my_function($a, $b) ?: $default;
```

Per prima cosa viene chiamata la `funzione($a, $b)`, poi viene testato il suo valore e, se non è vuoto, viene immediatamente passato alla variabile `$c`, altrimenti viene usata la variabile `$default`.

L'operatore `?:` funziona come l'operatore `!=` in un condizionale (falso indipendentemente dal tipo di dati) e può essere ricordato dall'aspetto di una faccina sorridente con i capelli alla Elvis, per esempio.

Supporto per le versioni precedenti di PHP
----------------------------

L'operatore `?:` funziona da PHP 7. Nelle versioni precedenti, ci si deve accontentare della condizione `if (...)`, che può ottenere lo stesso comportamento. Ricordate che gli operatori ternari sono solo una scorciatoia per scrivere la stessa cosa che viene gestita dalle condizioni.

La non esistenza di un valore può essere verificata con la funzione `isset()`, che controlla che la variabile esista e non sia vuota (`null`).

Invece del codice:

```php
$a = 5;
$b = 3;

$c = $a ?? $b;
```

Poi scriviamo l'alternativa più vecchia:

```php
$a = 5;
$b = 3;

$c = isset($a) && $a ?? $b;
```

> **Attenzione:** L'ordine di `isset()` e della variabile stessa è importante. Se si inverte l'ordine e la variabile non esiste, viene generato un errore di accesso alla variabile inesistente.
