Operatori ternari in PHP (?:) - condizione in una linea
=======================================================

> id: '1191be9b-ff9d-4725-b0b4-17b60de2b935'
> slug:
> 	cs: ternarni-operator
> 	it: operatori-ternari-in-php---condizione-in-una-linea
> 
> perex:
> 	- Ternární operátor umožňuje zapsat jednoduchou podmínku do jednoho řádku jako výraz.
> 	- L'operatore ternario permette di scrivere una semplice condizione in una riga come espressione.
> 
> publicationDate: '2019-11-26 11:59:18'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: '491dbbbaeceba356a030e2501f5c0e2b'

L'operatore ternario vi permette di abbreviare una condizione semplice in una singola linea in un posto dove l'analisi è inutile, complessa o del tutto inappropriata.

TL;DR
------

Gli operatori ternari sono un'abbreviazione per le condizioni `if` e `else`, quindi non è necessario usarli. Sono utili per pezzi di logica di verifica sempre ripetitivi. Usate sempre il formato `(condizione ? logica positiva : logica negativa)` includendo le parentesi. Usare per la verifica breve per rendere il codice più chiaro e breve.

Definizioni di base
------------------

Spesso abbiamo una condizione della forma, per esempio:

```php
$a = 5;
$b = 3;

if ($a > $b) {
     echo 'È più grande';
} else {
     echo 'È più piccolo';
}
```

Quindi, se vogliamo scrivere solo una semplice frase, dobbiamo usare 4 linee di codice, che potrebbero essere ridotte a una.

```php
$a = 5;
$b = 3;

echo 'È' . ($a > $b ? 'più grande' : 'più piccolo');
```

Generalmente, l'operatore ternario è scritto usando 3 parti (ecco perché si chiama "ternario"):

```php
(podmínka ? pokud je pravda : pokud není pravda)
```

Gli operatori ternari sono usati molto spesso nella pratica, per esempio per indicare le righe pari in una tabella:

```php
$pole = [3, 1, 4, 1, 5, 9, 2];

for ($i = 0; $pole[$i]; $i++) {
     echo '<tr class=""' . ($i % 2 ? 'da' : 'anche') . '">';

          // Questo è in qualche modo lavorare con un foglio di calcolo...
          // per esempio: echo '<td>' . $field[$i] . '</td>';

     echo '</tr>';
}
```

Esempio di utilizzo dell'operatore ternario
------------------------------------

Molto spesso abbiamo bisogno di controllare l'esistenza di un valore variabile e usarlo immediatamente se necessario. Se non esiste, vogliamo restituire il valore predefinito.

L'approccio classico è quello di fare così:

```php
$a = 5;
$b = 8;

$c = $a ? $a : $b;
```

Se la variabile `$a` esiste, usa `$a` come valore, altrimenti `$b`.

Tuttavia, a volte otteniamo il valore da una funzione:

```php
$a = 5;
$b = 3;
$default = 42;

$c = funkce($a, $b) ? funkce($a, $b) : $default;
```

Questo metodo di chiamata è molto inefficiente in termini di risorse di sistema. Prima, la funzione deve essere chiamata, e se esiste, viene chiamata di nuovo per ottenere il valore, che viene memorizzato nella variabile `$c`.

Questo potrebbe essere gestito meglio tramite una variabile di aiuto:

```php
$a = 5;
$b = 3;
$pomocnaPromenna = funkce($a, $b);
$default = 42;

$c = $pomocnaPromenna ? $pomocnaPromenna : $default;
```

Uso inappropriato
------------------

L'operatore ternario non vale sempre la pena di essere usato perché tende a causare confusione quando si scrivono condizioni complicate e annidate.

Guardate voi stessi un esempio:

```php
$valid = true;
$lang = 'Francese';

$x = $valid
     ? ($lang === 'Francese' ? 'oui' : 'sì')
     : ($lang === 'Francese' ? 'non' : 'da');
```

Sapreste a colpo d'occhio che la variabile `$x` contiene il valore `oui`?

Dopo un po' di pratica potresti, ma non è la risposta giusta. :)

Verificare la (non) esistenza del valore e usare il valore predefinito
--------------------

Gli operatori ternari sono più potenti nella verifica di routine della (non) esistenza di valori e nell'uso di altri valori predefiniti.

Per esempio, vogliamo controllare se esiste una categoria principale per un articolo e, in caso contrario, emettere un messaggio di sostituzione:

```php
echo $mainCategory ?? 'La categoria non esiste';
```

L'operatore `??` (due punti interrogativi) controlla se la variabile `$mainCategory` esiste e non è `null`. Funziona allo stesso modo della funzione `isset()`.

Convalidare il vuoto di un valore
-----------------------------

Un costrutto molto spesso utile per verificare che il valore di uscita non sia vuoto (cioè non `null`, `0`, `false` o `''*(stringa vuota)*).

Questo può essere scritto come segue:

```php
$a = 5;
$b = 3;
$default = 42;

$c = funkce($a, $b) ?: $default;
```

Prima viene chiamata la `funzione($a, $b)`, poi il suo valore viene testato e se non è vuoto, viene immediatamente passato alla variabile `$c`, altrimenti viene usata la variabile `$default`.

L'operatore `?:` funziona come l'operatore `!=` in un condizionale (falso indipendentemente dal tipo di dati), e può essere ricordato dall'aspetto di una faccia sorridente con i capelli alla Elvis, per esempio.

Supporto per le vecchie versioni di PHP
----------------------------

L'operatore `?:` funziona da PHP 7. Nelle versioni precedenti, dobbiamo accontentarci della condizione `if (...)`, che può ottenere lo stesso comportamento. Ricordate che gli operatori ternari sono in realtà solo una scorciatoia per scrivere la stessa cosa che viene gestita dalle condizioni.

La non esistenza di un valore può essere controllata con la funzione `isset()`, che controlla che la variabile esista e non sia vuota (`null`).

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

> **Attenzione:** L'ordine di `isset()` e la variabile stessa contano. Se invertissimo l'ordine e la variabile non esistesse, solleverebbe un errore di accesso alla variabile inesistente.
