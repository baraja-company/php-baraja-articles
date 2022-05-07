I cicli e i loro tipi in PHP
============================

> id: e2927790-d0fb-4de5-9848-01fdd088464b
> slug:
> 	cs: cykly
> 	it: i-cicli-e-i-loro-tipi-in-php
> 
> perex:
> 	- 'Cykl for, while, foreach a rekurze v PHP + vysvětlení rozdílů mezi cykly. Možnosti iterativního zpracování úloh.'
> 	- 'Ciclo for, while, foreach e ricorsione in PHP + spiegazione delle differenze tra i cicli. Possibilità di elaborazione iterativa dei compiti.'
> 
> publicationDate: '2020-04-11 18:56:34'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '297931d6e7b8c9e011dbbe06b414cdde'

I cicli in generale sono usati per ripetere la stessa sezione di codice (di solito su un insieme di dati) Per ogni tipo di compito, è adatto un diverso tipo di ciclo, che differisce principalmente nel significato e nella sintassi. È anche importante notare che tutti i tipi di loop sono convertibili tra loro, solo che non sempre vale la pena in termini di complessità del codice e tempo di calcolo.

La divisione di base dei loop in termini di tipo di elemento:

- `for`: un ciclo generico in cui conosciamo in anticipo il numero di ripetizioni, o possiamo semplicemente calcolarlo in anticipo,
- `while` e `until-while`: un ciclo generale in cui non si conosce in anticipo il numero di ripetizioni,
- `foreach`: scorrere gli array e gli oggetti,
- `recursione`: decomposizione di un problema complesso in un insieme di problemi più piccoli.

Proprietà generali dei loop
-----------------------

I cicli generalmente funzionano ripetendo un pezzo di codice marcato **più e più volte**, **fino a quando la condizione finale** è valida. Il ciclo viene fermato o fallendo nel soddisfare la condizione di terminazione, o fermandolo manualmente chiamando `break;`.

Se niente ferma il ciclo, allora niente gli impedisce di funzionare indefinitamente, o finché l'applicazione non viene terminata manualmente, o il limite di tempo per elaborare lo script PHP si esaurisce (di solito 30 secondi).

**Termini importanti:**

- `Cycle` - un'espressione del linguaggio che permette di ripetere una certa parte del codice
- `Iterazione` - una specifica esecuzione del corpo del ciclo
- `Initializzazione` - iniziare l'esecuzione di un ciclo (prima che inizi la prima iterazione)
- `Increment` - incrementa una variabile di iterazione di uno
- Condizione di uscita - Una condizione che viene verificata ad ogni iterazione (la posizione della verifica varia a seconda del tipo di ciclo). Il ciclo viene eseguito finché la condizione è soddisfatta

Per il ciclo
----------

`For` è utile per i casi in cui conosciamo in anticipo il numero di ripetizioni (o possiamo calcolarlo facilmente). È perfetto per l'attraversamento lineare di intervalli, per esempio.

È generalmente scritto (3 parti):

```php
for (inicializace; výraz; inkrementace) {
    // il corpo del ciclo
}
```

- `Initializzazione`: definizione dello stato iniziale del ciclo (quali variabili ci servono),
- `expression`: Condizione. Finché è valido, il ciclo verrà eseguito,
- `increment`: cambia lo stato del ciclo dal passaggio precedente (`increment` - incrementa di uno).

Un esempio potrebbe essere quello di produrre una serie di numeri da `0` a `10`:

```php
for ($i = 0; $i <= 10; $i++) {
    echo $i . '<br>';
}
```

O multipli di due da `2` a `100`:

```php
for ($i = 2; $i <= 100; $i += 2) {
    echo $i . '<br>';
}
```

Il ciclo for è utile per vari trucchi, come <a href="/abeceda-cisla-intervalli">ottenere l'alfabeto, un array di numeri e intervalli</a>.

Mentre un ciclo do-while
-----------------------

Un ciclo "While" viene eseguito finché la condizione è soddisfatta. La differenza tra `while` e `do-while` è quando la condizione sarà valutata.

```php
while (výraz) {
    // il corpo del ciclo
}
```

In alternativa:

```php
do {
    // il corpo del ciclo
} while(výraz);
```

- Il `while` prima valuta la condizione e poi esegue il ciclo interno,
- `do-while` elabora prima il ciclo interno e poi valuta la condizione.

Questo è particolarmente vantaggioso quando non possiamo sapere in anticipo se il ciclo verrà mai eseguito e vogliamo garantire che venga sempre eseguito almeno una volta (allora usiamo `do-while`).

Esempio:

> Abbiamo un numero memorizzato nella variabile `$x = 2`. Vogliamo anche generare un numero nell'intervallo `<0; 10>` nella variabile `$y` in modo che sia diverso dal numero nella variabile `$x`. In poche parole: genera un numero casuale tra `0` e `10` che non sia `2`.

