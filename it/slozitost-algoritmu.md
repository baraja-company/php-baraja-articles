Complessità degli algoritmi
===========================

> id: f0baa25a-4cff-4d3d-b758-5e03f4a8c69c
> slug:
> 	- null
> 	it: complessita-degli-algoritmi
> 
> cs: slozitost-algoritmu
> publicationDate: '2021-08-03 20:40:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '6269d38b9a2a8d75ec01b569af8b371c'

Ogni algoritmo ha la sua complessità, che può essere espressa in notazione matematica. Questa panoramica mostra la complessità tipica degli algoritmi in base alla dimensione dei dati di input (cioè il numero di elementi con cui lavorano) e quali tipi di algoritmi sono adatti a quale tipo di compito.

In generale, c'è un algoritmo specializzato migliore per ogni tipo di problema. Nessun algoritmo è universalmente migliore, e bisogna sempre conoscere il proprio contesto.

Notazione Big O
--------------

La *Notazione Big O* è usata per classificare gli algoritmi in base a come il loro tempo di esecuzione o i requisiti di memoria aumentano con l'aumentare delle dimensioni dell'input.

Il seguente grafico mostra gli ordini di crescita più comuni degli algoritmi specificati in Big O Notation.

Di seguito è riportato un elenco di alcune delle notazioni Big O più comunemente usate e un confronto delle loro prestazioni rispetto alle diverse dimensioni dei dati di input.

| Notazione Big O | Complessità per 10 elementi | Complessità per 100 elementi | Complessità per 1000 elementi |
| -------------- | ---------------------------- | ----------------------------- | ------------------------------- |
| **O(1)** | 1 | 1 | 1 |
**O(log N)** | 3 | 6 | 9 |
| 10 | 100 | 1000 |
**O(N log N)** | 30 | 600 | 9000 |
**O(N^2)** | 100 | 10000 | 1000000 |
**O(2^N)** | 1024 | 1.26e+29 | 1.07e+301 |
**O(N!)** | 3628800 | 9.3e+157 | 4.02e+2567 |

Complessità delle operazioni sulla struttura dei dati
----------------------------------

| Struttura di dati | Accesso | Ricerca | Inserisci | Rimuovi | Commenta |
| ----------------------- | :-------: | :-------: | :-------: | :-------: | :-------- |
| 1. n, n, n, n, n, n, n, n, n.
**Stack** | n | n | n | 1 | 1 | |
**Coda** | n | n | n | 1 | | |
| **Lista collegata** | n | n | n | 1 | n
| Nel caso di una funzione hash perfetta, la complessità sarà O(1) |
| **Albero di ricerca binario** | n | n | n | n | Nel caso di un albero bilanciato, la complessità sarà O(log(n)).
| **B-Tree** | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) |
| **Albero Rosso-Nero** | log(n) | log(n) | log(n) | log(n) |
| **AlberoAVL** | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) |
| Quando si cercano i "falsi positivi".

Complessità degli algoritmi di ordinamento
----------------------------

| Nome dell'algoritmo | Migliore | Media | Peggiore | Memoria | Stabile? | Commento |
| --------------------- | :-------------: | :-----------------: | :-----------------: | :-------: | :-------: | :-------- |
| **Bubble sort** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | Sì | |
| **Insertion sort** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | | |
| **Selection sort** | n<sup>2</sup> | n<sup>2</sup> | n<sup>2</sup> | 1 | | |
| **Heap sort** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | 1 | | No |
| **Merge sort** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | n | Sì |
| | **Quick sort** | n&nbsp;log(n) | n&nbsp;log(n) | n<sup>2</sup> | log(n) | No | Quicksort è solitamente eseguito con complessità dello stack O(log(n)).
| **Shell sort** | n&nbsp;log(n) | dipende dalla sequenza | n&nbsp;(log(n))<sup>2</sup> | 1 | | No |
| **Counting sort** | n + r | n + r | n + r | n + r | Sì | r - il numero più grande nella matrice |
| **Radix sort** | n * k | n * k | n * k | n + k | Sì | k - lunghezza della chiave più lunga |
