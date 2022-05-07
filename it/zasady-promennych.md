Principi di scrittura delle variabili
=====================================

> id: '4c8e631b-b305-4169-8885-4f9506155999'
> slug:
> 	cs: zasady-promennych
> 	it: principi-di-scrittura-delle-variabili
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '5528d9b73c1d1c07c330a58e4aeaa06a'

Questa è la seconda parte di una serie di tutorial su PHP. In questo episodio, vedremo le regole di base per scrivere le variabili.

Questa pagina è solo una rapida panoramica. Se state cercando una descrizione tecnica dettagliata di tutte le caratteristiche, ho scritto un <a href="/variabile">articolo separato</a>.

Sintassi di base
--------------------------

Le variabili in PHP iniziano con il segno del dollaro `$` seguito immediatamente dal nome.

```php
$zvire = 'cat';
```

Le stringhe (sequenze di caratteri) sono racchiuse tra virgolette o apostrofi:

```php
$a = "Virgolette";
$b = 'apostrofi';
```

Le cifre non sono racchiuse tra virgolette:

```php
$a = 5;
$b = 10;
$c = 3.14159;
```

Il nome della variabile può consistere solo in caratteri dell'alfabeto inglese e numeri. Il nome inizia sempre con una lettera.

Se il nome consiste di più di una parola, si usa usare la sintassi `camelCase` (prima lettera minuscola e ogni altra parola che inizia con una lettera maiuscola):

```php
$kocka = 'Kitty';
$rychlyPocitac = 'Certo che è mio!';
$pocetRohuJednorozce = 1;
```

Il nome non deve contenere spazi, trattini, uncini, virgole, virgolette, parentesi o altri caratteri speciali. L'unico carattere speciale permesso è il `underline`.

I numeri decimali devono essere scritti con un punto:

```php
$pi = 3.14159;
```

Spesso può essere utile eseguire operazioni matematiche direttamente quando si definisce una variabile:

```php
$a = 5;
$b = 3;
$c = $a + $b;	// aggiungere 5 + 3
echo $c;		// stampa 8
```

Inserimento corretto di una virgoletta o di un apostrofo
--------------------------

Le virgolette e gli apostrofi non devono essere combinati arbitrariamente. Per esempio, se decidiamo di usare le virgolette, dobbiamo anche terminare la stringa con le virgolette e non usarle all'interno.

Questo è quindi sbagliato:

```php
echo "<img src="obrazek.gif">";
```

Perché non è chiaro dove inizia e finisce la catena. Pertanto, le virgolette e gli apostrofi non possono essere annidati.

Una possibile soluzione si chiama <a href="/escapovani">escaping</a>, dove il carattere problematico è preceduto da un backslash.

```php
echo "<img src="image.gif">";
```

Il backslash dice che il prossimo carattere sarà esattamente quello che vogliamo usare.

Tuttavia, per l'output di codice HTML, è preferibile racchiudere l'intera stringa in apostrofi e poi usare gli apici nel modo normale:

```php
echo '<img src="image.gif">';
```

In alternativa, può essere invertito:

```php
echo "<img src='picture.gif'>";
```

Riempire una variabile da un indirizzo url o da un modulo
--------------------------

Gli indirizzi che contengono un punto interrogativo portano informazioni sulle variabili di input, così per esempio `index.php?page=contacts` denota la variabile `page` con il valore `contacts`. Il valore di questa variabile viene letto come `$_GET['page']`.

Il carattere punto interrogativo non è in alcun modo legato al nome del file su disco. È sempre lo stesso file a cui passiamo i parametri nell'indirizzo.

Discuto questo problema in dettaglio nel mio articolo su <a href="/metodi-odesilani-dat">metodi di invio dati</a>.

Definire il contenuto di una variabile da un indirizzo
--------------------------

Alcune variabili sono già disponibili al momento dell'esecuzione dello script (e possono quindi essere utilizzate subito), queste sono chiamate **variabili superglobali**. Per esempio, se vogliamo leggere un valore da un URL, usiamo la variabile `$_GET`.
L'uso è il seguente:

```php
$a = $_GET['a'];

echo $a;
```

Questo script stampa ciò che ha nell'URL dopo il punto interrogativo nel codice sorgente.

> Attenzione, questo campione non è sicuro! Se un visitatore disonesto passasse, per esempio, del codice HTML nell'URL, questo verrebbe inserito nella pagina ed eseguito. Pertanto, dobbiamo sempre trattare l'output; la funzione `htmlspecialchars()` è usata per questo.

```php
$a = $_GET['a'];

echo htmlspecialchars($a);
```

> Se accediamo alla pagina senza specificare il parametro `?a=qualcosa`, la variabile `$_GET['a']` non esisterà e PHP lancerà un messaggio di errore. Dobbiamo trattare questa condizione con una condizione e non fare nulla se la variabile non esiste (o in alternativa, emettere un contenuto alternativo). L'esistenza della variabile può essere verificata con la funzione `isset()`.

```php
if (isset($_GET['a'])) {
    $a = $_GET['a'];

    echo htmlspecialchars($a);
} else {
    echo 'La variabile "a" non esiste!';
}
```

Esempio con conteggio
--------------------------

Con le variabili dell'indirizzo URL possiamo fare pezzi di cane, per esempio, sommarle e scrivere direttamente il risultato:

```php
echo $_GET['a'] + $_GET['b'];
```

Se vogliamo includere più parametri di input nell'URL, dobbiamo separarli con una e commerciale (`&`). L'indirizzo può essere così: `index.php?a=5&b=3`.

Collegamento di input di testo (stringhe)
--------------------------

Possiamo anche collegare facilmente 2 input di testo (stringhe). Questo viene fatto usando l'operatore dei punti. Si può collegare in una variabile o al momento dell'annuncio.

```php
$a = 'cane';
$b = 'cat';

echo $a . 'a' . $b;
```

C'è scritto 'cane e gatto'.
