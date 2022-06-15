Fusión de matrices grandes en PHP
=================================

> id: ccfb3f47-c8a0-4d0f-aa29-18d4c1775725
> slug:
> 	cs: mergovani-velkeho-pole
> 	es: fusion-de-matrices-grandes-en-php
> 
> publicationDate: '2020-02-06 09:32:08'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3bfc3b53d26a4c9b675e851a643fc71a'

A menudo necesitamos fusionar varios arrays juntos, esto se puede hacer de forma muy elegante con la función `array_merge`:

```php
$userIdsA = [1, 2, 3];
$userIdsB = [5, 6, 7];

// devuelve [1, 2, 3, 5, 6, 7]
$finalIds = array_merge($userIdsA, $userIdsB);
```

La función `array_merge` fusiona dos arrays en uno grande. Si hay una colisión de teclas, gana el valor de la matriz más a la derecha.

Fusión repetida en un bucle
---------------------------

Sin embargo, a menudo obtenemos una matriz de matrices que se crea sólo en un bucle (por ejemplo, a partir de una base de datos y luego se pasa por foreach), por lo que no sabemos el número de fusiones de antemano.

Una solución ingenua podría ser la siguiente:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds = array_merge($finalIds, $user->someIds);
}
```

Sin embargo, esta solución es muy ineficiente para la CPU porque tenemos que fusionar las matrices en cada iteración e iterar sobre toda la matriz grande.

Sin embargo, hay una solución sencilla en la que modificamos el algoritmo de fusión para que sólo pase por los datos una vez:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds[] = $user->someIds;
}
```

En este caso, el campo `$finalIds` generará un poco más de datos, pero esto sigue siendo menos problemático que la ventaja de ahorrar tiempo.

La fusión en sí varía dependiendo de la versión de PHP que estés utilizando, y se resuelve con un elegante truco:

```php
/* PHP 5.6 y anteriores */
$finalIds = call_user_func_array('array_merge', $finalIds + [[]]);

/* PHP 5.6+ y posterior */
$finalIds = array_merge([], ...$finalIds);

/* PHP 7.4+ y posterior para campos no vacíos */
$finalIds = array_merge(...$finalIds);
```

En particular, la solución `array_merge(...$finalIds)` parece muy interesante, porque utiliza un nuevo concepto de PHP 7 en el que se puede pasar un número dinámico de argumentos a una función utilizando el carácter triplete al principio. El proceso de fusión es entonces lo más eficiente posible y toda la lógica es manejada automáticamente por PHP internamente.

La notación abreviada `array_merge(...$finalIds)` sólo puede utilizarse para matrices no vacías. Si es un array vacío, no se pasa ningún argumento a la función y ésta lanza un error `Function array_merge invoked with 0 parameters, at least 1 required.`.
