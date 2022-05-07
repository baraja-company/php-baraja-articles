Funzioni pure in PHP
====================

> id: d94c18a9-8bc4-4377-8042-e7c8b48320a2
> slug:
> 	cs: pure-funkce
> 	it: funzioni-pure-in-php
> 
> publicationDate: '2021-10-27 10:30:00'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: e47a3507ddb189a218ad8e3a38703e5c

Nella programmazione funzionale, c'è un concetto di **funzione pura**, che si riferisce a una funzione che restituisce sempre lo stesso output allo stesso input (cioè è deterministica), e allo stesso tempo non soffre di effetti collaterali (cioè non influenza il suo ambiente).

Come appare una funzione pura
----------------------

Esempio di funzione pura:

```php
// Questa è una funzione pura
function add(int $a, int $b): int
{
	return $a + $b;
}
```

Questa è una funzione pura perché l'output è sempre lo stesso in base agli argomenti in ingresso.

Cosa non è una funzione pura
-------------------

```php
// Questa è una funzione impura
function add(int $a, int $b): int
{
	echo 'Aggiungendo...';
	file_put_contents('file.txt', 'Valore:' . $a);
	return $a + $b;
}
```

Questo tipo di funzione non è puro perché la funzione cambia il file system. Un altro tipo di funzione impura è quando interagisce con il database, stampa sullo schermo e così via.
