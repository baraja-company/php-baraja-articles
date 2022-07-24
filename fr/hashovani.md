Hachage de chaînes de caractères et de mots de passe
====================================================

> id: '7978bee8-62cc-4770-b15b-a8d08d1dcf34'
> slug:
> 	cs: hashovani
> 	fr: hachage-de-chaines-de-caracteres-et-de-mots-de-passe
> 
> perex:
> 	- 'Hash není šifra! Metody hashování dat a hesel. MD5, SHA1, Bcrypt. Ověření hesla.'
> 	- 'Le hachage n''est pas un chiffrement ! Méthodes de hachage des données et des mots de passe. MD5, SHA1, Bcrypt. Vérification du mot de passe.'
> 
> publicationDate: '2019-09-11 10:13:30'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: f6ea0b06d6ace3c41684a49938f7ce8e

Le processus de hachage (par opposition au cryptage) produit une sortie à partir de l'entrée, à partir de laquelle la chaîne originale ne peut plus être dérivée.

Il est donc bien adapté à la protection des chaînes de caractères sensibles, des mots de passe et des sommes de contrôle.

Une autre caractéristique intéressante des fonctions de hachage est qu'elles génèrent toujours des sorties de la même longueur, et qu'une petite modification de l'entrée change toujours complètement la sortie.

Fonctions de hachage
----------------

Il existe de nombreuses fonctions de hachage en PHP, les plus importantes sont :

- **Bcrypt : password_hash()** - Le hachage de mot de passe le plus sûr, lent en termes de calcul, utilise un sel interne et effectue des hachages itératifs.
- **md5()** - Fonction très rapide adaptée au hachage de fichiers. La sortie est toujours de 32 caractères.
- **sha1()** - Fonction de hachage rapide pour le hachage de fichiers, utilisée en interne par Git pour le hachage de commit. La sortie est toujours de 40 caractères.

Hachage
-----------

```php
$password = 'secret-mot de passe';

echo password_hash($password); // Bcrypt
echo md5($password);
echo sha1($password);
```

**Avertissement:** Ni `md5()` ni `sha1()` ne conviennent pour le hachage de mots de passe, car il est facile de découvrir le mot de passe original, ou du moins de précalculer les mots de passe. Il est bien mieux d'utiliser `bcrypt`, qui a été développé pour le hachage de mots de passe.
>
> Le site <a href="https://www.md5cracker.com/">md5cracker.com</a> possède une base de données de sommes de contrôle (hashs), essayez de rechercher le hash : `79c2b46ce2594ecbcb5b73e928345492`, comme vous pouvez le voir, le `md5()` pur n'est donc pas si sûr pour les mots et mots de passe courants.

La seule solution correcte : `Bcrypt + salt`.
--------------------------------------

Dans la conférence <a href="https://www.youtube.com/watch?v=F58_A5TM-Sc">How not to mess up in the target plane</a>, David Grudl a abordé les moyens de hacher et de stocker correctement les mots de passe.

La seule solution correcte est : `Bcrypt + salt`.

Plus précisément :

```php
$password = 'dièse';

// Génère un hachage sécurisé
echo password_hash($password, PASSWORD_BCRYPT);

// Alternativement avec une complexité plus élevée (la valeur par défaut est 10) :
echo password_hash($password, PASSWORD_BCRYPT, ['coût' => 12]);
```

L'avantage du Bcryp réside principalement dans sa rapidité et son salage automatique.

Le fait qu'il soit **long** à générer, disons 100 ms, fait qu'il est très coûteux pour un attaquant de tester de nombreux mots de passe.

En outre, le hachage de sortie est automatiquement traité avec un **sel aléatoire**, ce qui signifie que lorsque le même mot de passe est haché à plusieurs reprises, la sortie est toujours un hachage différent. Par conséquent, un attaquant ne sera pas en mesure d'utiliser une table de hachage précalculée.

Par conséquent, nous ne serons pas en mesure de vérifier l'exactitude du mot de passe par un hachage répété, mais devrons appeler une fonction spécialisée :

```php
if (password_verify($password, $hash)) {
    // Le mot de passe est correct
} else {
    // Le mot de passe est incorrect
}
```

Salage des mots de passe
------------

Pour rendre le piratage de hachage plus difficile, il est bon d'insérer une chaîne supplémentaire dans l'entrée originale. L'idéal serait d'en choisir un au hasard. Ce processus est appelé **salage de mots de passe**.

La sécurité repose sur l'idée qu'un attaquant ne pourra pas utiliser une table précalculée de mots de passe et de hachages, car il ne connaîtra pas le sel et devra casser les mots de passe individuellement.

Par exemple :

```php
$password = 'passeport secret';
$salt = 'fghjgtzjhg';

$hash = md5($password . $salt);

echo $password; // imprime le mot de passe original
echo $hash;     // imprime le hachage du mot de passe avec le sel
```

Fonctions de hachage composées
------------------------

Vous pourriez penser que ce serait une bonne idée d'exécuter la fonction de hachage de façon répétée, ce qui augmenterait la complexité du craquage, puisque le mot de passe original devra être haché à plusieurs reprises.

Par exemple :

```php
$password = 'mot de passe';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x haché via md5()
```

Paradoxalement, la difficulté de percer est réduite ou reste presque la même.

La raison en est que la fonction `md5()` est extrêmement rapide et peut calculer plus d'un million de hachages par seconde sur un ordinateur ordinaire, de sorte qu'essayer les mots de passe un par un ne ralentit pas beaucoup.

La deuxième raison relève davantage de la théorie, à savoir la possibilité de se heurter à une soi-disant collision. Si nous hachons un mot de passe à plusieurs reprises, il se peut qu'au fil du temps nous tombions sur un hachage que l'attaquant connaît déjà, ce qui lui permettra de hacher le mot de passe en utilisant la base de données.

Par conséquent, il est préférable d'utiliser une fonction de hachage sécurisée lente et de n'effectuer le hachage qu'une seule fois, tout en continuant à traiter la sortie finale par salage.

Comparaison extrêmement sûre de deux hachages/chaînes de caractères
---------------------------------------------------

Saviez-vous que l'opérateur === n'est pas le choix le plus sûr pour la comparaison de hachages dans la vérification de mots de passe ?

Lorsqu'il compare des chaînes de caractères, il parcourt les deux chaînes caractère par caractère jusqu'à ce qu'il arrive à la fin (succès, elles sont identiques) ou qu'il n'y ait pas de différence (les chaînes sont différentes).

Et cela peut être exploité lors d'une attaque. Si vous mesurez le temps avec suffisamment de précision, vous pouvez estimer combien de caractères il reste à ajouter pour obtenir une correspondance exacte et atteindre la fin, ou vous pouvez estimer le chemin parcouru par les chaînes de caractères lorsque vous les comparez.

La solution consiste à utiliser la fonction hash_equals() chaque fois que des chaînes de caractères sont comparées, et il serait important qu'un attaquant puisse trouver la position où la comparaison a échoué.

Et comment la fonction fait-elle cela ? Il s'assure que la comparaison de deux cordes prend toujours le même temps, de sorte que vous ne pouvez pas savoir en mesurant le temps où la différence s'est produite. Je trouve certains types d'attaques vraiment très improbables et difficiles à mettre en œuvre. C'est l'un d'entre eux.
