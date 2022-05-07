Introduzione alla programmazione orientata agli oggetti in PHP
==============================================================

> id: '44a79461-dcfd-4dd0-b6fd-ba5db76db6de'
> slug:
> 	cs: uvod-do-oop
> 	it: introduzione-alla-programmazione-orientata-agli-oggetti-in-php
> 
> publicationDate: '2020-01-01 19:54:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '888db2cf35845892eebade9fdcc18871'

Benvenuti al primo articolo del corso online OOP in PHP. Per una lista completa degli articoli, visita la <a href="/oop">pagina panoramica</a>.

> **Note sul contenuto:**
>
> L'obiettivo di questa serie è di **spiegare al meglio l'essenza** della programmazione orientata agli oggetti in modo da non dover passare centinaia di ore a sperimentare cose che non hanno senso. Tutte le tecniche e i tutorial provengono dalla pratica e sono spiegati come avrei voluto leggerli io stesso quando stavo imparando a programmare. Alcuni **concetti sono molto semplificati** a scapito della correttezza dei fatti al 100%, perché credo che sia sempre **meglio capire principalmente i principi** e avere almeno un'idea dei problemi che perdersi nella programmazione e vedere tutto come un gran casino.
>
> Come vedrete presto, programmare in OOP ha enormi vantaggi per il vostro codice. Porterà un nuovo livello di astrazione sulla vostra applicazione e vi permetterà di risolvere problemi molto complessi che non sarebbero possibili in altro modo.

Cosa sono gli oggetti
------------------

Nel concetto di base della programmazione, gli oggetti sono tipi di dati speciali che, oltre al proprio valore, possono definire stato dettagliato, metodi, comportamento, e permettono di eseguire varie operazioni, come nel mondo reale.

Un oggetto nella programmazione è qualcosa come una "cosa" del mondo reale che possiamo nominare (per esempio, con un nome) e ha proprietà come una cosa del mondo reale. Per esempio, un oggetto può essere un articolo che ha alcuni valori (titolo, contenuto, autore, data di pubblicazione, ...) e metodi (inserire un nuovo titolo, leggere il contenuto, calcolare il numero di giorni dalla pubblicazione, ...). In questo articolo, ci occuperemo principalmente di come creare un oggetto e lavorare con esso ad un livello base.

Oggetti e classi
---------------

Come abbiamo già stabilito, un **oggetto è qualcosa che esiste**. La parola "esiste" è molto importante in questa fase, perché, come vedremo presto, c'è ancora un'implementazione pratica da affrontare nella programmazione.

Siccome nella programmazione stiamo scrivendo codice e un oggetto è qualcosa di "vivente", da questo punto in poi distingueremo tra due concetti diversi in cui è importante capire il significato in dettaglio e distinguere rigorosamente questo:

