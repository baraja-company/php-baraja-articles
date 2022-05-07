Sammanslagning av stora matriser i PHP
======================================

> id: ccfb3f47-c8a0-4d0f-aa29-18d4c1775725
> slug:
> 	cs: mergovani-velkeho-pole
> 	sv: sammanslagning-av-stora-matriser-i-php
> 
> publicationDate: '2020-02-06 09:32:08'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3bfc3b53d26a4c9b675e851a643fc71a'

Ofta behöver vi slå ihop flera matriser, och detta kan göras på ett elegant sätt med funktionen `array_merge`:

```php
$userIdsA = [1, 2, 3];
$userIdsB = [5, 6, 7];

// återger [1, 2, 3, 5, 6, 7].
$finalIds = array_merge($userIdsA, $userIdsB);
```

Funktionen `array_merge` slår ihop två matriser till en stor matris. Om det sker en nyckelkollision vinner värdet i den högra matrisen.

Upprepad sammanslagning i en slinga
---------------------------

Ofta får vi dock en matris av matriser som endast skapas i en slinga (t.ex. från en databas som sedan skickas genom foreach), och därför vet vi inte antalet sammanslagningar i förväg.

En naiv lösning kan se ut så här:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds = array_merge($finalIds, $user->someIds);
}
```

Den här lösningen är dock mycket ineffektiv eftersom vi måste slå ihop matriserna vid varje iteration och iterera över hela den stora matrisen.

Det finns dock en enkel lösning där vi ändrar sammanslagningsalgoritmen så att den bara går igenom uppgifterna en gång:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds[] = $user->someIds;
}
```

I det här fallet kommer fältet `$finalIds` att generera lite mer data, men det är fortfarande ett mindre problem än den tidsbesparande fördelen.

Själva sammanslagningen varierar beroende på vilken version av PHP du använder och löses med ett elegant trick:

```php
/* PHP 5.6 och äldre */
$finalIds = call_user_func_array('array_merge', $finalIds + [[]]);

/* PHP 5.6+ och senare */
$finalIds = array_merge([], ...$finalIds);

/* PHP 7.4+ och senare för icke-tomma fält */
$finalIds = array_merge(...$finalIds);
```

Särskilt lösningen `array_merge(...$finalIds)` ser mycket intressant ut, eftersom den använder ett nytt PHP 7-koncept där du kan skicka ett dynamiskt antal argument till en funktion med hjälp av trippeltecknet i början. Sammanfogningsprocessen blir då så effektiv som möjligt och hela logiken hanteras automatiskt av PHP internt.

Den korta notationen `array_merge(...$finalIds)` kan endast användas för icke-tomma matriser. Om det är en tom array skickas inget argument till funktionen och funktionen kastar felet `Function array_merge invoked with 0 parameters, at least 1 required.`.
