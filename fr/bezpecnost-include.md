Inclure la sécurité dans le PHP et les fichiers joints
======================================================

> id: '820f8de6-ff1e-406c-8fe5-95080642656f'
> slug:
> 	cs: bezpecnost-include
> 	fr: inclure-la-securite-dans-le-php-et-les-fichiers-joints
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1207d637c20fcb5e8f609be2eb449135'

Il arrive souvent que l'on veuille joindre à une page un fichier que l'on a stocké sur un autre disque. Si nous saisissons son nom exact directement dans la fonction attach, il n'y a aucun souci à se faire.

Joindre un fichier de manière sécurisée
--------------------------

```php
include 'menu.html';
```

L'écriture précédente est parfaitement sûre car nous montons toujours le même fichier. Une erreur de sécurité ne peut pas se produire dans ce cas. Le seul problème qui peut se produire est l'absence du fichier **menu.html**, ce qui déclenchera un message d'avertissement (qui n'apparaîtra probablement pas de toute façon), mais cette situation est rare car nous joignons généralement un fichier dont l'existence est pratiquement certaine.

Attacher un dossier selon le modèle
--------------------------

Mais que faire si, par exemple, nous voulons joindre des articles à un site de contenu simple comme celui-ci ? Sur ce site, j'ai un dossier physique où les articles sont stockés au format HTML et je les attache directement au code source.

Toutefois, le simple fait de se connecter ne suffit pas ! Un débutant peut appeler les articles individuels comme ceci :

```php
include 'articles/' . $_GET['Article'] . '.html';
```

> Mais c'est extrêmement dangereux. Un attaquant peut transmettre un lien vers un autre répertoire en utilisant `../` ou quelque chose de similaire comme nom d'article, et il est parfois possible de se débarrasser de la fin en passant un octet nul à la fin. Vous devriez au moins utiliser la fonction `basename()`, mais il est préférable de n'autoriser que les valeurs de la liste blanche.

Pourquoi ne pas charger des fichiers non pertinents ?
--------------------------

Souvent, le chargement d'un fichier incorrect (inattendu) ne nous dérange pas - c'est la faute de l'utilisateur qui demande une page qu'il ne souhaite pas vraiment, mais il peut y avoir des situations où cela est important. En particulier :

- L'utilisateur charge un fichier auquel le public n'a pas accès et seul le serveur peut y accéder.
- Le chargement d'un autre script PHP peut déclencher une action inattendue ou un message d'erreur qui peut donner un indice sur le fonctionnement du site et contribuer à d'autres attaques.
- Le chargement d'un autre fichier peut non seulement entraîner son ajout au document, mais aussi son exécution.

Liste blanche et validation des entrées
--------------------------

Si nous n'avons pas la possibilité de valider les entrées d'une manière sécurisée (par exemple à partir d'une liste blanche), nous ne devrions pas compter sur l'honnêteté de l'utilisateur et défendre les scripts, au moins au niveau PHP de toute façon.

La première chose importante est de placer tous les fichiers chargés dans le même dossier (répertoire) et de désactiver certains caractères dangereux, notamment la barre oblique et le point. Il sera ainsi impossible d'accéder à d'autres dossiers contenant des fichiers potentiellement vulnérables. Il est également possible de désactiver les caractères dangereux en les supprimant simplement de la chaîne d'entrée.

```php
$load = '../index'; // cette entrée peut être potentiellement dangereuse
$load = strtr($load, './', ''); // supprime tous les points et les barres obliques de la chaîne de caractères.
include $load .'.html';
```

Des fichiers en cours d'exécution ?
--------------------------

Il est important de noter que la construction **include** exécute les fichiers lorsqu'ils sont connectés de la même manière que s'il s'agissait de code PHP, c'est donc une bonne idée de prévoir cette possibilité.

Toutefois, il arrive souvent que nous joignions des fichiers qui ne doivent pas être exécutés par la suite et que nous ne soyons intéressés que par le texte stocké (contenu) sous la forme d'une chaîne. Par conséquent, nous pouvons charger le fichier dans une variable et travailler avec lui comme une chaîne de caractères, ce qui est assez sûr.

```php
$load = '../index'; // cette entrée peut être potentiellement dangereuse
$load = strtr($load, './', ''); // supprime tous les points et les barres obliques de la chaîne de caractères.
$file = file_get_contents($load . '.html'); // chargement du contenu dans la variable
echo $file; // vide le contenu du fichier
```

Cette solution semble intéressante et sûre à première vue - et elle l'est. Même si l'utilisateur parvient à appeler un fichier PHP, celui-ci ne sera jamais exécuté. Cependant, il peut l'afficher (c'est-à-dire son code source) et nous devons y faire attention.

Reconnaissance du fichier souhaité à partir du script
--------------------------

Il n'y a pas de guide définitif pour cela, chacun doit le faire lui-même en fonction des besoins du scénario. Par exemple, je reconnais un article à d'autres fichiers par le fait qu'ils contiennent un titre de taille H1. Ainsi, si quelqu'un charge un fichier où il n'y a pas de titre, je n'affiche rien et la page se termine par un message d'erreur. Il est toujours important de trouver une caractéristique unique que seuls les fichiers que vous voulez ont et que les autres fichiers n'ont pas, et vous pouvez partir de là.

Conclusion
--------------------------

Bien que la validation et le chargement des fichiers soient relativement simples, un grand nombre de débutants font encore des erreurs - et continueront à en faire. Le plus important est de bien comprendre le sens de ce que nous chargeons et de savoir comment distinguer le contenu que nous voulons du reste. Et surtout, travaillez avec le contenu sous forme de chaîne et ne le chargez jamais directement dans la page.
