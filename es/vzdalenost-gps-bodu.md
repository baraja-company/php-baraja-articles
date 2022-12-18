Distancia entre dos puntos GPS
==============================

> id: '9e67de8f-922a-4f96-886a-07f0002b66ad'
> slug:
> 	cs: vzdalenost-gps-bodu
> 	es: distancia-entre-dos-puntos-gps
> 
> publicationDate: '2022-12-18 11:00:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: d466d432cddc147949c75ef28fbaf503

El algoritmo puede calcular fácilmente la distancia aproximada en línea recta entre dos puntos GPS:

Implementación de PHP
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

Esta función presupone que la Tierra es una esfera perfecta, por lo que el resultado debe utilizarse sólo como estimación. Para que el cálculo de la distancia sea realista, hay que tener en cuenta la altitud, la forma del terreno y el aplanamiento local.

Utilizo esta función para estimar la distancia de los puntos de venta de las tiendas electrónicas. En la República Checa, la desviación media es de aproximadamente 5 metros por cada 1 km, lo que es suficiente en la operación real.

Implementación en TypeScript
--------------------------

Puede que necesites hacer el cálculo en el cliente, ahí es donde javascript es útil:

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

Cómo buscar lugares cercanos en la base de datos
------------------------------------

Para encontrar las ubicaciones circundantes en la base de datos alrededor de un punto específico, puede utilizar los costosos cálculos de la función indicada anteriormente directamente en SQL, o puede hacerlo de forma inteligente.

Personalmente, utilizo una combinación de un par de algoritmos para encontrar ubicaciones cercanas (por ejemplo, sucursales para pasar por caja de una tienda electrónica a un cliente).

En el primer paso, basta con cargar un área cuadrada alrededor del punto seleccionado (se obtiene simplemente restando un valor constante al punto deseado y buscando en el intervalo). Esto nos da una lista de resultados candidatos, pero aún no sabemos exactamente a qué distancia están, ni nada sobre su orden.

En el segundo paso, tenemos que averiguar si estamos buscando puntos dentro de un círculo específico, o si queremos encontrar puntos más distantes en el cuadrado. Personalmente, recomiendo recorrer en bicicleta esta lista de cadidatos para calcular la distancia desde el punto de partida para cada resultado, y ordenar los resultados por la distancia calculada.

En el tercer paso, puede utilizar un filtro sencillo para eliminar las ubicaciones que se encuentren a una distancia superior a la deseada del círculo definido.
