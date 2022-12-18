Entfernung zwischen zwei GPS-Punkten
====================================

> id: '9e67de8f-922a-4f96-886a-07f0002b66ad'
> slug:
> 	cs: vzdalenost-gps-bodu
> 	de: entfernung-zwischen-zwei-gps-punkten
> 
> publicationDate: '2022-12-18 11:00:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: d466d432cddc147949c75ef28fbaf503

Die ungefähre Entfernung zweier GPS-Punkte in Luftlinie lässt sich mit dem Algorithmus leicht berechnen:

PHP-Implementierung
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

Diese Funktion geht davon aus, dass die Erde eine perfekte Kugel ist; verwenden Sie die Ausgabe daher nur als Schätzung. Für eine realistische Entfernungsberechnung müssen Sie noch die Höhe, die Form des Geländes und die lokale Abflachung berücksichtigen.

Ich verwende diese Funktion, um die Entfernung von E-Shop-Filialen zu schätzen. In der Tschechischen Republik beträgt die durchschnittliche Abweichung etwa 5 Meter pro 1 km, was im realen Betrieb ausreichend ist.

Implementierung in TypeScript
--------------------------

Möglicherweise müssen Sie die Berechnung auf dem Client durchführen, was mit Javascript möglich ist:

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

So suchen Sie in der Datenbank nach Orten in Ihrer Nähe
------------------------------------

Um die umliegenden Orte in der Datenbank um einen bestimmten Punkt herum zu finden, können Sie entweder die teuren Berechnungen der oben genannten Funktion direkt in SQL verwenden oder Sie können es auf intelligente Weise tun.

Ich persönlich verwende eine Kombination aus zwei Algorithmen, um Standorte in der Nähe zu finden (z. B. Filialen, die ein Kunde von einem E-Store zur Kasse bringen kann).

Im ersten Schritt laden Sie einfach einen quadratischen Bereich um den ausgewählten Punkt (den Sie erhalten, indem Sie einfach einen konstanten Wert vom gewünschten Punkt abziehen und dann das Intervall suchen). So erhalten wir eine Liste von möglichen Ergebnissen, aber wir wissen noch nicht genau, wie weit sie voneinander entfernt sind, und auch nichts über ihre Reihenfolge.

Im zweiten Schritt müssen wir herausfinden, ob wir nach Punkten innerhalb eines bestimmten Kreises suchen, oder ob wir weiter entfernte Punkte auf dem Quadrat finden wollen. Ich persönlich empfehle, die Liste der Kadidaten durchzugehen, um die Entfernung vom Startpunkt für jedes Ergebnis zu berechnen und die Ergebnisse nach der berechneten Entfernung zu ordnen.

Im dritten Schritt können Sie einen einfachen Filter verwenden, um Orte zu entfernen, die mehr als den gewünschten Abstand zum definierten Kreis haben.
