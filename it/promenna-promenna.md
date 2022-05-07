Variabili Variabili
===================

> id: a0baeb3a-ab6e-4dd9-b1bc-b1872a12ee08
> slug:
> 	cs: promenna-promenna
> 	it: variabili-variabili
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '590515cc711ec27cc0f0bfe190c8fbff'

> **Attenzione:** Questo articolo è stato scritto molti anni fa e alcune informazioni potrebbero essere obsolete o errate. Per favore, tenetelo a mente quando leggete.

Le **variabili** non sono destinate all'impiego comune (risolvono problemi che possono essere risolti in altri modi), sono usate principalmente per rendere le scritture più concise e gli accessi alla memoria più complessi.

Considerate il seguente esempio:

```php
$x = 25;                  // contiene 25
$nacitana_promenna = 'x'; // contiene "x"
$y = $$nacitana_promenna; // contiene 25
echo $y;                  // stampa 25
```

Notate i due dollari che si susseguono. In questo caso, la variabile $y sarà caricata con il valore della variabile che ha il nome contenuto in $nacitana_variabile.

Un po' confuso, eh? Ecco perché è meglio non usare le variabili.
> **Nota:** Le variabili variabili sono una specialità di PHP a causa del segno del dollaro. In altri linguaggi, l'inizio del nome della variabile non è marcato con nessun carattere, quindi non si possono usare variabili variabili perché sarebbe ambiguo quando è una variabile classica e quando non lo è.
