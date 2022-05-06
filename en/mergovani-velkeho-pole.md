Merging large arrays in PHP
===========================

> id: ccfb3f47-c8a0-4d0f-aa29-18d4c1775725
> slug:
> 	cs: mergovani-velkeho-pole
> 	en: merging-large-arrays-in-php
> 
> publicationDate: '2020-02-06 09:32:08'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3bfc3b53d26a4c9b675e851a643fc71a'

Often we need to merge multiple arrays together, this can be done very elegantly with the `array_merge` function:

```php
$userIdsA = [1, 2, 3];
$userIdsB = [5, 6, 7];

// returns [1, 2, 3, 5, 6, 7]
$finalIds = array_merge($userIdsA, $userIdsB);
```

The `array_merge` function combines two arrays into one large array. If there is a collision in the keys, the value of the rightmost array wins.

Repeated merging in a loop
---------------------------

However, we often get an array of arrays that is created only in a loop (for example, from a database and then passed through foreach), and so we don't know the number of merges in advance.

A naive solution might look like this:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds = array_merge($finalIds, $user->someIds);
}
```

However, this solution is very CPU inefficient because we have to merge the arrays together with each iteration and iterate over the entire large array.

However, there is a simple solution where we modify the merging algorithm to only go through the data once:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds[] = $user->someIds;
}
```

In this case, the `$finalIds` array will generate a bit more data, but it's still less of a problem than the time-saving benefit.

The merging itself varies depending on the version of PHP you are using and is solved with an elegant trick:

```php
/* PHP 5.6 and earlier */
$finalIds = call_user_func_array('array_merge', $finalIds + [[]]);

/* PHP 5.6+ and later */
$finalIds = array_merge([], ...$finalIds);

/* PHP 7.4+ and later for non-empty arrays */
$finalIds = array_merge(...$finalIds);
```

In particular, the `array_merge(...$finalIds)` solution looks very interesting, as it takes advantage of a new PHP 7 concept where you can pass a dynamic number of arguments to a function using the triplet character at the beginning. The merge process is then as efficient as possible and the whole logic is handled automatically by PHP internally.

The shorthand notation `array_merge(...$finalIds)` can only be used for non-empty arrays. If it is an empty array, no argument is passed to the function and the function throws a `Function array_merge invoked with 0 parameters, at least 1 required.` error.
