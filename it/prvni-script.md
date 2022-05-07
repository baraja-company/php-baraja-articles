Come scrivere il tuo primo script PHP
=====================================

> id: '2d6986dc-f24b-4ae5-b24c-d5bb9bf94512'
> slug:
> 	cs: prvni-script
> 	it: come-scrivere-il-tuo-primo-script-php
> 
> perex:
> 	- Jak napsat první PHP script? Úvod do PHP pro začátečníky.
> 	- Come scrivere il tuo primo script PHP? Introduzione al PHP per principianti.
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: a5fab0e5edf99323140e497a6fe52322

Prima di scrivere il nostro primo script PHP, dobbiamo prima spiegare teoricamente come caricare una pagina usando PHP.

1. In primo luogo, l'utente chiama un URL specifico nel suo browser web, per esempio `https://baraja.cz`.
2. Poi il browser web costruisce una `request`, che è una richiesta speciale al server web che viene inviata a Internet. Contiene informazioni sulla pagina richiesta, identificazione e impostazioni di base del browser, informazioni sui cookie e così via.
3. Questa "richiesta" viaggia attraverso Internet verso un server web (più spesso "Apache"), che legge la richiesta e inizia a compilare una risposta.
4. Poiché l'URL chiamato è uno script PHP e la richiesta è per un file chiamato `index.php`, `Apache` legge il file `index.php` dalla directory principale sul disco e lo passa all' `interprete PHP`, che è un programma che può elaborare il codice PHP stesso e costruire il `codice HTML` basato su di esso, che viene inviato all'utente.
5. Una volta che il codice HTML è compilato, la risposta è rimandata all'utente (questa è chiamata "risposta") e il browser web rende la pagina in modo normale, come se fosse puro HTML.

> Nota che il browser web non apprende nulla del contenuto dello script PHP, ma elabora solo l'HTML generato, quindi i tuoi script e il contenuto del server rimangono al sicuro.

Creiamo il primo script
----------------------------

> La scrittura del tuo primo script presuppone che tu abbia un server web in esecuzione sul tuo computer. Per Windows, <a href="https://www.apachefriends.org/index.html">XAMPP</a> è meglio (scarica PHP versione 7.0 o successiva), e <a href="https://www.apachefriends.org/index.html">XAMPP</a> funziona esattamente allo stesso modo su Mac come su Windows. Per Linux, raccomando <a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">LAMP server</a> (anche questo sito gira su Lamp server).

Il nome del file di script PHP deve finire con l'estensione `.php` in modo che il server web sappia che vogliamo processarlo secondo le regole PHP. Quindi creiamo un file `index.php`, per esempio, che conterrà il codice per la pagina principale del nostro sito web.

Aprire il file
--------------------------

Aprite questo file in un editor di testo adatto a scrivere il codice sorgente.

In Windows, per esempio, <a href="https://www.sublimetext.com">Sublime Text</a> è un buon punto di partenza, poiché colora piacevolmente la sintassi (regole del linguaggio) e rende il codice più facile da leggere. In seguito, consiglio di comprare <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, che è molto usato nelle aziende e offre la possibilità di programmare in più persone.

Scrivere la struttura di base di una pagina HTML
--------------------------

Probabilmente conoscete già la struttura di base di una pagina HTML:

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>

    </body>
</html>
```

Tutto il codice HTML sarà gestito in modo normale e sarà di grande aiuto per la progettazione del sito. PHP usa i principi di HTML e CSS in larga misura.

Separazione dello script PHP dal codice HTML
--------------------------

PHP è principalmente un linguaggio di template che genera contenuti personalizzati in punti appropriati del codice. Per poter dire chiaramente cosa è HTML e cosa è PHP, dobbiamo usare un tag separatore.

Attualmente, è meglio usare la notazione con `<?php` e `?>`.

```php
// ecco il codice PHP
?>
```

> Usiamo il terminatore `?>` se vogliamo usare qualche altro codice HTML. Se non c'è altro codice HTML alla fine dello script PHP, è preferibile non includere il tag `?>`, in modo che non ci siano inutili spazi bianchi e interruzioni di riga alla fine della pagina che l'editor di testo può inserire.

In passato, il tag `<?` è stato usato molto al posto di `<?php`, ma potrebbe non essere sempre supportato.

I tag Wrapper possono essere messi ovunque nel codice HTML, come nel corpo della pagina:

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>
    <?php
        // tady bude PHP kód
    ?>
    </body>
</html>
```