In questo caso, è conveniente usare un ciclo `do-while`. Non sappiamo in anticipo quante volte il ciclo dovrà essere eseguito (potremmo essere sfortunati e continuare a colpire lo stesso numero, nel qual caso il ciclo verrà eseguito per molto tempo), e vogliamo anche garantire che venga eseguito almeno una volta prima che la condizione venga valutata, poiché dobbiamo prima generare il numero.

```php
$x = 2;
$y = $x;

do {
   $y = rand(0, 10);
} while($x == $y);

echo 'X :' . $x . '<br>';
echo 'Y:' . $y;
```

Foreach
-------

`foreach` è perfetto per sfogliare campi e oggetti. Possiamo ottenere non solo valori specifici, ma anche chiavi.

Se vogliamo ottenere solo valori specifici:

```php
foreach ($data as $value) {
    // elaborazione dei dati
}
```

In alternativa, possiamo prendere le chiavi:

```php
foreach ($data as $key => $value) {
    // elaborazione dei dati
}
```

`foreach` è perfetto per sfogliare dati reali - per esempio, risultati da un database, o campi con chiavi:

```php
$countries = [
    'it' => 'Repubblica Ceca',
    'Vedere' => 'Slovacchia',
];

foreach ($countries as $shortcut => $name) {
    echo $shortcut . ':' . $name . '<br>';
}
```

Ricorsione
-------

La ricorsione si verifica quando una funzione o un metodo chiama se stesso. Serve a scomporre un problema complesso in un gruppo di problemi più piccoli.

> Capire la ricorsione può essere impegnativo per i principianti perché si basa sull'idea di passare la responsabilità.
>
> La funzione dice effettivamente qualcosa del tipo "non posso risolvere questo problema, ma conosco qualcuno che può...", quindi lo chiama, lui chiama qualcuno, ... fino a quando il membro finale viene chiamato, e taglia il problema.

Vale certamente la pena notare che qualsiasi algoritmo ricorsivo può essere riscritto per usare i loop dove la ricorsione non è necessaria (è vero anche il contrario). La ricorsione è un buon servitore, ma un cattivo padrone. Aiuta a risolvere alcuni tipi di problemi in modo semplice e molto efficiente, mentre il looping attraverso i cicli è utile per altre cose.

In generale, la ricorsione è intensiva di memoria perché crea sempre nuove istanze e contesti di funzioni e metodi. Tuttavia, per attraversare strutture ad albero, per esempio, questo è l'unico modo ragionevole per risolvere il problema.

Esempio:

Dobbiamo calcolare il fattoriale del numero `5`, è così che si fa in matematica:

`5! = 5 * 4 * 3 * 2 * 1`

Cioè, c'è una moltiplicazione successiva di una serie di numeri da `1` fino al numero di cui ci interessa il fattoriale.

Ricorsivamente, questo può essere risolto in modo molto elegante:

```php
function factorial(int $n): int
{
   if ($n === 0) {
      return 1;
   }

   return $n * factorial($n - 1);
}
```

In alternativa, un'implementazione ancora più breve usando l'operatore ternario:

```php
function factorial(int $n): int
{
    return $n === 0 ? 1 : $n * factorial($n - 1);
}
```

Lo stesso può essere riscritto per usare i cicli:

```php
function factorial(int $n): int
{
    $result = 1;

    for ($i = $n; $i > 0; $i--) {
        $result *= $i;
    }

    return $result;
 }
```

La scrittura con un ciclo è leggermente più lunga, ma di nuovo ha un'impronta di memoria significativamente inferiore. Usiamo solo una variabile `$result` per il calcolo, dove cambiamo il valore continuamente e non dobbiamo creare nuove istanze di `factorial()`.

Scrivere i cicli infiniti con molta attenzione
-----------------------------------

Può essere molto facile che un ciclo diventi infinito (otteniamo questo risultato non soddisfacendo mai la condizione di terminazione). Dovreste tenerne conto e possibilmente fermare il ciclo voi stessi con `break;`.

Per esempio, tira il dado finché non esce il numero `6`. L'implementazione vi spinge a usare il ciclo `while`:

```php
while (true) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'Il valore è sceso' . $value;
      break;
   }
}
```

Scrivendo `while(true)` ci siamo assicurati un numero infinito di ripetizioni, perché `true` sarà sempre vero. Quindi dobbiamo fermare noi stessi il ciclo con il costrutto `break;`, ma cosa succede se il valore `6` non scende mai e non fermiamo mai il ciclo?

Personalmente, conto sempre il numero di ripetizioni e se supera qualche limite critico, termino forzatamente il ciclo. Per esempio, se superiamo i `1000` tentativi. In questo caso, è meglio usare `for` piuttosto che `while` e contare il numero di corse.

Se superiamo il numero di corse, è educato informare lo sviluppatore lanciando un'eccezione.

```php
for ($i = 0; $i <= 1000; $i++) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'Il valore è sceso' . $value;
      break;
   }
}

if ($i === 1000) {
   throw new \Exception('Il numero massimo di lanci è stato superato.');
}
```
