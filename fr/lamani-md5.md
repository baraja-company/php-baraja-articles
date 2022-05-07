Comment casser la fonction md5
==============================

> id: '9e678fcc-3d5e-46a3-9c3f-c1eb3d1ad367'
> slug:
> 	cs: lamani-md5
> 	fr: comment-casser-la-fonction-md5
> 
> publicationDate: '2019-08-23 15:33:10'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '4e176a09e43dbf12e131103be6a9cf4f'

MD5 est une fonction très couramment utilisée pour calculer des hachages.

Les débutants l'utilisent souvent pour <a href="/hashovani">hachage de mot de passe</a>, ce qui n'est pas une bonne idée car il existe de nombreux moyens de retrouver le mot de passe original.

Cet article décrit des méthodes spécifiques pour y parvenir.

Complexité du temps
----------------

Toute sécurité repose sur le fait qu'il faut un temps disproportionné pour essayer tous les mots de passe. Eh bien, ça devrait. Le problème avec l'algorithme `md5()` en particulier est que c'est une fonction très rapide. Sur un ordinateur normal, il n'est pas difficile de calculer plus d'un million de hachages par seconde.

Si nous cassons le mot de passe en essayant les combinaisons une par une, il s'agit d'une **attaque par force brute**.

Méthodes de fissuration
----------------

Il existe plusieurs stratégies :

- Essais successifs par essais et erreurs (attaque par force brute)
- Test des mots de passe dictionnaires
- Rainbow tables (base de données de hachage précalculé)
- Recherches sur Google
- Collisions dans l'algorithme

Il existe de nombreuses autres méthodes, cet article ne décrit que les plus courantes.

Stratégies de rupture par force brute
-----------------------------

Toutes les combinaisons de lettres, de chiffres et d'autres caractères sont essayées une par une.

Les tentatives générées sont hachées une par une et comparées au hachage original.

Donc, par exemple :

```php
aaaaaaabaaacaaadaaaeaaaf...
```

Le problème de cette attaque se trouve dans l'algorithme `md5()` lui-même, si nous devions essayer uniquement les lettres minuscules de l'alphabet anglais et les chiffres, il faudrait au plus quelques dizaines de minutes pour essayer toutes les combinaisons sur un ordinateur communément disponible.

Il est donc important de choisir des mots de passe longs, de préférence aléatoires et comportant des caractères spéciaux.

Stratégie d'attaque par dictionnaire
----------------------------

Les gens choisissent généralement des mots de passe faibles qui existent dans le dictionnaire.

Si nous tirons parti de ce fait, nous pouvons rapidement écarter les variantes improbables telles que `6w1SCq5cs` et deviner à la place des mots existants.

En outre, nous savons, grâce aux précédentes fuites de mots de passe de grandes entreprises, que les utilisateurs choisissent une majuscule au début du mot de passe et un chiffre à la fin. Voyons voir - est-ce que votre mot de passe a aussi cela ? :)

Tables arc-en-ciel - base de données précalculée
--------------------------------------

Comme un mot de passe correspond toujours au même hachage, il est facile de recalculer une énorme base de données dans laquelle les mots de passe seront recherchés en premier.

En fait, la recherche est toujours plus rapide de plusieurs ordres de grandeur que la recherche répétée de hachages.

En outre, pour les fuites de données plus importantes, les mots de passe peuvent être hachés en parallèle de cette manière et, par exemple, 10 % de tous les mots de passe des utilisateurs peuvent être rapidement récupérés.

Une bonne base de données de mots de passe est par exemple <a href="https://crackstation.net/">Crack Station</a>.

Recherche sur Google
-------------------

De nombreux mots de passe simples sont connus directement par Google, car il indexe les pages qui contiennent des hachages.

J'utilise toujours Google comme première option. :)

Trouver des collisions dans l'algorithme
--------------------------

Le <a href="https://cs.wikipedia.org/wiki/Dirichlet%C5%AFv_princip">Principe de Dirichlet</a> décrit que si nous avons un ensemble de hachages qui font toujours 32 caractères, alors il y a au moins 2 mots de passe différents d'une longueur de 33 caractères (un plus long) qui génèrent le même hachage.

En pratique, cela n'a pas de sens de chercher les collisions, mais parfois l'auteur de l'application lui-même facilite la recherche en recalculant les collisions.

Par exemple :

```php
$password = 'mot de passe';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x haché via md5()
```

Dans ce cas, il est logique de deviner la collision plutôt que le hachage original.

Santé à l'arrosage !
