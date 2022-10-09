Sammenlægning af store arrays i PHP
===================================

> id: ccfb3f47-c8a0-4d0f-aa29-18d4c1775725
> slug:
> 	cs: mergovani-velkeho-pole
> 	da: sammenlaegning-af-store-arrays-i-php
> 
> publicationDate: '2020-02-06 09:32:08'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3bfc3b53d26a4c9b675e851a643fc71a'

Ofte har vi brug for at flette flere arrays sammen, og dette kan gøres meget elegant med funktionen `array_merge`:

```php
$userIdsA = [1, 2, 3];
$userIdsB = [5, 6, 7];

// returnerer [1, 2, 3, 5, 6, 6, 7]
$finalIds = array_merge($userIdsA, $userIdsB);
```

Funktionen `array_merge` samler to arrays til ét stort array. Hvis der er en nøglekollision, vinder værdien i det mest højre array.

Gentagen sammenlægning i en sløjfe
---------------------------

Vi får dog ofte et array af arrays, der kun oprettes i en løkke (f.eks. fra en database og derefter sendes gennem foreach), og derfor kender vi ikke antallet af sammenlægninger på forhånd.

En naiv løsning kunne se således ud:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds = array_merge($finalIds, $user->someIds);
}
```

Denne løsning er imidlertid meget ineffektiv for CPU'en, fordi vi skal flette arrays sammen ved hver gentagelse og gentage over hele det store array.

Der er dog en enkel løsning, hvor vi ændrer sammenlægningsalgoritmen, så den kun gennemgår dataene én gang:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds[] = $user->someIds;
}
```

I dette tilfælde vil feltet `$finalIds` generere lidt flere data, men det er stadig et mindre problem end den tidsbesparende fordel.

Selve sammenlægningen varierer afhængigt af den version af PHP, du bruger, og løses med et elegant trick:

```php
/* PHP 5.6 og ældre */
$finalIds = call_user_func_array('array_merge', $finalIds + [[]]);

/* PHP 5.6+ og nyere */
$finalIds = array_merge([], ...$finalIds);

/* PHP 7.4+ og senere for ikke-tomme felter */
$finalIds = array_merge(...$finalIds);
```

Især løsningen `array_merge(...$finalIds)` ser meget interessant ud, fordi den bruger et nyt PHP 7-koncept, hvor du kan sende et dynamisk antal argumenter til en funktion ved hjælp af triplet-tegnet i begyndelsen. Sammenlægningsprocessen er så effektiv som muligt, og hele logikken håndteres automatisk af PHP internt.

Den korte notation `array_merge(...$finalIds)` kan kun bruges for ikke-tomme arrays. Hvis det er et tomt array, overføres der ikke noget argument til funktionen, og funktionen kaster fejlen `Function array_merge invoked with 0 parameters, at least 1 required.`.
