Conditions et ramification
==========================

> id: '2cea5541-6879-4763-a518-cb21bf9021dd'
> slug:
> 	cs: podminky
> 	fr: conditions-et-ramification
> 
> publicationDate: '2019-09-07 20:25:57'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: bc4140fc49413be3f2f2b2264b8ea5e6

**Avertissement : ** Cet article a été écrit il y a plusieurs années et certaines informations peuvent être dépassées ou incorrectes. Veuillez garder cela à l'esprit lors de votre lecture.

Finis les programmes linéaires ! Le principe le plus fondamental de tout programme est "ce qui se passe quand....". Une condition peut être écrite sous la forme d'une instruction logique, qui peut être vraie (la condition est satisfaite) ou fausse (elle n'est alors pas exécutée ou son contraire exact est exécuté). Les deux sont faciles à définir.

Notation générale
------------

En général, une condition peut être écrite comme une déclaration logique. La condition peut être satisfaite ou non. C'est une bonne idée de compter les deux options possibles. S'il existe plusieurs alternatives, on parle de **condition imbriquée**.

Exemple :

```php
if (hodnota   operace   hodnota) {
	// Cette opération est déclenchée si la condition est remplie
} else {
	// Ceci est déclenché si la condition ne s'applique pas.
}
```

Nous ne devons pas toujours définir les deux options (parfois, c'est complètement inutile). En fait, nous pouvons définir la situation si seule la condition est respectée. Cette opération s'effectue comme suit :

```php
if (hodnota   operace   hodnota) {
	// Cette opération est déclenchée si la condition est remplie
}
```

Opérateurs logiques
--------------------------

| Opérateur | Signification
|----------|---------
| `==` | Égaux
| `===` | Egale et a le même type de données (*tout peut être comparé à tout, mais la condition n'est satisfaite que s'il s'agit d'une valeur du même type de données (par exemple, un nombre, un texte, ...)*)
| `!=` | N'est pas égal à lui-même
| `<=` | Égal ou supérieur à
| `>=` | Egal à ou inférieur à
| `<` | Plus grand
| `>` | Less

Exemple concret
--------------------------

```php
$a = 5;
$b = 3;
if ($a === $b) {
	// bloc qui est imprimé si $a est égal à $b
} else {
	// bloc qui est imprimé si $a n'est PAS égal à $b
}
```

Conditions imbriquées
--------------------------

Malheureusement, la sortie est seulement `true` (valide) et `false` (invalide). Ainsi, si nous voulons envisager plusieurs possibilités, nous devons imbriquer plusieurs conditions les unes dans les autres. C'est ce qu'on appelle une **condition imbriquée**. Elle est imbriquée parce que l'une des solutions à la condition est juste une autre condition.

```php
$a = 5;         // poche gauche
$b = 3;         // poche droite
$kapsa = true;  // J'ai une poche ?

if ($kapsa === true) {

	if ($a > $b) {
		echo 'Dans la poche gauche, il y a plus';
	} else {
		echo 'Dans la poche droite, il y a plus';
	}

} else {
	echo 'Vous n'avez pas de poche';
}
```
