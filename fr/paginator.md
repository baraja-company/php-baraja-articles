Paginateur et pagination des résultats en PHP
=============================================

> id: a1450160-e320-414a-8266-128465632e94
> slug:
> 	cs: paginator
> 	fr: paginateur-et-pagination-des-resultats-en-php
> 
> perex: Stránkování dlouhého seznamu položek. Jak řešit omezení počtu vypsaných položek a vypočítat stránkování.
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: d0bc71ded032401875396b0fd263a820

Lorsque nous avons beaucoup de données à déverser, il est poli de les diviser en plusieurs pages. Cet article n'aborde pas la mise en œuvre pratique de la transmission des numéros de page et de l'énumération des résultats, mais uniquement l'extraction théorique des valeurs et le calcul du livre de codes optimal pour rendre la navigation dans un grand nombre de pages aussi conviviale que possible.

Combien de résultats avons-nous
----------------------

Pour commencer, nous devons savoir combien de résultats nous avons. Si les données proviennent d'une base de données, elles peuvent être comptées très efficacement avec l'instruction SQL suivante :

```sql
SELECT COUNT(*) FROM tabulka
```

Le calcul est très rapide car la base de données conserve les statistiques dans un fichier d'aide, de sorte qu'elle ne touche pas du tout aux données.

Si les données proviennent d'un autre endroit (et que nous les avons dans un tableau, par exemple), elles peuvent être comptées avec la fonction count() :

```php
$cisla = [3, 1, 4, 1, 5, 9, 2];

echo 'Le champ contient' . count($cisla) . 'numéros.';
```

Limiter le nombre de résultats
----------------------

Un autre problème est la limitation du nombre de résultats. Si les données sont dans la base de données, il suffit de mettre le paramètre `LIMIT` dans l'instruction SQL :

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10
```

Cette commande permettra toujours d'obtenir un maximum de 10 résultats, et rendra également la requête plus rapide car la base de données n'aura pas à parcourir l'ensemble des fichiers de données.

Si nous avons des données provenant d'une autre source (encore une fois un tableau), nous pouvons également limiter les résultats au niveau de PHP en utilisant la variable d'aide `$iterator` :

```php
$pole = [...];

$iterator = 0;
$limit = 10;
foreach ($pole as $prvek) {
	// c'est ici que les données sont vidées

	$iterator++;
	if ($iterator >= $limit) {
	    break; // Arrête le cycle lorsqu'il a été exécuté 10 fois.
	}
}
```

Sauter les X premiers résultats
----------------------

Lorsque nous sommes sur la première page, c'est assez simple, il suffit de limiter le nombre de résultats en utilisant `LIMIT`. Mais que faire si je suis sur la troisième page ? Alors nous devons sauter les premiers résultats `X`.

En SQL, nous avons une notation élégante pour cela aussi :

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10 OFFSET 20
```

Il saute les 20 premiers résultats et limite la sortie suivante à 10 résultats, de sorte qu'il sort l'intervalle `<21 - 30>`.

En PHP pur, cela est géré de deux façons.

Si nous connaissons les index du tableau, nous pouvons commencer à le lire à partir d'un certain point (ce qui est très rapide) :

```php
$pole = [...];

$start = 20;
$limit = 10;
for ($i = $start; ($i <= $start + $limit && isset($pole[$i])); $i++) {
	// c'est ici que les données sont vidées
}
```

Cependant, pour un champ inconnu, nous devons utiliser à nouveau l'itérateur et sauter les éléments :

```php
$pole = [...];

$iterator = 0;
$start = 20;
$limit = 10;
foreach ($pole as $prvek) {
	if ($iterator < $start) {
		$iterator++;
		continue;
	}

	// d'une manière ou d'une autre, les données sont transférées ici.

	$iterator++;
	if ($iterator >= $start + $limit) break;
}
```

Affichage du paginateur/stepper optimal
----------------------

Supposons que nous connaissions le nombre total d'éléments, le nombre d'éléments sur la page et le numéro de la page actuelle. Nous voulons maintenant rendre une barre qui permettra une navigation rapide de toutes les pages avec des résultats de recherche. Cependant, comme il y a beaucoup de pages (de l'ordre de milliers), nous ne pouvons pas toutes les lister en même temps, nous devons donc choisir intelligemment quelques pages représentatives qui représentent le mieux l'intervalle entre les pages.

Cela peut ressembler à ceci :

```php
1 | 15 | 30 | 36 | 45 | 60 | 72
```

Affectation :

Je suis à la page 36 sur 72, comment placer les numéros de page de manière optimale ?
Eh bien, à travers la séquence.

> **Tip:** Par observation pratique, j'ai découvert que la partie gauche du Paginator doit être calculée par une séquence arithmétique (ce qui me permet de me déplacer linéairement du même nombre de pas) et la partie droite par une **séquence géométrique**, ce qui permet de faire facilement un grand pas. Ainsi, si je veux accéder à une page particulière, je commence par sauter un grand nombre d'éléments inutiles, puis j'affine la sélection en revenant sur la gauche.

Théorie des suites arithmétiques (on continue à ajouter le même nombre) :

```php
$d = 10;   // taille du pas
$a[1] = 1; // premier élément
$a[2] = $a[1] + $d; // deuxième élément
$a[3] = $a[1] + 2 * $d;
$a[3] = $a[2] + $d;
$a[$n] = $a[1] + ($n - 1) * $d; // nième élément

function getAritmeticItem(int $start, int $step, int $n): int
{
	return $start + ($n - 1) * $step;
}
```

Théorie des suites géométriques (toujours multiplier par le même nombre) :

```php
$q = 10;   // taille du pas
$a[1] = 1; // premier élément
$a[2] = $a[1] * $q; // deuxième élément
$a[3] = $a[1] * $q * $q;
$a[3] = $a[1] * pow($q, 2);
$a[3] = $a[2] * $q;
$a[$n] = $a[1] * pow($q, $n - 1); // nième élément

function getGeometricItem(int $start, int $step, int $q): int
{
	return $start * pow($q, $step - 1);
}
```



```php
$start = 1;
$current = 36;
$end = 72;
```
