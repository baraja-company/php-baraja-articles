Variables globales en PHP
=========================

> id: '1cdfc51a-31f4-4a5d-81f6-cfd92f86a9d4'
> slug:
> 	cs: globalni-promenna
> 	fr: variables-globales-en-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '31e78a9abf19252ef62315230a3731b5'

Les variables globales sont disponibles à tout moment dans n'importe quelle partie de l'application et n'ont pas besoin d'être transmises.

> Une application bien conçue ne devrait pas utiliser de variables globales car elles violent le principe d'encapsulation et peuvent provoquer des erreurs difficiles à détecter si elles sont manipulées sans précaution.

Exemple d'utilisation :

```php
$a = 1;
$b = 2;

function suma(): void
{
	global $a, $b;
	$b = $a + $b;
}

suma();

echo $b; // imprime le nombre 3 car la variable $b est globale
```

Notez que nous avons obtenu les variables `$a` et `$b` en dehors de leur contexte naturel. Ce comportement est qualifié de "magique" car si une autre fonction remplace les variables en cours d'utilisation, l'application connaîtra une condition inattendue.

Correctement, l'application devrait **encapsuler** et passer les variables à chaque fois :

```php
$a = 1;
$b = 2;

function suma(int $a, int $b): int
{
	return $a + $b;
}

echo suma($a, $b); // imprime 3
```

Grâce à cela, nous pouvons appeler la fonction de manière dynamique avec différents paramètres d'entrée et sa sortie ne dépendra que des entrées, et non de l'environnement.

Récupération des paramètres d'entrée à partir de l'URL
---------------------------------

Peut-être que la seule utilisation raisonnable des variables globales est l'analyse syntaxique des entrées utilisateur, auquel cas nous parlons de <a href="/superglobal-variable">variablessuperglobales</a>.

Dans ce cas, il s'agit d'une conception propre car la variable doit être en lecture seule, et non en écriture seule, et de plus, elle est la même dans toute l'application :

```php
function getNameFromUrl(): string
{
    return isset($_GET['nom'])
    	? htmlspecialchars($_GET['nom'])
    	: '';
}

echo getNameFromUrl();
```
