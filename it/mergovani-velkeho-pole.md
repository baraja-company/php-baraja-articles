Unire grandi array in PHP
=========================

> id: ccfb3f47-c8a0-4d0f-aa29-18d4c1775725
> slug:
> 	cs: mergovani-velkeho-pole
> 	it: unire-grandi-array-in-php
> 
> publicationDate: '2020-02-06 09:32:08'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3bfc3b53d26a4c9b675e851a643fc71a'

Spesso abbiamo bisogno di unire più array insieme, questo può essere fatto in modo molto elegante con la funzione `array_merge`:

```php
$userIdsA = [1, 2, 3];
$userIdsB = [5, 6, 7];

// restituisce [1, 2, 3, 5, 6, 7]
$finalIds = array_merge($userIdsA, $userIdsB);
```

La funzione `array_merge` fonde due array in un unico grande array. Se c'è una collisione di tasti, vince il valore dell'array più a destra.

Fusione ripetuta in un ciclo
---------------------------

Tuttavia, spesso otteniamo un array di array che viene creato solo in un ciclo (per esempio, da un database e poi passato attraverso foreach), e quindi non sappiamo in anticipo il numero di fusioni.

Una soluzione ingenua potrebbe essere questa:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds = array_merge($finalIds, $user->someIds);
}
```

Tuttavia, questa soluzione è molto inefficiente per la CPU perché dobbiamo unire gli array insieme ad ogni iterazione e iterare su tutto il grande array.

Tuttavia, c'è una soluzione semplice in cui si modifica l'algoritmo di fusione per passare attraverso i dati solo una volta:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds[] = $user->someIds;
}
```

In questo caso, il campo `$finalIds` genererà un po' più di dati, ma questo è ancora un problema minore del vantaggio di risparmiare tempo.

La fusione stessa varia a seconda della versione di PHP che stai usando, ed è risolta con un elegante trucco:

```php
/* PHP 5.6 e precedenti */
$finalIds = call_user_func_array('fusione di array', $finalIds + [[]]);

/* PHP 5.6+ e successivi */
$finalIds = array_merge([], ...$finalIds);

/* PHP 7.4+ e successivi per campi non vuoti */
$finalIds = array_merge(...$finalIds);
```

In particolare, la soluzione `array_merge(...$finalIds)` sembra molto interessante, perché usa un nuovo concetto di PHP 7 in cui si può passare un numero dinamico di argomenti a una funzione usando il carattere tripletta all'inizio. Il processo di fusione è quindi il più efficiente possibile e l'intera logica è gestita automaticamente da PHP internamente.

La notazione abbreviata `array_merge(...$finalIds)` può essere usata solo per array non vuoti. Se è un array vuoto, nessun argomento viene passato alla funzione e la funzione lancia un errore `Function array_merge invoked with 0 parameters, at least 1 required.`.
