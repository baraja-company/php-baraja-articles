Vzdálenost dvou GPS bodů
========================

> id: 9e67de8f-922a-4f96-886a-07f0002b66ad
> slug:
> 	cs: vzdalenost-gps-bodu
> 
> publicationDate: "2022-12-18 11:00:00"
> mainCategoryId: "1f73dcfa-92a9-4738-ab30-8cbfb00ad23b"

Přibližnou zdálenost dvou GPS bodů vzdušnou čarou můžeme snadno vypočítat algoritmem:

Implementace v PHP
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

Tato funkce předpokládá, že je Země dokonalá koule, proto výstup používejte jen jako odhad. Pro reálný výpočet vzdálenosti ještě musíte zohlednit nadmořkou výšku, tvar terénu a lokální zploštění.

Tuto funkci používám pro odhadování vzdálenosti výdejních míst e-shopů. V České republice je průměrná odchylka přibližně 5 metrů na každém 1 km, což v reálném provozu stačí.

Implementace v TypeScriptu
--------------------------

Možná budete potřebovat provést výpočet na klientovi, tam se hodí javascript:

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

Jak vyhledat okolní místa v databázi
------------------------------------

Pro vyhledání okolních míst v databázi kolem konkrétního bodu můžete použít buď drahé výpočty funkcí uvedenou nahoře přímo v SQL, nebo to uděláte chytře.

Osobně pro vyhledání míst v okolí (například poboček pro výdej z e-shopu zákazníkovi) používám kombinaci dvojice algoritmů.

V prvním kroku stačí kolem zvoleného bodu načíst čtvercovou oblast (tu získáte prostým odečtením konstantní hodnoty od požadovaného bodu, a pak hledáním v intervalu). Tím získáme seznam kandidátů na výsledek, ale ještě nevíme, jak přesně jsou vzálené, a taky nic o jejich pořadí.

Druhém kroku potřebujeme zjistit, jestli hledáme body v konkrétním okruhu, nebo chceme najít vzdálenější místa na čtverci. Osobně doporučuji pro každý výsledek v tomto seznamu kadidátů cyklem spočítat vzdálenost od výchozího bodu, a podle vypočítané vzdálenosti výsledky seřadit.

Ve třetím kroku můžete jednoduchým filtrem odebrat místa, která jsou vzdálená od definovaného okruhu o víc, než požadovanou vzdálenost.
