Distance between two GPS points
===============================

> id: '9e67de8f-922a-4f96-886a-07f0002b66ad'
> slug:
> 	cs: vzdalenost-gps-bodu
> 	en: distance-between-two-gps-points
> 
> publicationDate: '2022-12-18 11:00:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: d466d432cddc147949c75ef28fbaf503

The approximate distance of two GPS points as the crow flies can be easily calculated by the algorithm:

PHP implementation
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

This function assumes that the Earth is a perfect sphere, so use the output as an estimate only. For a realistic distance calculation you still need to take into account altitude, terrain shape and local flattening.

I use this function to estimate the distance of e-shop outlets. In the Czech Republic, the average deviation is approximately 5 meters for every 1 km, which is sufficient in real operation.

Implementation in TypeScript
--------------------------

You may need to do the calculation on the client, that's where javascript comes in handy:

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

How to search for nearby places in the database
------------------------------------

To find the surrounding locations in the database around a specific point, you can either use the expensive calculations of the function listed above directly in SQL, or you can do it smartly.

Personally, I use a combination of a pair of algorithms to find nearby locations (for example, branches to checkout from an e-store to a customer).

In the first step, you just load a square area around the selected point (you get this by simply subtracting a constant value from the desired point, and then searching the interval). This gives us a list of candidate outcomes, but we don't yet know exactly how far apart they are, and nothing about their order.

In the second step, we need to figure out if we are looking for points within a specific circle, or if we want to find more distant points on the square. Personally, I recommend cycling through this list of cadidates to calculate the distance from the starting point for each result, and ordering the results by the calculated distance.

In the third step, you can use a simple filter to remove locations that are more than the desired distance from the defined circle.
