Come capire il codice PHP
=========================

> id: d8d452da-37b4-4502-a573-a27afe7ea37a
> slug:
> 	cs: jak-se-vyznat
> 	it: come-capire-il-codice-php
> 
> publicationDate: '2020-04-11 18:45:23'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c3012a19e6dd01770fad9ab456a5b6b3

La maggior parte delle lingue può essere scritta in modi diversi, con lo stesso risultato. Allo stesso tempo, una volta che avete scritto il codice, probabilmente lo leggerete in futuro e lo correggerete o aggiungerete nuove funzionalità.

Quindi, per evitare di dover pensare al codice tutto il tempo e per navigarlo bene, c'è un insieme di strumenti e modi per "scrivere bene il codice" direttamente in PHP, o per costruire il codice in un modo che supporti direttamente la sua futura leggibilità (anche da parte di un altro umano).

> **Nota dell'autore:**
>
> L'esperienza mostra che il codice diventa obsoleto così velocemente che anche l'autore stesso dell'applicazione percepisce il proprio codice come estraneo dopo mezzo anno. Quindi, se lo scriviamo correttamente dall'inizio, non impedirà la sua futura estensibilità.
>
> Nello sviluppo reale, è segretamente dimostrato che la formattazione uniforme del codice e l'introduzione di regole in generale previene un certo numero di errori.

TL;DR
-----

La leggibilità del codice è spesso legata alla formattazione e alle regole di scrittura.

All'interno dei team di sviluppo, ha senso stabilire regole formali per come formattiamo e manteniamo il codice.

Personalmente, uso (nel 2022) lo standard di codifica del framework Nette e le regole sono valutate automaticamente in ogni commit. Vedere l'articolo su [using GitHub CI](https://php.baraja.cz/github-actions-nejlepsi-ci-pro-rok-2021#hotove-priklady) per maggiori informazioni.

L'installazione del test standard di codifica e la sua esecuzione si fanno con un paio di comandi:

```shell
composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
php temp/coding-standard/ecs check src
```

Note nel codice
---------------

Le note non hanno alcun effetto sull'elaborazione del codice e sono solo per l'uso del programmatore. Per pezzi di codice più grandi e completi, è importante scrivere una nota che spieghi a cosa serve il codice e come funziona in linea di principio.

```php
// Definizioni delle variabili
$a = 5;
$b = 3;
$c = 2;

// Somma di tutti i numeri
$sum = $a + $b + $c;

// Lista per gli utenti
echo $sum;
```

La nota inizia con una coppia di slash (`//`) ed è valida fino alla fine della linea. Può essere usato ovunque.

La nota non dovrebbe spiegare l'implementazione specifica dell'algoritmo, ma piuttosto i suoi principi generali. Questo perché il codice può cambiare più volte nel tempo, nel qual caso dovremmo correggere anche la nota.

> **Nota dell'autore:**
>
> Spesso accade che il codice non faccia esattamente ciò che la sua descrizione spiega. Questo è dovuto principalmente al fatto che il programmatore ha fatto un errore da qualche parte. La nota dovrebbe quindi descrivere i principi generali in modo da poter poi modificare il codice di conseguenza. Ma non dimenticate mai che l'unica verità su ciò che accade realmente in un'applicazione è descritta solo dal codice reale, e la nota non ha alcun effetto su questo.

Separazione grafica delle parti di codice
----------------------------

Quando si progetta un'applicazione, è importante separare i blocchi logici l'uno dall'altro. Di solito sono separati in funzioni, metodi, o nel caso del codice di base almeno da commenti.

In un algoritmo più lungo, di solito descrivo prima l'intero principio dell'algoritmo all'inizio, e poi numero i singoli posti nel codice in modo che lo sviluppatore possa capire meglio la funzionalità specifica basata su di essi.

```php
/**
 * La funzione calcola la media aritmetica.
 *
 * 1. Ottenere una lista di numeri
 * 2. Ottenere la somma e il numero di numeri
 * 3. Calcolare e stampare la media
 */

// 1.
$numbers = [1, 3, 8, 12];

// 2.
$sum = array_sum($numbers);
$count = count($numbers);

// 3.
echo 'La media è:' . ($sum / $count);
```

I caratteri `/**` iniziano un commento multilinea che si applica fino al marcatore `*/`. Per facilitare la lettura, è una buona idea mettere un asterisco all'inizio di ogni riga.

Commenti sulla documentazione
----------------------

I commenti alla documentazione sono di solito usati per descrivere e documentare le funzioni (comportamento, parametri, valori di ritorno, autore, ecc.).

Nelle versioni precedenti di PHP (prima della `7.0`), i tipi di dati non erano ancora utilizzati, quindi il tipo di una particolare variabile era descritto direttamente nel commento.

```php
/**
 * @autore Jan Barášek <jan@barasek.com>
 * @licenza MIT
 * @link https://php.baraja.cz
 * @param float[] $numeri
 */
function average(array $numbers): float
{
    $sum = array_sum($numbers);
    $count = count($numbers);

    return $sum / $count;
}
```

I commenti alla documentazione sono chiamati `documentazione' principalmente perché hanno un formato pre-concordato che è compreso da specifici ambienti di sviluppo (ed editor), ma anche da strumenti automatizzati per generare documentazione o controllare il codice.

Scrivere codice in ceco o in inglese?
-----------------------------

Scrivo tutto il codice solo in inglese (compresi i nomi delle funzioni, le variabili, i commenti, ...).

Questo ha una serie di vantaggi:

- Lo sviluppatore può allenare proattivamente il suo inglese da subito.
- Gran parte dell'applicazione utilizza librerie di terze parti che sono in inglese, quindi mantiene automaticamente la coerenza
- La maggior parte della roba avanzata non ha affatto la traduzione in inglese
- Sono sicuro che puoi pensare a molti altri esempi

PHP non richiede direttamente l'inglese e si può scrivere tutto in inglese. Vedo l'uso dell'inglese più come una sorta di investimento per il futuro, e un'opportunità per estendere facilmente il codice da parte di altre persone che non sono di madrelingua inglese.

Il codice completamente localizzato in inglese è usato anche nelle aziende, quindi è bene praticare l'inglese fin dall'inizio.

Ordine delle operazioni numeriche
------------------------

Tieni sempre presente che PHP arrotonda i numeri quando esegue operazioni numeriche. Questo può essere spesso una seccatura, poiché qualsiasi risultato con numeri decimali è accompagnato da una certa imprecisione.

Una buona soluzione sembra essere quella di incrementare prima i numeri e poi calcolare con i numeri più grandi possibili. In questo modo, c'è statisticamente meno distorsione.

Esempio:

```php
echo 10 / 3; // Scrive 3.3333333333333
```

In alcuni casi, si può anche usare il trucco di non usare affatto i decimali e calcolare tutto come un numero intero. In questo caso, non c'è questa distorsione:

```php
echo 1 / 2 * 2; // questo è peggio perché 1/2 = 0,5*2 = 1
echo 2 * 1 / 2; // questo è meglio perché 2*1 = 2/2 = 1
```

Quando risolvi operazioni numeriche grandi e complesse, usa le frazioni per scrivere i numeri.
