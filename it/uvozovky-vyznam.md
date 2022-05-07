Significato speciale delle virgolette
=====================================

> id: '2649942d-94d6-48c3-9c78-5a303165bf72'
> slug:
> 	cs: uvozovky-vyznam
> 	it: significato-speciale-delle-virgolette
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c1b63b98a3e9d9c642247c363747616a

In PHP, si possono spesso sostituire le virgolette con gli apostrofi e ottenere lo stesso effetto. A volte usiamo anche una combinazione di virgolette e apostrofi di proposito per ottenere un risultato diverso senza dover usare l'escape.

**TLDR: Usare le virgolette può essere pericoloso in alcuni contesti, quindi usate invece gli apostrofi ovunque. Le virgolette sono adatte solo per casi speciali.**

| Carattere | Nome |
|------|-----------
| `"` | virgolette |
| `'`` Apostrophe |

Esempio uno - stesso risultato
-----------------------------

```php
echo "Ciao, mondo!"; // questo è lo stesso
echo 'Ciao, mondo!'; // come questo
```

In questo caso, l'uso delle virgolette o degli apostrofi non ha importanza. Così, in superficie, può sembrare che non ci sia differenza tra le virgolette e gli apostrofi.

Combinazione di virgolette e apostrofi
------------------------------

Considerate il seguente codice:

```php
echo 'Il mio colore preferito è il "rosso"!';
```

Se usassi i "ritorni a capo" per delimitare la stringa che viene scritta, sarebbe **ambiguo** dove la stringa **inizia** e dove finisce**, così ho usato le "apostrofi" per delimitare la stringa, il che risolve questo problema.

Inserire le virgolette in una stringa delimitata da virgolette
---------------------------------------------------

Occasionalmente, ci possono essere situazioni in cui ho bisogno di elencare sia `quote` che `apostrofi` all'interno della stessa stringa. Potete usare non solo la concatenazione di due stringhe, ma anche il cosiddetto **escaping** dei caratteri.

```php
echo "Questa stringa "contiene" virgolette.";
```

Il backslash in questo caso ha il significato di "esattamente questo carattere" e quindi la virgoletta non sarà vista come la fine di una stringa (o altro) e quindi sarà sempre una virgoletta. Altri caratteri strani possono essere marcati in questo modo quando la loro durata non può essere garantita e il significato corretto potrebbe essere frainteso.

Variabile all'interno di una stringa
-----------------------

Le virgolette possono essere estremamente complicate perché permettono di inserire variabili direttamente in una stringa:

```php
$color = 'Rosso';

echo "Moje oblíbená barva je {$color}, a tvoje?";
```

Molto più sicuri sono gli apostrofi, che non lo permettono e bisogna piegare la stringa:

```php
$color = 'Rosso';

