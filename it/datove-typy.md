Tipi di dati in PHP
===================

> id: cd63818c-259a-484e-b006-86c35b362019
> slug:
> 	cs: datove-typy
> 	it: tipi-di-dati-in-php
> 
> perex:
> 	- 'Datové typy v PHP: String, integer, float, boolean, pole (array), objekt a další.'
> 	- 'Tipi di dati in PHP: stringa, intero, float, booleano, array, oggetto e altro.'
> 
> publicationDate: '2019-08-23 15:44:28'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: fcb2c647973c5f94e601c789b0a882f1

Tutti i dati trattati in PHP sono di un certo tipo. Per esempio, un intero, una stringa o un booleano (vero/falso).

Tipi di dati di base
--------------------

I tipi di base sono anche chiamati **tipi di dati primitivi**, o <a href="/funzione-è-scalare">tipi scalari</a>.

| Tipo | Nome | Descrizione |
|---------|-----------------|-------|
| (`int` | Integer | (`integer`) Contiene solo interi. Il valore massimo è determinato dal numero di bit. Su 32-bit PHP la gamma è da `-2.147.483.648` a `-2.147.483.647` (~ ± 2 miliardi), 64-bit PHP ha una gamma da `-9.223.372.036.854.775.808` a `-9.223.372.036.854.775.807` (~ ± 9 quintilioni). Il valore massimo può sempre essere ottenuto chiamando la costante `PHP_INT_MAX`. Se il valore massimo di un intero viene superato, PHP rappresenterà il numero come `float` e lo sovrascriverà automaticamente.
| Questa è una variante del numero in virgola mobile per la quale si applica la regola *"più piccolo è più preciso "*. Il numero è memorizzato internamente come una cosiddetta **mantissa** e **esponente**, quindi state effettivamente memorizzando 2 numeri tra i quali viene eseguita l'operazione `mantissa * (2^esponente)`, rendendo possibile memorizzare una gamma davvero enorme di numeri. Questo utilizza il principio che per i grandi numeri non abbiamo sempre bisogno di conoscere il loro valore esatto, ma vogliamo risparmiare quanta più memoria possibile. I numeri di tipo `float` non hanno bisogno di essere memorizzati esattamente e non dovrebbero essere usati per calcolare denaro.
| `string` | String | Contiene una sequenza di caratteri delimitati da virgolette o apostrofi. La lunghezza massima è limitata solo dalla capacità della memoria operativa. Una stringa può essere memorizzata in qualsiasi codifica, contenere emoji o dati binari.
| `bool` | Boolean | (`boolean`) Valore booleano dall'algebra booleana, può contenere solo `true` o `false`.
| Il valore vuoto `null` è utile nei casi in cui vogliamo esprimere che qualcosa non esiste. Per esempio, un articolo non ha categoria. A volte `null` viene erroneamente sostituito con null (`0`) o stringhe vuote (`''`), ma questa non è una buona soluzione per esprimere la non esistenza.

**Avviso:** Il tipo `null` non è scalare.

Zero (0) contro null
----------------

Molti sviluppatori hanno difficoltà a capire la differenza tra `0` (zero) e `null` (un valore inesistente) quando iniziano lo sviluppo.

Questa distinzione può essere spiegata molto bene e con umorismo con la seguente immagine:

<img src="{$baseUrl}/images/0-vs-null.jpg" alt="0 vs. null" class="w-100 mb-3">

Riconfezionamento manuale
--------------------

Alcuni tipi possono essere convertiti tra loro. Il tipo di dati di destinazione è specificato usando parentesi tonde e può essere specificato ovunque nell'applicazione, per esempio quando si elenca un valore.

Per esempio:

```php
$pi = 3.14;

echo $pi;       // stampa 3.14

echo (int) $pi; // stampa 3
```

Riempimento dinamico
---------------------

Considerate le seguenti 2 variabili:

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

Nel secondo caso, si verifica la cosiddetta **sovrascrittura dinamica**, cioè la variabile converte il suo tipo di dati in modo che un'operazione computazionale possa essere eseguita su di essa. Questo comportamento non è sempre affidabile ed è più che altro un comportamento correttivo per correggere gli script dei principianti scritti male. Se possibile, scrivete sempre i numeri con un tipo di dati per memorizzarli, perché questo aumenta la loro precisione e li rende più facili da usare in futuro.

> **Nota:** È importante notare che non possiamo convertire i tipi di dati in modo completamente arbitrario, quindi questo non è sempre possibile. Se si sovrascrive un tipo di dati a un altro (incompatibile), o la conversione potrebbe non avvenire affatto, o il contenuto originale potrebbe essere corrotto o completamente distrutto e sostituito con un altro. Per esempio, se si riscrive una stringa in un intero (e del testo che non è un numero viene memorizzato nella variabile), il valore `1` sarà memorizzato nella variabile invece del valore numerico.

Confrontare i tipi
----------------

Quando si confrontano i valori, è necessario considerare diversi tipi di dati.

In generale, l'operatore `==` è usato per il confronto generale di due valori (indipendentemente dai tipi), mentre `===` confronta sia i valori che il tipo di dati.

Per esempio:

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
