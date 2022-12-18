Avstånd mellan två GPS-punkter
==============================

> id: '9e67de8f-922a-4f96-886a-07f0002b66ad'
> slug:
> 	cs: vzdalenost-gps-bodu
> 	sv: avstand-mellan-tva-gps-punkter
> 
> publicationDate: '2022-12-18 11:00:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: d466d432cddc147949c75ef28fbaf503

Algoritmen kan enkelt beräkna det ungefärliga avståndet mellan två GPS-punkter i kråkfågelvägen:

Implementering av PHP
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

Funktionen utgår från att jorden är en perfekt sfär, så använd resultatet endast som en uppskattning. För en realistisk avståndsberäkning måste du fortfarande ta hänsyn till höjd, terrängens form och lokal utjämning.

Jag använder den här funktionen för att uppskatta avståndet till e-butiker. I Tjeckien är den genomsnittliga avvikelsen cirka 5 meter per 1 km, vilket är tillräckligt i verklig drift.

Genomförande i TypeScript
--------------------------

Du kan behöva göra beräkningen på klienten, det är där javascript är praktiskt:

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

Hur man söker efter närliggande platser i databasen
------------------------------------

Om du vill hitta de omgivande platserna i databasen runt en viss punkt kan du antingen använda de dyra beräkningarna i funktionen ovan direkt i SQL eller göra det på ett smart sätt.

Personligen använder jag en kombination av ett par algoritmer för att hitta närliggande platser (t.ex. grenar för att betala ut från en e-butik till en kund).

I det första steget laddar du bara ett kvadratiskt område runt den valda punkten (du får detta genom att helt enkelt subtrahera ett konstant värde från den önskade punkten och sedan söka i intervallet). Detta ger oss en lista över möjliga utfall, men vi vet ännu inte exakt hur långt ifrån varandra de ligger och vi vet ingenting om deras ordning.

I det andra steget måste vi ta reda på om vi letar efter punkter inom en viss cirkel eller om vi vill hitta mer avlägsna punkter på kvadraten. Personligen rekommenderar jag att du cyklar igenom listan över kadidater för att beräkna avståndet från startpunkten för varje resultat och ordnar resultaten efter det beräknade avståndet.

I det tredje steget kan du använda ett enkelt filter för att ta bort platser som ligger mer än det önskade avståndet från den definierade cirkeln.
