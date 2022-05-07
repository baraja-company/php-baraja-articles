Fusionner de grands tableaux en PHP
===================================

> id: ccfb3f47-c8a0-4d0f-aa29-18d4c1775725
> slug:
> 	cs: mergovani-velkeho-pole
> 	fr: fusionner-de-grands-tableaux-en-php
> 
> publicationDate: '2020-02-06 09:32:08'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3bfc3b53d26a4c9b675e851a643fc71a'

Souvent, nous avons besoin de fusionner plusieurs tableaux ensemble, cela peut être fait de manière très élégante avec la fonction `array_merge` :

```php
$userIdsA = [1, 2, 3];
$userIdsB = [5, 6, 7];

// renvoie [1, 2, 3, 5, 6, 7].
$finalIds = array_merge($userIdsA, $userIdsB);
```

La fonction `array_merge` fusionne deux tableaux en un seul grand tableau. S'il y a une collision de clés, la valeur du tableau le plus à droite l'emporte.

Fusion répétée dans une boucle
---------------------------

Cependant, nous obtenons souvent un tableau de tableaux qui n'est créé que dans une boucle (par exemple, à partir d'une base de données et ensuite passé dans foreach), et donc nous ne connaissons pas le nombre de fusions à l'avance.

Une solution naïve pourrait ressembler à ceci :

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds = array_merge($finalIds, $user->someIds);
}
```

Cependant, cette solution est très inefficace au niveau du CPU car nous devons fusionner les tableaux à chaque itération et itérer sur l'ensemble du grand tableau.

Cependant, il existe une solution simple qui consiste à modifier l'algorithme de fusion pour ne parcourir les données qu'une seule fois :

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds[] = $user->someIds;
}
```

Dans ce cas, le champ `$finalIds` générera un peu plus de données, mais c'est toujours moins un problème qu'un gain de temps.

La fusion elle-même varie en fonction de la version de PHP que vous utilisez, et est résolue par une astuce élégante :

```php
/* PHP 5.6 et plus */
$finalIds = call_user_func_array('matrice_fusion', $finalIds + [[]]);

/* PHP 5.6+ et versions ultérieures */
$finalIds = array_merge([], ...$finalIds);

/* PHP 7.4+ et versions ultérieures pour les champs non vides */
$finalIds = array_merge(...$finalIds);
```

En particulier, la solution `array_merge(...$finalIds)` semble très intéressante, car elle utilise un nouveau concept de PHP 7 où vous pouvez passer un nombre dynamique d'arguments à une fonction en utilisant le caractère triplet au début. Le processus de fusion est alors aussi efficace que possible et toute la logique est gérée automatiquement par PHP en interne.

La notation abrégée `array_merge(...$finalIds)` ne peut être utilisée que pour des tableaux non vides. Si c'est un tableau vide, aucun argument n'est passé à la fonction et la fonction lève une erreur `Function array_merge invoquée avec 0 paramètres, au moins 1 requis.`.
