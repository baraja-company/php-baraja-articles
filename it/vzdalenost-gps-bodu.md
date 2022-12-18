Distanza tra due punti GPS
==========================

> id: '9e67de8f-922a-4f96-886a-07f0002b66ad'
> slug:
> 	cs: vzdalenost-gps-bodu
> 	it: distanza-tra-due-punti-gps
> 
> publicationDate: '2022-12-18 11:00:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: d466d432cddc147949c75ef28fbaf503

La distanza approssimativa di due punti GPS in linea d'aria può essere facilmente calcolata dall'algoritmo:

Implementazione PHP
------------------

```php
function getCoordsDistance(
	float $lat1,
	float $lng1,
	float $lat2,
	float $lng2
): float {
	$r = 6371;
	$lat1 = ($lat1 / 180) * M_PI;
	$lat2 = ($lat2 / 180) * M_PI;
	$lng1 = ($lng1 / 180) * M_PI;
	$lng2 = ($lng2 / 180) * M_PI;

	$x = ($lng2 - $lng1) * cos(($lat1 + $lat2) / 2);
	$y = $lat2 - $lat1;
	$d = sqrt($x * $x + $y * $y) * $r;

	return round($d * 1000);
}
```

Questa funzione presuppone che la Terra sia una sfera perfetta, quindi utilizzare il risultato solo come stima. Per un calcolo realistico della distanza è necessario tenere conto dell'altitudine, della forma del terreno e dell'appiattimento locale.

Utilizzo questa funzione per stimare la distanza dei punti vendita elettronici. Nella Repubblica Ceca, la deviazione media è di circa 5 metri per ogni 1 km, sufficiente per il funzionamento reale.

Implementazione in TypeScript
--------------------------

Potrebbe essere necessario eseguire il calcolo sul client, ed è qui che javascript si rivela utile:

```js
export const getCoordsDistance = (
    lat1: number,
    lng1: number,
    lat2: number,
    lng2: number
): number => {
  const r = 6371;
  const subLat1 = (lat1 / 180) * Math.PI;
  const subLat2 = (lat2 / 180) * Math.PI;
  const subLng1 = (lng1 / 180) * Math.PI;
  const subLng2 = (lng2 / 180) * Math.PI;

  const x = (subLng2 - subLng1) * Math.cos((subLat1 + subLat2) / 2);
  const y = subLat2 - subLat1;
  const d = Math.sqrt(x * x + y * y) * r;

  return Math.round(d * 1000 * 100000000) / 100000000;
};
```

Come cercare i luoghi vicini nel database
------------------------------------

Per trovare le posizioni circostanti nel database intorno a un punto specifico, è possibile utilizzare i costosi calcoli della funzione sopra elencata direttamente in SQL, oppure farlo in modo intelligente.

Personalmente, utilizzo una combinazione di un paio di algoritmi per trovare le località vicine (ad esempio, le filiali per il checkout da un e-store a un cliente).

Nel primo passo, si carica un'area quadrata attorno al punto selezionato (si ottiene semplicemente sottraendo un valore costante dal punto desiderato e cercando poi l'intervallo). Questo ci dà un elenco di risultati candidati, ma non sappiamo ancora esattamente quanto siano distanti tra loro e nulla sul loro ordine.

Nella seconda fase, dobbiamo capire se stiamo cercando punti all'interno di un particolare cerchio o se vogliamo trovare punti più distanti sul quadrato. Personalmente, consiglio di scorrere l'elenco dei cadidati per calcolare la distanza dal punto di partenza di ciascun risultato e di ordinare i risultati in base alla distanza calcolata.

Nella terza fase, è possibile utilizzare un semplice filtro per rimuovere le posizioni che si trovano a una distanza superiore a quella desiderata dal cerchio definito.
