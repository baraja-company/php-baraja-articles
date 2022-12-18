Afstand mellem to GPS-punkter
=============================

> id: '9e67de8f-922a-4f96-886a-07f0002b66ad'
> slug:
> 	cs: vzdalenost-gps-bodu
> 	da: afstand-mellem-to-gps-punkter
> 
> publicationDate: '2022-12-18 11:00:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: d466d432cddc147949c75ef28fbaf503

Den omtrentlige afstand mellem to GPS-punkter i luftlinje kan let beregnes af algoritmen:

PHP-implementering
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

Denne funktion antager, at Jorden er en perfekt kugle, så brug kun resultatet som et skøn. For at få en realistisk afstandsberegning skal du stadig tage højde for højde, terrænform og lokal udjævning.

Jeg bruger denne funktion til at vurdere afstanden til e-butikker. I Tjekkiet er den gennemsnitlige afvigelse ca. 5 meter for hver 1 km, hvilket er tilstrækkeligt i den virkelige drift.

Implementering i TypeScript
--------------------------

Det kan være nødvendigt at foretage beregningen på klienten, og det er her, at javascript er praktisk:

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

Sådan søger du efter nærliggende steder i databasen
------------------------------------

Hvis du vil finde de omkringliggende steder i databasen omkring et bestemt punkt, kan du enten bruge de dyre beregninger i funktionen ovenfor direkte i SQL, eller du kan gøre det på en smart måde.

Personligt bruger jeg en kombination af et par algoritmer til at finde nærliggende steder (f.eks. filialer, hvor en e-butik skal betale kassen til en kunde).

I det første trin indlæser du blot et kvadratisk område omkring det valgte punkt (du får dette ved simpelthen at trække en konstant værdi fra det ønskede punkt og derefter søge i intervallet). Dette giver os en liste over mulige resultater, men vi ved endnu ikke præcis, hvor langt de er fra hinanden, og vi ved heller ikke noget om deres rækkefølge.

I det andet trin skal vi finde ud af, om vi leder efter punkter inden for en bestemt cirkel, eller om vi ønsker at finde mere fjerne punkter på kvadratet. Personligt anbefaler jeg, at du cykler gennem denne liste over kadidater for at beregne afstanden fra startpunktet for hvert resultat og ordner resultaterne efter den beregnede afstand.

I det tredje trin kan du bruge et simpelt filter til at fjerne steder, der er mere end den ønskede afstand fra den definerede cirkel.
