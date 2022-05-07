Matematica in PHP
=================

> id: '69a432ee-7635-46e2-bb21-8492cb62c4e6'
> slug:
> 	cs: matematika
> 	it: matematica-in-php
> 
> perex:
> 	- 'Matematika a matematické funkce v PHP. Definice čísel, operátorů a vhodné typy pro výpočty a ukládání čísel.'
> 	- 'Matematica e funzioni matematiche in PHP. Definizioni di numeri, operatori e tipi appropriati per calcoli e memorizzazione di numeri.'
> 
> publicationDate: '2020-02-16 18:24:10'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '1070f11c80eff1faf3388e47721b01a2'

Come in altri linguaggi, i numeri in PHP sono rappresentati nel sistema binario (il sistema di uno e zero), quindi alcune operazioni matematiche possono produrre risultati leggermente diversi da quelli che ci aspetteremmo. Questa pagina dà una panoramica del comportamento dei calcoli in diversi contesti. Per la comprensione è richiesta almeno una conoscenza di base delle **tecniche numeriche** e dei **sistemi numerici**.

Rappresentazione dei numeri in memoria
---------------------------

Il modo in cui i numeri sono rappresentati è caratterizzato dal tipo di dati, per esempio integer è convertito direttamente dal codice sorgente da decimale a binario. Non dobbiamo preoccuparci di convertire i numeri, tutto viene fatto automaticamente.

La conseguenza di questa conversione è che il valore massimo di un intero è determinato dal numero di bit disponibili in memoria. Su 32-bit PHP la gamma è da `-2.147.483.648` a `-2.147.483.647` (~ ± 2 miliardi), 64-bit PHP ha una gamma da `-9.223.372.036.854.775.808` a `-9.223.372.036.854.775.807` (~ ± 9 quintilioni). Il valore massimo può sempre essere ottenuto chiamando la costante `PHP_INT_MAX`.

I numeri decimali sono memorizzati nel tipo di dati **float**, che ha una precisione limitata, quindi è memorizzato internamente come una coppia di numeri che sono soggetti all'operazione matematica `mantissa * (2^esponente)`. La conseguenza pratica è che siamo in grado di memorizzare un numero relativamente grande (dell'ordine di miliardi) in un piccolo numero di bit, solo non molto preciso.

La conseguenza dell'uso del tipo di dati **float** è che il risultato del calcolo di `1 - 0.9` non è esattamente `0.1`.

Esempio da <a href="https://diskuse.jakpsatweb.cz/?action=vthread&forum=9&topic=97912">web forum</a>:

```php
$x = 0.3;
$y = 0.4;
echo 0.7 - $x - $y; // stampa -5.5511151231258E-17
```

Se aggiustiamo leggermente i numeri:

```php
$x = 6.5;
$y = 7.5;
echo 14.0 - $x - $y; // stampa 0
```

In generale, questo problema è <a href="https://vtm.zive.cz/clanky/proc-pocitacum-delaji-problemy-desetinna-cisla/sc-870-a-185672/default.aspx">discusso in un articolo separato su VTM.e15.cz: Perché i computer hanno problemi con i numeri decimali</a>, o in generale su <a href="https://cs.wikipedia.org/wiki/Pohyblivá_řádová_čárka">punto fisso su Wikipedia</a>.

> In generale, è una buona idea arrotondare il risultato dopo il calcolo, o evitare del tutto i decimali. Per esempio, memorizzate il prezzo non in corone (come 14,90) ma in centesimi (come 1490) e poi lavorate esattamente con esso. L'arrotondamento è discusso nella prossima sezione di questo articolo.

Operazioni matematiche
-------------------

```php
$a = 5;
$b = 3;

echo $a + $b;
```

