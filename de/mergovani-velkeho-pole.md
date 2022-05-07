Zusammenführen großer Arrays in PHP
===================================

> id: ccfb3f47-c8a0-4d0f-aa29-18d4c1775725
> slug:
> 	cs: mergovani-velkeho-pole
> 	de: zusammenfuehren-grosser-arrays-in-php
> 
> publicationDate: '2020-02-06 09:32:08'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3bfc3b53d26a4c9b675e851a643fc71a'

Oft müssen wir mehrere Arrays zusammenführen, dies kann sehr elegant mit der Funktion `array_merge` gemacht werden:

```php
$userIdsA = [1, 2, 3];
$userIdsB = [5, 6, 7];

// liefert [1, 2, 3, 5, 6, 7]
$finalIds = array_merge($userIdsA, $userIdsB);
```

Die Funktion `array_merge` verschmilzt zwei Arrays zu einem großen Array. Kommt es zu einer Schlüsselkollision, gewinnt der Wert des ganz rechts stehenden Feldes.

Wiederholtes Zusammenführen in einer Schleife
---------------------------

Allerdings erhalten wir oft ein Array von Arrays, das erst in einer Schleife erstellt wird (z. B. aus einer Datenbank und dann durch foreach geleitet wird), so dass wir die Anzahl der Zusammenführungen nicht im Voraus kennen.

Eine naive Lösung könnte wie folgt aussehen:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds = array_merge($finalIds, $user->someIds);
}
```

Diese Lösung ist jedoch sehr CPU-ineffizient, da wir die Arrays bei jeder Iteration zusammenführen und über das gesamte große Array iterieren müssen.

Es gibt jedoch eine einfache Lösung, bei der wir den Zusammenführungsalgorithmus so ändern, dass er die Daten nur einmal durchläuft:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds[] = $user->someIds;
}
```

In diesem Fall erzeugt das Feld "$finalIds" etwas mehr Daten, aber das ist immer noch weniger ein Problem als der Zeitvorteil.

Das Zusammenführen selbst variiert je nach PHP-Version, die Sie verwenden, und wird mit einem eleganten Trick gelöst:

```php
/* PHP 5.6 und älter */
$finalIds = call_user_func_array('array_merge', $finalIds + [[]]);

/* PHP 5.6+ und höher */
$finalIds = array_merge([], ...$finalIds);

/* PHP 7.4+ und höher für nicht leere Felder */
$finalIds = array_merge(...$finalIds);
```

Insbesondere die Lösung `array_merge(...$finalIds)` sieht sehr interessant aus, weil sie ein neues Konzept von PHP 7 verwendet, bei dem man eine dynamische Anzahl von Argumenten an eine Funktion übergeben kann, indem man das Triolenzeichen am Anfang verwendet. Der Zusammenführungsprozess ist dann so effizient wie möglich und die gesamte Logik wird intern von PHP automatisch gehandhabt.

Die Kurzschreibweise `array_merge(...$finalIds)` kann nur für nicht-leere Arrays verwendet werden. Wenn es sich um ein leeres Array handelt, wird kein Argument an die Funktion übergeben und die Funktion löst einen Fehler "Funktion array_merge mit 0 Parametern aufgerufen, mindestens 1 erforderlich" aus.