echo 'Il mio colore preferito è' . $color . 'e il tuo?';
```

Buone abitudini
--------------------------

In generale, raccomando di usare gli apostrofi (se possibile) per delimitare qualsiasi cosa, poiché sono molto meno comuni nelle stringhe rispetto alle semplici virgolette.

Inoltre, PHP è un linguaggio web, cioè è usato per generare documenti HTML, dove le virgolette sono molto comuni proprio perché sono usate anche per generare parti di codice HTML. Personalmente, consiglio di abituarsi a usare rigorosamente gli apostrofi ovunque, perché così non si deve ricordare cosa si sta racchiudendo.

Comportamento delle virgolette
--------------------------

Attenzione! Non buttate via completamente le virgolette! Hanno alcuni vantaggi speciali che possono essere utili per il lavoro avanzato in PHP - tuttavia, i principianti li considerano come errori e non li capiscono.

```php
$x = 10; // imposta una variabile
echo "Hodnota proměnné je: $x, přesně."; // e un elenco
```

All'interno delle virgolette, potete anche elencare i valori delle variabili, o il segno del dollaro fa sì che tutto ciò che lo segue sia una variabile. Quindi, se non volete emettere il valore di una variabile, ma il segno del dollaro, dovrete evadere.

```php
$price = 25; // prezzo in dollari
echo "Cena produktu: $price\$"; // stampa "Prezzo del prodotto: $25"
```

Il vantaggio delle virgolette è discutibile in questo caso e potrebbe essere meglio usare gli apostrofi e collegare semplicemente le stringhe.

```php
$price = 25;
echo 'Prezzo del prodotto:' . $price . '$'; // stampa lo stesso dell'esempio precedente
```

> **Nota:** Il prezzo in dollari è correttamente scritto nel formato `$25`, il che causerebbe ancora più confusione, perché la notazione `$$price` in realtà chiama qualcosa chiamato una `variabile variabile` (nel nome della variabile si chiama il nome della variabile da chiamare - solo confusione).

**TIP:** Potresti essere interessato a <a href="/promenna-variabile">variabile variabile</a>.

Designazione della stringa
--------------------------

In generale, qualsiasi cosa tra virgolette o apostrofi è trattata come una stringa. Così:

```php
$x = 5;   // questo è qualcos'altro,
$x = "5"; // di questo.
```

Nel primo caso il **numero** 5 è memorizzato nella variabile **$x**, nel secondo caso la **stringa** "5" è memorizzata nella stessa variabile. Fortunatamente, questo non ha importanza in PHP, e si può lavorare con entrambe le varianti (quasi) allo stesso modo, perché PHP può **trasformare** automaticamente le variabili secondo il loro contenuto. Tuttavia, è generalmente raccomandato di non scrivere i numeri tra virgolette, specialmente per le operazioni di calcolo dove possono verificarsi errori di arrotondamento.

A volte potrei voler memorizzare l'output di una funzione in una variabile:

```php
$pi = 3.14159;
$zaokrouhleni = round($pi); // questo è corretto
$zaokrouhleni = "round($pi)"; // questo è "sbagliato" (la domanda è quale output mi aspetto).
```

L'errore nel secondo caso è proprio nelle virgolette, quando l'output della funzione **round()** non è memorizzato nella variabile **$round**, ma nella stringa che chiama questa funzione.
> **TIP:** Il valore `$pi` non deve essere inserito direttamente nello script in questo modo e possiamo usare la funzione `pi()`, che è più precisa per calcoli più complessi.

Significato speciale di fuga
--------------------------

> **Attenzione:** I seguenti esempi funzionano solo tra virgolette, se li racchiudi in apostrofi si comporteranno come caratteri regolari senza il significato speciale (tranne ``'`` che sfugge all'apostrofo).
L'escape è per quando voglio emettere qualche carattere speciale tra virgolette o un apostrofo che potrebbe essere interpretato come un'espressione linguistica e quindi processato, anche se il programmatore non lo ha inteso. Abbiamo già mostrato un esempio; questa sezione descrive le possibili eccezioni comportamentali.

Questo perché a volte la fuga stessa ha un significato speciale. Esempio:

```php
echo "Testo lungo, diviso in due righe.";
```

L'esempio precedente stampa:

```php
Dlouhý text, rozdělený na
dva řádky.
```

Quindi, se vogliamo scrivere una barra, dobbiamo anche fare l'escape (l'escape del carattere `n` non è necessario in questo caso, perché verrebbe inteso di nuovo come un wrap, o in questo caso non dobbiamo fare alcun escape):

```php
echo "Testo lungo, diviso in due righe.";
```

Ci sono altri caratteri speciali come questo, per esempio `\t` fa un tab. La lista completa è nella documentazione ufficiale.

Uso reale dei caratteri speciali nell'escaping
-----------------------------------------------

Per cominciare, vorrei sottolineare che quasi tutto può essere fatto in PHP in più modi, e gli esempi dati qui sono solo per illustrare come il problema può essere affrontato.

Per esempio, se vogliamo analizzare il testo riga per riga, possiamo usare la funzione <a href="/explode">explode</a>.

```php
$parser = explode("\n", $retezec); // analizza il testo riga per riga
```

In questo caso, l'uso del carattere speciale `\n` ha senso, perché possiamo dire molto efficacemente che vogliamo analizzare per interruzioni di riga.

```php
$parser = explode('
', $retezec);
```

> **Attenzione:** Sui sistemi Unix e Windows, i caratteri usati per marcare la fine delle linee sono confusi. Per esempio, Windows usa `CRLF` (una coppia di caratteri `r\n`), mentre Linux usa solo `LF` (un singolo carattere `\n`). Quando si analizza per linea, questo dovrebbe essere tenuto a mente. Di solito il problema si risolve normalizzando i caratteri solo a `LF`.

Riassunto
-------

Se puoi, usa **apostrofi** ovunque.

È bene conoscere l'uso delle virgolette e usarle solo quando è necessario (o generalmente buono). Se state emettendo del testo che può contenere delle virgolette, racchiudetelo in apostrofi (che poi si comportano in modo più prevedibile). Personalmente, uso le virgolette per esprimere vari caratteri speciali che sono inutilmente complessi da inserire negli apostrofi e richiederebbero un escaping complesso.
