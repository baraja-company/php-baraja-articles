Variabili locali in PHP
=======================

> id: '9c3fbf4e-8612-4599-8127-d66b9ec5e980'
> slug:
> 	cs: lokalni-promenna
> 	it: variabili-locali-in-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: cffee22c4dbcc2ec331b73235349c409

Le variabili locali sono valide solo all'interno del corpo di **<a href="/prikazy-a-function">funzione</a>** o **metodo** (nella programmazione orientata agli oggetti).

Se stiamo lavorando nel contesto di un normale script, tutto avviene come previsto:

```php
$x = 5;

echo $x; // stampa 5
```

Ma quando definiamo una funzione personalizzata, il comportamento cambia leggermente:

```php
$x = 5;

function mojeFunkce(): int
{
    $x = 3;

    echo $x; // stampa 3
}

echo $x;     // stampa 5
```

La ragione è che la variabile $x esiste solo nel contesto della funzione o del metodo corrente. Questo comportamento è intenzionale.

Se vogliamo passare un valore dal codice circostante a una funzione, dobbiamo chiamarla con i parametri necessari:

```php
echo mojeFunkce(5);	// stampa 6

function mojeFunkce(int $x): int
{
    return $x + 1;
}
```

I valori vengono inseriti nelle funzioni tramite parametri. Se volete passare ulteriori variabili nella funzione oltre ai parametri, potete usare <a href="/global-variable">variabili globali</a>, ma questa non è una buona idea.

> L'uso di variabili locali fa un'enorme differenza quando si programma un'applicazione più grande. Se non distinguessimo la validità delle variabili attraverso i contesti, sarebbe impossibile garantire che una variabile non venga sovrascritta in un posto dove non ci contiamo (ecco perché le variabili globali sono il male).

```php
$x = 5;
$y = 3;

function soucet(int $x, int $y): int
{
    return $x + $y;
}

echo $x;             // stampa 5
echo soucet($x, $y); // stampa 8
```
