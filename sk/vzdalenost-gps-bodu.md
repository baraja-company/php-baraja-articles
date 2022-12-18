Vzdialenosť medzi dvoma bodmi GPS
=================================

> id: '9e67de8f-922a-4f96-886a-07f0002b66ad'
> slug:
> 	cs: vzdalenost-gps-bodu
> 	sk: vzdialenost-medzi-dvoma-bodmi-gps
> 
> publicationDate: '2022-12-18 11:00:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: d466d432cddc147949c75ef28fbaf503

Približnú vzdialenosť dvoch bodov GPS vzdušnou čiarou možno ľahko vypočítať pomocou algoritmu:

Implementácia PHP
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

Táto funkcia predpokladá, že Zem je dokonalá guľa, preto jej výsledok používajte len ako odhad. Pre realistický výpočet vzdialenosti je ešte potrebné zohľadniť nadmorskú výšku, tvar terénu a miestne zrovnanie.

Túto funkciu používam na odhad vzdialenosti predajní e-shopu. V Českej republike je priemerná odchýlka približne 5 metrov na každý 1 km, čo je v reálnej prevádzke dostatočné.

Implementácia v jazyku TypeScript
--------------------------

Výpočet možno budete musieť vykonať na klientovi, na to sa hodí javascript:

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

Ako vyhľadať blízke miesta v databáze
------------------------------------

Ak chcete nájsť okolité miesta v databáze v okolí konkrétneho bodu, môžete buď použiť nákladné výpočty vyššie uvedenej funkcie priamo v jazyku SQL, alebo to môžete urobiť inteligentne.

Osobne používam kombináciu dvojice algoritmov na vyhľadávanie blízkych miest (napríklad pobočiek na pokladni e-shopu pre zákazníka).

V prvom kroku stačí načítať štvorcovú oblasť okolo vybraného bodu (získate ju jednoduchým odpočítaním konštantnej hodnoty od požadovaného bodu a následným vyhľadaním intervalu). Získali sme tak zoznam možných výsledkov, ale zatiaľ nevieme, ako presne sú od seba vzdialené, a ani nič o ich poradí.

V druhom kroku musíme zistiť, či hľadáme body v rámci konkrétneho kruhu, alebo chceme nájsť vzdialenejšie body na štvorci. Osobne odporúčam prejsť tento zoznam kadidátov a vypočítať vzdialenosť od východiskového bodu pre každý výsledok a zoradiť výsledky podľa vypočítanej vzdialenosti.

V treťom kroku môžete pomocou jednoduchého filtra odstrániť miesta, ktoré sú od definovaného kruhu vzdialené viac ako požadovaná vzdialenosť.