Costrutti di base
--------------------------

Tra gli elementi di base ci sono:

- <a href="/echo">Elenco dei contenuti nel codice sorgente</a>
- <a href="/variabile">Variabili</a>
- <a href="/if">Condizioni</a>

In questa sezione, dimostreremo un semplice elenco di contenuti al codice sorgente usando una variabile.

Il principio della costruzione del codice sorgente
--------------------------------

Tutti i costrutti (espressioni del linguaggio), dichiarazioni e funzioni sono separati da punti e virgola per rendere inequivocabile da dove e verso dove è valido il costrutto corrente.

Un punto e virgola è solitamente seguito da un'interruzione di riga.

Scritto simbolicamente:

```php
příkaz;
další příkaz;
proměnná x = její hodnota;

vypsat proměnnou x;

uložit do souboru;
```

Estrarre il codice sorgente
--------------------------

Il costrutto <a href="/echo">echo</a> è usato per elencare il contenuto.

È molto facile da usare:

```php
echo 'Ciao, mondo!';
```

Poi stampa il testo "Hello world!" nel codice HTML. Prova il campione.

> Tutti gli altri demo conterranno solo il codice PHP all'interno. Il codice HTML che lo circonda è libero di farsi un'idea propria (per esempio, usate l'esempio all'inizio di questo articolo).

Variabili
--------------------------

Le variabili sono posizioni di memoria virutale che immagazzinano dati e sono usate per spostarli. Il nome di una variabile inizia sempre con un `dollaro`, seguito dal `nome` stesso, e poi dal suo `valore`.

> Ho riassunto una descrizione dettagliata di come funzionano le variabili in <a href="/variabile">un articolo separato sulle variabili</a>.

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo;
echo '<br>';
echo $jmenoAutora;
```

> Il nome della variabile dovrebbe esprimere ciò che la variabile contiene effettivamente per rendere il codice più chiaro. Notate anche l'inserimento del tag HTML `<br>` per indentare il testo. Dovresti avere già familiarità con questo tag dell'HTML.

Ciò che viene emesso nel costrutto `echo` è chiamato stringa (una sequenza di caratteri). Le singole stringhe possono essere concatenate con un punto (`.`) per ridurre l'output a una sola riga:

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo . '<br>' . $jmenoAutora;
```

> Dopo aver collegato le stringhe con un punto, il tutto sarà visto come una grande stringa.

Operazioni variabili
--------------------------

Tra le variabili, tutte le <a href="/matematica">operazioni matematiche di base</a> funzionano perfettamente in modo intuitivo come previsto.

Definiamo 2 variabili e mettiamoci dentro dei numeri:

```php
$x = 5; // definisce la variabile x con il valore 5
$y = 3; // definisce una variabile y con valore 3

echo $x + $y; // aggiunge le variabili e stampa 8
```

> Nota che il segno di uguale (`=`) non è usato per eseguire un'operazione matematica, quindi non puoi scrivere equazioni, per esempio. PHP si comporta come una calcolatrice in questo senso.

Se non vogliamo usare variabili, possiamo eseguire le operazioni direttamente. Quindi il luogo delle operazioni non ha importanza e valutano ovunque.

```php
echo 5 + 3; // stampa 8
```

In alternativa, possiamo aggiungere le variabili e memorizzare il risultato in un'altra variabile:

```php
$x = 5;
$y = 3;
$z = $x + $y; // la variabile $z contiene 8

echo $z; // stampa 8
```

Nella prossima parte, impareremo le basi complete di <a href="/principi-di-variabili">definizione e uso delle variabili</a>.
