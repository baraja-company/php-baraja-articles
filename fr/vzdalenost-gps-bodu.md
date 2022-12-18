Distance entre deux points GPS
==============================

> id: '9e67de8f-922a-4f96-886a-07f0002b66ad'
> slug:
> 	cs: vzdalenost-gps-bodu
> 	fr: distance-entre-deux-points-gps
> 
> publicationDate: '2022-12-18 11:00:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: d466d432cddc147949c75ef28fbaf503

La distance approximative de deux points GPS à vol d'oiseau peut être facilement calculée par l'algorithme :

Mise en œuvre de PHP
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

Cette fonction suppose que la Terre est une sphère parfaite, donc utilisez la sortie comme une estimation seulement. Pour un calcul réaliste de la distance, vous devez encore tenir compte de l'altitude, de la forme du terrain et de l'aplatissement local.

J'utilise cette fonction pour estimer la distance des boutiques en ligne. En République tchèque, l'écart moyen est d'environ 5 mètres pour 1 km, ce qui est suffisant en fonctionnement réel.

Mise en œuvre en TypeScript
--------------------------

Il se peut que vous deviez effectuer le calcul sur le client, c'est là que le javascript s'avère utile :

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

Comment rechercher des lieux proches dans la base de données
------------------------------------

Pour trouver les emplacements environnants dans la base de données autour d'un point spécifique, vous pouvez soit utiliser les calculs coûteux de la fonction listée ci-dessus directement en SQL, soit le faire intelligemment.

Personnellement, j'utilise une combinaison d'une paire d'algorithmes pour trouver des emplacements proches (par exemple, des branches à la caisse d'un magasin électronique pour un client).

Dans la première étape, vous chargez simplement une zone carrée autour du point sélectionné (vous l'obtenez en soustrayant simplement une valeur constante du point souhaité, puis en recherchant l'intervalle). Nous obtenons ainsi une liste de résultats possibles, mais nous ne savons pas encore à quel point ils sont éloignés les uns des autres, et nous ne savons rien de leur ordre.

Dans la deuxième étape, nous devons déterminer si nous recherchons des points à l'intérieur d'un cercle particulier, ou si nous voulons trouver des points plus éloignés sur le carré. Personnellement, je recommande de parcourir à vélo cette liste de cadidats pour calculer la distance depuis le point de départ de chaque résultat, et de classer les résultats en fonction de la distance calculée.

Dans la troisième étape, vous pouvez utiliser un filtre simple pour supprimer les emplacements qui se trouvent à une distance supérieure à celle souhaitée par rapport au cercle défini.
