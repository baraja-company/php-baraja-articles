Filosofia di base della programmazione orientata agli oggetti
=============================================================

> id: c38214d9-8c92-4484-9c31-239d716f2545
> slug:
> 	cs: filosofie-oop
> 	it: filosofia-di-base-della-programmazione-orientata-agli-oggetti
> 
> publicationDate: '2020-01-02 22:21:56'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3841046b40a039413bacd045f275c68

La programmazione orientata agli oggetti è un paradigma, una visione di come programmare. Vedrete presto da soli che l'OOP porta una semplificazione abbastanza fondamentale a tutti i problemi e le difficoltà comuni che si risolvono più e più volte nella programmazione reale.

L'idea di base
-----------------

L'idea di base della programmazione orientata agli oggetti è quella di dividere una grande applicazione (un compito complesso) in molte piccole parti che possiamo risolvere in modo elegante e indipendente.

Per esempio, se stiamo programmando un sistema di prenotazione di biglietti aerei, è un progetto molto complesso che risolve migliaia di casi. Se possiamo decomporre l'intera logica complessa in molti livelli e parti, possiamo facilmente capire l'intero sistema complesso e programmare i singoli sottocompiti in modo indipendente.

Vantaggi pratici di OOP e della divisione del codice in oggetti
------------------------------------------------

A parte il punto di vista accademico, ci sono molte ragioni pratiche per usare OOP:

- L'applicazione creerà un <a href="/incapsulamento">incapsulamento</a>, il che significa che i dati sono sempre elaborati in un contesto locale, che sono pochi, quindi non dobbiamo pensare all'intera applicazione in una volta sola.
- Possiamo dividere un'applicazione in centinaia di migliaia di piccole parti che possono essere sviluppate da persone diverse in parallelo e organizzare solo un'interfaccia comune.
- Possiamo guardare l'applicazione dalla prospettiva di diversi livelli (astrattamente a interi moduli, o localmente a specifiche funzioni che risolvono uno specifico algoritmo).
- Quando si esegue il debugging dell'applicazione (debugging) siamo in grado di fare dei passi nell'applicazione, omettere o sostituire alcune parti.
- Possiamo applicare meglio i **design patterns** e quindi prevenire errori e complessità nel codice prima che si verifichino.

Personalmente, non riesco a immaginare squadre con più di una persona che programmano in modo diverso.

> **Nota:**
>
> L'uso di oggetti mette un po' più di memoria e di CPU sul computer, quindi questo ridurrà un po' le prestazioni dell'applicazione. In un ambiente reale, tuttavia, questo non importa, perché riprogrammare l'applicazione a oggetti fa perdere un po' di prestazioni (di solito unità di percentuale), ma fa risparmiare tempo ai programmatori (di solito decine o centinaia di percentuale). Il tempo umano è sempre molto più costoso (e molto limitato) del tempo del computer.
>
> > Non dimenticate anche che OOP porta una grande semplificazione a tutta l'applicazione e permette di completare grandi applicazioni in un tempo ragionevole. Un gran numero di applicazioni complesse sarebbe quasi impossibile da programmare senza oggetti.

Relazione con il mondo reale
-------------------------

L'obiettivo fondamentale della OOP nella progettazione del software è quello di simulare il più possibile le proprietà, i comportamenti e i principi del mondo reale. Gli oggetti in OOP rappresentano entità reali. Questo modo di pensare ci permette di costruire enormi sistemi complessi che possono essere ben compresi, risolvere internamente problemi del mondo reale come sarebbero risolti senza un computer, e i principi possono essere spiegati a persone reali.

Per esempio, se stiamo implementando un'applicazione di gestione dei contenuti, ha senso disporre tutta la logica interna in molte entità reali (articolo, autore, categoria) e costruire le sessioni non secondo ID di record generati artificialmente (come si fa convenzionalmente nei database) ma secondo relazioni reali.

Esempio di implementazione concreta:

```php
class Article
{
    private Author $author;

    /** @var Categoria[] */
    private array $categories;

    private string $title;
}

class Author
{
    private string $name;
}

class Category
{
    private string $name;
}
```

Come possiamo vedere, la classe `Article` non contiene solo parametri tecnici come l'ID del record con l'autore, ma è un vero e proprio legame all'entità Author, che di nuovo ha le sue proprietà.

Per una spiegazione dell'implementazione specifica e della sintassi, vedere il <a href="/uvod-do-oop">tutorial introduttivo</a>.