Si possono usare convenzioni matematiche comuni per le operazioni, che rispettano la precedenza degli operatori secondo le regole della matematica (la moltiplicazione ha la precedenza sull'addizione, ecc.) I valori possono essere scritti direttamente o letti tramite le variabili.

> Potrebbe non essere ovvio a prima vista, ma nell'esempio di matematica, il segno di uguale (`=`) non è scritto. Quindi, ne consegue che si possono risolvere solo semplici `operazioni binarie`, che funzionano sempre con solo `due elementi` (non contare le equazioni, devi usare una libreria specializzata per questo).

Panoramica degli operatori di base
----------------------------

> Nella colonna `risultato`, elenco ciò che viene stampato assumendo:

```php
$a = 5;
$b = 3;
$c = 10;
```

| Operazione | Segno | Segno in parole | Esempio di notazione | Risultato
|-------------------|-----------|---------------|-----------------------|----------
| Aggiunta | `+` | Più | `echo $a + $b;` | 8
| Sottrazione | `-` | Meno | `echo $a - $b;` | 2
| Moltiplicazione | `*` | Asterisco | `echo $a * $b;` | 15
| divisione | `/` | slash | `echo $a / $b;` | 1.66666666667
| parentesi | `( )| parentesi | `echo $a + ($b * $c);`| 35
| concatenazione di stringhe | `.` | Periodo | `echo $a. $b. $c;` | 5310
| Rimane dopo la divisione | `%` | Percentuale | `echo $a % $b;` | 2
| Aggiungendo uno | `++` | due plus | `echo $a++;` | 6
| Sottrarre uno | `--` | due meno | `echo $a--;` | 4
| Power | `**` | due asterischi | `echo $a ** $b;` | 125

> L'operatore di potenza (doppio asterisco) è disponibile solo a partire da PHP 7.1. Nelle altre versioni di PHP devi usare la funzione universale `pow($a, $b)`.

Panoramica delle funzioni matematiche di base
---------------------------------------

Se state risolvendo qualche compito computazionale comune, molto probabilmente c'è già una funzione direttamente in PHP, ecco una panoramica commentata di esse.

Arrotondamento
--------------

L'arrotondamento normale è fatto con la funzione <a href="/funzione-round">round()</a>, ha 3 parametri:

- Numero arrotondato
- Opzionale: Precisione (fino a quante cifre decimali)
- Opzionale: valore dimezzato (da quale valore arrotondare per eccesso)

```php
round(5);               // 5
round(5.1);             // 5
round(5.4);             // 5
round(5.5);             // 6
round(5.8);             // 6
round(5483.47621, 2);   // 5483.47
round(5483.47621, -2);  // 5500
```

C'è anche una funzione <a href="function-floor">floor()</a> per arrotondare per difetto e una <a href="/function-ceil">ceil()</a> per arrotondare per eccesso.

> **Attenzione ai tipi di dati!
>
> Tutte le funzioni di arrotondamento restituiscono il risultato come `float`, anche se il risultato è un intero.
>
> > Se volete trattare il risultato come un intero, è importante sovrascrivere il risultato. Per esempio: `echo (int) round(3.14);`

PHP lavora con diversi tipi di dati, per esempio con **float** può facilmente accadere che il risultato di `round()` non sia un numero con espansione decimale finita. Per l'output, raccomando di usare la funzione `number_format()`, di cui parlo sotto.

Funzioni goniometriche
--------------------

Questi sono usati per molti calcoli diversi e sono basati sul cerchio unitario. Si usano, per esempio, per tracciare cerchi, ellissi, spostamenti e così via.

- <a href="/funzione-sin">sin()</a>
- <a href="/funzione-cos">cos()</a>
- <a href="/funzione-tan">tan()</a>

Funzioni ciclometriche
--------------------

Calcola l'angolo da `x` e `y`. Conversione di coordinate cartesiane e polari.

- <a href="/funzione-asin">asin()</a>
- <a href="/funzione-acos">acos()</a>
- <a href="/funzione-atan">atan()</a>

Funzioni iperboliche
-------------------

Basato sull'iperbole unitaria. Le loro definizioni possono essere facilmente descritte usando delle similitudini:

- Ellisse, cerchio, cerchio con raggio 1
- Iperbole, iperbole equiaxiale, iperbole unitaria

Sono utilizzati, per esempio, nella generazione del terreno e nella simulazione fisica.

- <a href="/funzione-sinh">sinh()</a>
- <a href="/funzione-cosh">cosh()</a> (catena)
- <a href="/funzione-tanh">tanh()</a>

Funzioni iperbolometriche
------------------------

Basato sull'iperbole unitaria.

- <a href="/funzione-asinh">asinh()</a>
- <a href="/funzione-acosh">acosh()</a>
- <a href="/funzione-atanh">atanh()</a>

Esempi di utilizzo delle funzioni goniometriche
---------------------------------------

```php
echo sin(30);            // -0.98803162409286
echo sin(deg2rad(30));   // 0.5
echo cos(deg2rad(123));  // -0.54463903501503
echo 1/tan(deg2rad(45)); // 1 (onen cotangente)
echo sin(deg2rad(500));  // 0.64278760968654
echo atan2(deg2rad(50), deg2rad(23)); // 1.1396575860761
```

Calcolatrice - elaborazione di espressioni matematiche
--------------------------------------------

A volte può succedere che abbiamo bisogno di elaborare un'espressione matematica come una stringa utente, per esempio `5+2^(1+3/2)`.

Non c'è un modo semplice per elaborare tali input in PHP, ma ho programmato una serie di librerie che affrontano questo problema.

Per i dettagli, vedi l'articolo separato <a href="/pokrocila-calculator">Calculator in PHP: elaborare un'espressione matematica come una stringa</a>.

Operazioni con grandi numeri e precisione dei calcoli
------------------------------------------

I numeri grandi e i decimali sono memorizzati come float in PHP, in più sono arrotondati a circa 15 cifre decimali, che a volte può essere fastidioso. Questo è il motivo per cui è stata creata la **<a href="https://www.php.net/manual/en/book.bc.php">biblioteca matematica di precisione arbitraria</a>**.

I numeri sono rappresentati come **stringhe** in questa libreria, quindi non sono limitati dall'imprecisione di **float**, ma le operazioni eseguite sono diversi ordini di grandezza più lente (ma ancora più veloci che se programmassimo noi stessi una tale funzione).

Per esempio, se vogliamo ottenere una radice quadrata da 2 a 3 cifre decimali, basta chiamare:

```php
echo bcsqrt('2', 3); // 1.414
```
