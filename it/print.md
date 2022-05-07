Stampa
======

> id: '23d672c5-b347-43b8-bd08-8ccb7ecca33f'
> slug:
> 	cs: print
> 	it: stampa
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7f7c3c32d9f3c1efd794ca38b7db384

`print` - Uscita della stringa

Descrizione
--------------------------

```php
print 'Ciao, mondo!';
```

`print()` non è in realtà una vera funzione (è un costrutto del linguaggio), quindi non è necessario usare le parentesi.

Parametri
--------------------------

- **argomento

Parametro di uscita

Valori di ritorno
--------------------------

Restituisce sempre il numero 1.

Nota
--------------------------

Nota: poiché questo è un **costrutto linguistico** (non una funzione), non può essere caricato in una variabile.

Esempio
--------------------------

```php
print "Ciao, mondo";

print "print" può emettere più righe di testo, ma attenzione al tag HTML
perché non si stampa. Questo è ciò che il <a href="/nl2br">Nl2br</a>.";

// Esempio di collegamento con la variabile
$a = 'php';

print 'Mi piace' . $a; // Mi piace php
```

**print** è esattamente la stessa funzione di **echo**. Se stai cercando maggiori informazioni, controlla l'articolo <a href="/echo">echo</a>.