- Un **oggetto** è qualcosa di "vivo" che esiste in questo momento (ha un'istanza), e può eseguire qualcosa o reagire ad altri oggetti.
- Una **classe** è codice scritto staticamente che deve ancora essere valutato e rimane sempre lo stesso.

Poiché scriviamo sempre il codice come una stringa statica (testo) quando programmiamo, distingueremo i programmi in `classi` (definizione statica dell'intero oggetto) e `oggetti` (un'istanza `vivente` della classe).

Esempio di definizione di una classe:

```php
class Article
{
    public string $title;

    public ?string $autor = null;
}
```

> **Note pratiche:**
>
> In un'applicazione reale, ogni classe è solitamente scritta in un file PHP separato che ha lo stesso nome della classe.
>
> Quindi per la classe `Article`, per esempio, creiamo `src/Article.php`. Questa convenzione non è richiesta in PHP, ma aiuta a rendere l'applicazione più leggibile in modo da non avere un sacco di codice insieme in un unico posto.
>
> Ogni classe può esistere al massimo una volta in tutta l'applicazione (avere un nome unico), ma ci possono essere (quasi) infinite istanze (a seconda della capacità della RAM). Inoltre, una singola classe non può essere suddivisa in più file e deve essere definita in un unico posto in una volta sola. Se avete bisogno di creare più classi con lo stesso nome, usate **namespaces**.
>
> Mostreremo anche in seguito che un codice ben strutturato permette di essere riutilizzato in molti progetti.

Questo codice definisce una classe `Article` con due proprietà (`title` e `author`). I commenti sono ignorati da PHP e sono usati solo per il controllo statico dei tipi (tipicamente letti da PhpStorm).

Codice:

```php
public string $title;
```

significa la definizione di `properties`, che è una proprietà della classe `Article`. Possiamo pensare a questo come se `Articolo` fosse un tipo di campo e `titolo` il suo indice, in cui possiamo scrivere un valore. Ogni proprietà deve avere un'accessibilità definita (`pubblica`, `protetta` o `privata`), spiegheremo più avanti cosa significa ogni impostazione. Se non sapete quale accessibilità scegliere in un caso particolare, mettete `public`.

Istanziare una classe e creare un oggetto
----------------------------------

Il concetto di `instanza` è uno dei concetti più difficili da capire, quindi è importante che leggiate molto attentamente le righe seguenti e vi raccomando di fare una buona prova con tutti gli esempi.

Se la nostra applicazione ha già una classe definita nel codice sorgente, in realtà non è usata da nessuna parte e PHP non lo sa. Questo perché la OOP introduce il "principio di incapsulamento dei dati", il che significa che una classe ha un contesto interno (locale) che è valido solo all'interno della classe. Questo perché nello sviluppo senza oggetti, eravamo abituati a valutare sempre il codice nella sua interezza. Inoltre, provate a pensare ad una classe come ad una qualche forma di funzione o programma chiamato esternamente.

Ora possiamo creare la nostra prima istanza, usando il nuovo operatore `new` per farlo.

```php
$myArticle = new Article;
$myArticle->title = 'Il mio primo articolo'; // scrive il valore

echo $myArticle->title; // elenca "Il mio primo articolo"
```

Nota che mettiamo l'intero oggetto `Article` creato dall'operatore `new` nella variabile `$myArticle`. Questo significa che l'oggetto è effettivamente un "tipo di dati", quindi possiamo spostarlo attraverso le variabili e continuare a lavorare con esso.

L'operatore freccia `->` è usato per manipolare l'oggetto. Se abbiamo un'istanza di un particolare oggetto (abbiamo una variabile con il suo contenuto), possiamo facilmente scrivere valori alle proprietà interne, o leggerle o cambiarle in vari modi usando i metodi (vedremo più avanti).

L'**Instanza di un oggetto è la creazione di una copia locale "live" della classe in memoria (variabile).

Creare più istanze della stessa classe allo stesso tempo
---------------------------------------------

Come abbiamo già discusso, ogni oggetto ha un contesto locale che si applica nei suoi interni. Grazie a questo principio, possiamo creare molte istanze indipendenti della stessa classe e manipolarle liberamente. Possiamo anche creare un numero dinamico di istanze e, per esempio, memorizzarle come elementi di un array. Le possibilità sono infinite.

> **Attenzione:**
>
> Quando si crea un numero estremo di istanze (centinaia o più), tenere a mente l'impronta di memoria, poiché PHP ha bisogno di mantenere le informazioni su tutte le istanze nella RAM. In pratica, però, il problema dell'overflow di memoria dovuto a molte istanze non si verifica.

Esempio:

```php
$firstArticle = new Article;
$firstArticle->title = 'Il mio primo articolo';

$secondArticle = new Article;
$secondArticle->title = 'Su un cane e un gatto';

echo $firstArticle->title;  // elenca "Il mio primo articolo"
echo $secondArticle->title; // scrive "Su un cane e un gatto"
```

Notate che l'elenco della proprietà `titolo` (`->titolo`) dipende da quale istanza stiamo leggendo.

Riassunto
-------

Abbiamo mostrato come definire la nostra prima classe e creare un'istanza di essa (un oggetto) in cui abbiamo scritto diversi valori.

Nella prossima puntata, spiegheremo il concetto di <a href="/methods-and-passing-input">Costruttore, metodi e passaggio di input</a>.
