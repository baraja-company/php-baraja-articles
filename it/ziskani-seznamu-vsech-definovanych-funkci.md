Ottenere un elenco di tutte le funzioni definite
================================================

> id: '99ed7887-33c8-44d7-b0cb-ac37b2336f48'
> slug:
> 	cs: ziskani-seznamu-vsech-definovanych-funkci
> 	it: ottenere-un-elenco-di-tutte-le-funzioni-definite
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '9dbffadf6cf6473eedadbc0bf7eccbb4'

A volte può essere utile ottenere una lista di tutte le caratteristiche disponibili nell'ambiente corrente. Questo è specialmente il caso quando stiamo gestendo il server di qualcun altro e abbiamo bisogno di orientarci.

La lista delle funzioni può essere ottenuta chiamando la funzione `get_defined_functions()`, che restituisce dati sotto forma di array:

```txt
[
   internal => [
      ...,
   ],
   user => [
      ...,
   ]
]
```

L'elenco delle funzioni è diviso in due grandi liste.

- Le funzioni "interne" sono quelle definite da PHP stesso e dalle estensioni installate.
- Le funzioni utente (`utente`) sono quelle definite dal codice utente stesso. Queste sono tutte le funzioni che abbiamo scritto nel codice sorgente, o che sono incluse nelle librerie installate.

Questo elenco può essere ben utilizzato per il debug di un'applicazione.
