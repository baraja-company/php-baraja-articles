Odległość między dwoma punktami GPS
===================================

> id: '9e67de8f-922a-4f96-886a-07f0002b66ad'
> slug:
> 	cs: vzdalenost-gps-bodu
> 	pl: odleglosc-miedzy-dwoma-punktami-gps
> 
> publicationDate: '2022-12-18 11:00:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: d466d432cddc147949c75ef28fbaf503

Przybliżona odległość dwóch punktów GPS w linii prostej może być łatwo obliczona przez algorytm:

Implementacja PHP
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

Ta funkcja zakłada, że Ziemia jest idealną kulą, więc użyj wyjścia tylko jako oszacowania. Dla realistycznego obliczenia odległości trzeba jeszcze uwzględnić wysokość, ukształtowanie terenu i lokalne spłaszczenia.

Funkcji tej używam do szacowania odległości punktów sprzedaży e-sklepów. W Czechach średnie odchylenie wynosi około 5 metrów na każdy 1 km, co jest wystarczające w realnej eksploatacji.

Implementacja w TypeScript
--------------------------

Być może będziesz musiał wykonać obliczenia na kliencie, to właśnie tam przydaje się javascript:

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

Jak wyszukać pobliskie miejsca w bazie danych
------------------------------------

Aby znaleźć okoliczne miejsca w bazie danych wokół określonego punktu, możesz albo użyć drogich obliczeń funkcji wymienionych powyżej bezpośrednio w SQL, albo możesz zrobić to sprytnie.

Osobiście używam kombinacji pary algorytmów, aby znaleźć pobliskie lokalizacje (na przykład oddziały do kasy ze sklepu internetowego do klienta).

W pierwszym kroku po prostu ładujesz kwadratowy obszar wokół wybranego punktu (otrzymujesz to, po prostu odejmując stałą wartość od żądanego punktu, a następnie przeszukując interwał). To daje nam listę kandydatów na wyniki, ale nie wiemy jeszcze dokładnie, jak daleko od siebie są, i nic o ich kolejności.

W drugim kroku musimy ustalić, czy szukamy punktów w obrębie danego okręgu, czy chcemy znaleźć bardziej odległe punkty na kwadracie. Osobiście polecam przejechanie na rowerze przez tę listę kadydatów, aby obliczyć odległość od punktu startowego dla każdego wyniku i uporządkować wyniki według obliczonej odległości.

W trzecim kroku możesz użyć prostego filtra, aby usunąć lokalizacje, które są bardziej niż pożądana odległość od zdefiniowanego okręgu.
