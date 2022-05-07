Paginatore e paginazione dei risultati in PHP
=============================================

> id: a1450160-e320-414a-8266-128465632e94
> slug:
> 	cs: paginator
> 	it: paginatore-e-paginazione-dei-risultati-in-php
> 
> perex:
> 	- Stránkování dlouhého seznamu položek. Jak řešit omezení počtu vypsaných položek a vypočítat stránkování.
> 	- Paginazione di una lunga lista di elementi. Come risolvere la limitazione del numero di elementi elencati e calcolare la paginazione.
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: d0bc71ded032401875396b0fd263a820

Quando abbiamo molti dati da scaricare, è educato dividerli in più pagine. Questo articolo non si occupa dell'implementazione pratica del passaggio dei numeri di pagina e dell'elencazione dei risultati, ma solo dell'estrazione teorica dei valori e del calcolo del codebook ottimale per rendere la consultazione di un gran numero di pagine il più semplice possibile.

Quanti risultati abbiamo
----------------------

Per cominciare, dobbiamo scoprire quanti risultati abbiamo. Se i dati provengono da un database, possono essere contati in modo molto efficiente con la seguente istruzione SQL:

```sql
SELECT COUNT(*) FROM tabulka
```

Il calcolo è molto veloce perché il database mantiene le statistiche in un file di aiuto, quindi non tocca affatto i dati.

Se i dati vengono da qualche altra parte (e li abbiamo in un array, per esempio), possono essere contati con la funzione count():

```php
$cisla = [3, 1, 4, 1, 5, 9, 2];

echo 'Il campo contiene' . count($cisla) . 'numeri.';
```

Limitare il numero di risultati
----------------------

Un altro problema è la limitazione del numero di risultati. Se i dati sono nel database, basta mettere il parametro `LIMIT` nella dichiarazione SQL:

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10
```

Questo comando otterrà sempre un massimo di 10 risultati, e renderà anche l'interrogazione più veloce perché il database non dovrà passare attraverso tutti i file di dati.

Se abbiamo dati da un'altra fonte (di nuovo un array), possiamo anche limitare i risultati a livello di PHP usando la variabile helper `$iterator`:

```php
$pole = [...];

$iterator = 0;
$limit = 10;
foreach ($pole as $prvek) {
	// qui è dove i dati vengono scaricati

	$iterator++;
	if ($iterator >= $limit) {
	    break; // Arresta il ciclo quando ha eseguito 10 volte
	}
}
```

Saltare i primi X risultati
----------------------

Quando siamo nella prima pagina, è abbastanza semplice, basta limitare il numero di risultati usando `LIMIT`. Ma cosa succede se sono alla terza pagina? Allora dobbiamo saltare i primi risultati `X`.

In SQL abbiamo un'elegante notazione per questo di nuovo:

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10 OFFSET 20
```

Salta i primi 20 risultati e limita l'output successivo a 10 risultati, quindi emette l'intervallo `<21 - 30>`.

In PHP puro questo è gestito in due modi.

Se conosciamo gli indici dell'array, possiamo iniziare a leggerlo da un certo punto (il che è molto veloce):

```php
$pole = [...];

$start = 20;
$limit = 10;
for ($i = $start; ($i <= $start + $limit && isset($pole[$i])); $i++) {
	// qui è dove i dati vengono scaricati
}
```

Tuttavia, per un campo sconosciuto dobbiamo usare di nuovo l'iteratore e saltare gli elementi:

```php
$pole = [...];

$iterator = 0;
$start = 20;
$limit = 10;
foreach ($pole as $prvek) {
	if ($iterator < $start) {
		$iterator++;
		continue;
	}

	// in qualche modo i dati vengono scaricati qui

	$iterator++;
	if ($iterator >= $start + $limit) break;
}
```

Visualizzazione del paginatore/stepper ottimale
----------------------

Supponiamo di conoscere il numero totale di elementi, il numero di elementi nella pagina e il numero di pagina corrente. Ora vogliamo rendere una barra che permetta la navigazione veloce di tutte le pagine con risultati di ricerca. Tuttavia, poiché ci sono molte pagine (dell'ordine di migliaia), non possiamo elencarle tutte in una volta, quindi dobbiamo scegliere intelligentemente alcune rappresentative che rappresentano al meglio la gamma tra le pagine.

Può apparire così:

```php
1 | 15 | 30 | 36 | 45 | 60 | 72
```

Assegnazione:

Sono a pagina 36 di 72, come posizionare in modo ottimale i numeri di pagina?
Bene, attraverso la sequenza.

> **Suggerimento:** Per osservazione pratica ho scoperto che la parte sinistra del Paginator dovrebbe essere calcolata attraverso una sequenza aritmetica (così posso muovermi linearmente dello stesso numero di passi) e la parte destra attraverso una **sequenza geometrica**, che a sua volta rende facile fare un grande passo. Così, se voglio arrivare a una pagina particolare, salto prima un gran numero di elementi non necessari e poi affino la selezione tornando a sinistra.

Teoria della sequenza aritmetica (continuiamo ad aggiungere lo stesso numero):

```php
$d = 10;   // dimensione del passo
$a[1] = 1; // primo elemento
$a[2] = $a[1] + $d; // secondo elemento
$a[3] = $a[1] + 2 * $d;
$a[3] = $a[2] + $d;
$a[$n] = $a[1] + ($n - 1) * $d; // nono elemento

function getAritmeticItem(int $start, int $step, int $n): int
{
	return $start + ($n - 1) * $step;
}
```

Teoria della sequenza geometrica (moltiplicare sempre per lo stesso numero):

```php
$q = 10;   // dimensione del passo
$a[1] = 1; // primo elemento
$a[2] = $a[1] * $q; // secondo elemento
$a[3] = $a[1] * $q * $q;
$a[3] = $a[1] * pow($q, 2);
$a[3] = $a[2] * $q;
$a[$n] = $a[1] * pow($q, $n - 1); // nono elemento

function getGeometricItem(int $start, int $step, int $q): int
{
	return $start * pow($q, $step - 1);
}
```



```php
$start = 1;
$current = 36;
$end = 72;
```
