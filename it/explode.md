Funzione PHP Explode - divide la stringa per separatore
=======================================================

> id: '4dc7cec6-0e96-4a6b-aee8-32c8817ba11e'
> slug:
> 	cs: explode
> 	it: funzione-php-explode---divide-la-stringa-per-separatore
> 
> perex:
> 	- Rozdělení řetězce na více částí podle oddělovače.
> 	- Dividere la catena in più parti secondo il separatore.
> 
> publicationDate: '2019-11-26 11:39:36'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '66725772be4aae0df8af399e7ce2ba07'

Explode è usato per dividere facilmente una stringa con un separatore.

- Restituisce i singoli risultati come un array numerato da zero,
- non puoi inserire un array, solo la stringa viene inserita,
- non può cambiare il delimitatore durante il parsing, non può selezionare delimitatori multipli.

| Supporto | PHP 4 e successivi |
|---------------|-----------------|
| Breve descrizione | Dividere una stringa in una matrice per separatore.
| Requisiti.
| Nota | Non può inserire un array, solo una stringa.

Spesso abbiamo bisogno di dividere una stringa secondo qualche semplice regola. Per esempio, un elenco di numeri separati da virgole.

La funzione explode() è ottima per questo, prendendo il separatore (che separa la stringa) come primo parametro e i dati stessi come secondo parametro:

```php
$cisla = '3, 1, 4, 1, 5, 9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
	echo $cislo . '<br>';
	// Qui possiamo elaborare ulteriormente i numeri
}
```

Ma cosa succede se i numeri sono separati da virgole, ma ci sono spazi intorno?

La soluzione è quella di analizzare per la più piccola sottostringa comune e poi rimuovere i caratteri indesiderati intorno ad essa (spazi e altri spazi bianchi):

```php
$cisla = '3, 1,4, 1 , 5 ,9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
     echo trim($cislo) . '<br>';
     // Qui possiamo elaborare ulteriormente i numeri
}
```

In questo caso, la funzione `trim()` rimuove elegantemente gli spazi bianchi intorno ai caratteri (spazi, tabulazioni, interruzioni di riga, ...), dando solo dati puliti.

Limite, che limita il numero di entità analizzate in un array
--------------------------

> CONSIGLIO: Per molti esempi, explode() non è adatto ed è molto meglio usare le espressioni regolari.

Tuttavia, spesso vogliamo analizzare i dati solo fino a una certa distanza, e il terzo parametro (opzionale) limit può essere usato per questo scopo.

Per esempio, abbiamo dati strutturati separati da due punti dove vogliamo ottenere il contenuto dopo i primi due punti e ignorare gli altri due punti.
Esempio:

```php
$cas = 'formato: "j.n.Y - H:i"';
```

Se dovessimo analizzare la stringa solo come:

```php
$parser = explode(':', $cas);
```

Otterremmo queste 3 sottostringhe (in altri casi potrebbero essercene molte di più):

```php
'formato'
'"j.n.Y - H'
'i"'
```

Pertanto, impostiamo un limite a quanti elementi ottenere (ed eventualmente tutti gli altri elementi saranno aggiunti alla fine dell'ultimo elemento):

```php
$parser = explode(':', $cas, 2);

// torna indietro:
echo $parser[0]; // formato
echo $parser[1]; // "j.n.Y - H:i"
```

> **Nota:** Gli apici indesiderati possono essere rimossi dalla stringa, per esempio, usando la funzione `trim()`:

```php
echo trim($parser[1], '"'); // il secondo parametro specifica la mappa dei caratteri da rimuovere
```

Tra, ottenere una stringa tra due stringhe
--------------------------

Spesso abbiamo bisogno di recuperare una stringa che è delimitata da altre due stringhe. Non c'è una funzione per questo direttamente in PHP, ma possiamo scriverla noi stessi:

```php
function between(string $left, string $right, string $data): string
{
   $l = explode($left, $data);
   $r = explode($right, $l[1]);

   return $r[0];
}
```

Espressioni regolari
--------------------------

Si può ottenere una suddivisione molto più elegante e lavorare con le stringhe usando <a href="/regex">espressioni regolari</a>, che discuto in una pagina separata.

Funzioni simili
--------------------------

- <a href="/funzione-implode">Implode()</a> - concatenare un array in una stringa

Esempio di analisi dell'indirizzo IP
--------------------------

```php
$ip = '10.0.0.138';
$parser = explode('.', $ip);
echo $parser[0]; // stampa "10"
echo $parser[1]; // stampa "0";
```

La variabile `$ip` contiene una stringa in ingresso che viene analizzata secondo il delimitatore `.`, il ritorno è un array. L'analisi procede fino alla fine della stringa, a meno che non sia specificato un limite.

Parametri
--------------------------

| # | Tipo | Descrizione
|---|--------|------|
| 1 | string | Stringa di separazione.
| 2 | string | Stringa analizzata.
| 3 | int | limite di analisi. Questo è un parametro opzionale. Esempio:

```php
$text = 'PHP è il mio linguaggio preferito!';
$parser = explode('', $text, 1);

echo $parser[0]; // stampa la prima parola
echo $parser[1]; // stampa il resto del testo
echo $parser[2]; // non produce nulla perché è stato impostato un limite!
```

Valori di ritorno
--------------------------

Il valore di ritorno è un array con una stringa analizzata.

Gli indici sono numerati da zero a `X` a meno che non sia specificato un limite.

Differenze dalle versioni precedenti
--------------------------

| Versione PHP | Descrizione |
|-----------|-------|
| 5.1.0 | Aggiunto il supporto per il limite negativo sui passaggi.
| 4.0.1 | Aggiunto il parametro opzionale **limit**.

Suggerimenti e note
--------------------------

Quando si usa un **limite** negativo, viene dato il numero di elementi dalla fine della stringa.

Esempio:

```php
$str = 'uno|due|tre|quattro';

// limite positivo
print_r(explode('|', $str, 2));

// limite negativo (da PHP 5.1)
print_r(explode('|', $str, -1));
```

Verranno restituiti i seguenti campi:

```php
Array
(
    [0] => one
    [1] => two|three|four
)

Array
(
    [0] => one
    [1] => two
    [2] => three
)
```
