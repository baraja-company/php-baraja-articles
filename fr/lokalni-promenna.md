Variables locales en PHP
========================

> id: '9c3fbf4e-8612-4599-8127-d66b9ec5e980'
> slug:
> 	cs: lokalni-promenna
> 	fr: variables-locales-en-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: cffee22c4dbcc2ec331b73235349c409

Les variables locales ne sont valables que dans le corps d'une **<a href="/prikazy-a-function">fonction</a>** ou d'une **méthode** (en programmation orientée objet).

Si nous travaillons dans le contexte d'un script ordinaire, tout se passe comme prévu :

```php
$x = 5;

echo $x; // imprime 5
```

Mais lorsque nous définissons une fonction personnalisée, le comportement change légèrement :

```php
$x = 5;

function mojeFunkce(): int
{
    $x = 3;

    echo $x; // imprime 3
}

echo $x;     // imprime 5
```

La raison en est que la variable $x n'existe que dans le contexte de la fonction ou de la méthode en cours. Ce comportement est intentionnel.

Si nous voulons transmettre une valeur du code environnant à une fonction, nous devons l'appeler avec les paramètres nécessaires :

```php
echo mojeFunkce(5);	// imprime 6

function mojeFunkce(int $x): int
{
    return $x + 1;
}
```

Les valeurs sont saisies dans les fonctions à l'aide de paramètres. Si vous voulez passer des variables supplémentaires dans la fonction au-delà des paramètres, vous pouvez utiliser <a href="/global-variable">variables globales</a>, mais ce n'est pas une bonne idée.

> L'utilisation de variables locales fait une énorme différence lors de la programmation d'une application plus importante. Si nous ne distinguions pas la validité des variables selon les contextes, il serait impossible de garantir qu'une variable ne sera pas surchargée à un endroit où nous ne comptons pas dessus (c'est pourquoi les variables globales sont diaboliques).

```php
$x = 5;
$y = 3;

function soucet(int $x, int $y): int
{
    return $x + $y;
}

echo $x;             // imprime 5
echo soucet($x, $y); // imprime 8
```
