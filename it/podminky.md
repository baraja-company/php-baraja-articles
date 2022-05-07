Condizioni e ramificazioni
==========================

> id: '2cea5541-6879-4763-a518-cb21bf9021dd'
> slug:
> 	cs: podminky
> 	it: condizioni-e-ramificazioni
> 
> publicationDate: '2019-09-07 20:25:57'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: bc4140fc49413be3f2f2b2264b8ea5e6

> **Attenzione:** Questo articolo è stato scritto molti anni fa e alcune informazioni potrebbero essere obsolete o errate. Per favore, tenetelo a mente quando leggete.

Niente più programmi lineari! Il principio più elementare di qualsiasi programma è "ciò che accade quando....". Una condizione può essere scritta come una dichiarazione logica, che può essere valida (la condizione è soddisfatta) o non valida (allora non viene eseguita o viene eseguito il suo esatto contrario). Entrambi sono facili da definire.

Notazione generale
------------

In generale, una condizione può essere scritta come una dichiarazione logica. La condizione può essere soddisfatta o meno. È una buona idea contare entrambe le opzioni possibili. Se ci sono più alternative, si chiama una **condizione annidata**.

Esempio:

```php
if (hodnota   operace   hodnota) {
	// Questo si attiva se la condizione è valida
} else {
	// Questo si attiva se la condizione non si applica
}
```

Non dobbiamo sempre definire entrambe le opzioni (a volte è completamente inutile). Infatti, possiamo definire la situazione se solo la condizione è soddisfatta. Questo viene fatto come segue:

```php
if (hodnota   operace   hodnota) {
	// Questo si attiva se la condizione è valida
}
```

Operatori logici
--------------------------

| Operatore | Significato
|----------|---------
| `==` | Equals
| `===` | Equals and has the same data type (*qualsiasi cosa può essere confrontata con qualsiasi cosa, ma la condizione è soddisfatta solo se è un valore dello stesso tipo di dati (ad esempio numero, testo, ...)*)
| `!=` | Non è uguale
| Uguale o maggiore di
| Uguale o inferiore a
| `<` | Maggiore
| Meno

Esempio reale
--------------------------

```php
$a = 5;
$b = 3;
if ($a === $b) {
	// blocco che viene stampato se $a è uguale a $b
} else {
	// blocco che viene stampato se $a NON è uguale a $b
}
```

Condizioni annidate
--------------------------

Sfortunatamente, l'output è solo `true` (valido) e `false` (non valido). Quindi, se vogliamo considerare più possibilità, dobbiamo annidare più condizioni una dentro l'altra. Questa è chiamata una **condizione annidata**. È annidato perché una delle soluzioni alla condizione è solo un'altra condizione.

```php
$a = 5;         // tasca sinistra
$b = 3;         // tasca destra
$kapsa = true;  // Ho una tasca?

if ($kapsa === true) {

	if ($a > $b) {
		echo 'Nella tasca sinistra c'è più';
	} else {
		echo 'Nella tasca destra c'è più';
	}

} else {
	echo 'Non hai una tasca';
}
```
