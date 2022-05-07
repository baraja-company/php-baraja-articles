La construction comprend
========================

> id: '881cc6ef-b1dc-4a71-ab22-d1943ce8095b'
> slug:
> 	cs: include-function
> 	fr: la-construction-comprend
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '53294e511809a77c08883100fd7416c6'

| Prise en charge de PHP 4, PHP 5 et PHP 7
|---------------|---------
| Short Description | Ajoute un autre fichier texte ou un script au script.
| Exigences | Autre fichier texte ou script à insérer.
| Note | Impossible de charger des fichiers externes.

Description
--------------------------

Insère un autre fichier texte ou un script dans la page. Prend en charge les fichiers de type texte brut.

Les fichiers incorporés se comportent comme s'ils étaient directement dans la page.

Les scripts intégrés sont exécutés automatiquement.

Le script incorporé transfère la valeur des variables.

> Ne peut être chargé à partir d'un stockage externe. Ne peut lire que du texte brut non formaté et des fichiers PHP.

Formats de chemin autorisés :

- `script.php` - fichier dans le même répertoire, sera exécuté
- `script.html` - fichier dans le même répertoire, ne sera pas exécuté
- `./file.php` - fichier dans le même répertoire, sera exécuté
- `../page.html`
- `folders\file\file.php` - écrire dans Windows
- `address/DalsiAddress/file.php` - écrire sur les systèmes Unix

Les barres obliques de type `****` et `**/**` peuvent s'interconvertir, vous n'avez donc pas à vous en occuper.

Entrée de chemin illégale : `https://domena.pripona/slozka/soubor.php`

Fonctions similaires
--------------------------

- <a href="/file-get-contents">file_get_contents()</a>

Exemple
--------------------------

```php
include 'file.php';
```

Il insère le script `file.php` dans la page et l'exécute.

`vars.php`

```php
$color = 'vert';
$fruit = 'pomme';
```

`test.php`

```php
$color = '';
$fruit = '';

echo 'A' . $color . '' . $fruit; // A

include 'vars.php';

echo 'A' . $color . '' . $fruit; // Une pomme verte
```

> AVERTISSEMENT : La notation suivante n'est pas possible, transférer le contenu des variables en les définissant !

```php
include 'file.php?parameter=neco';
```

Valeurs de retour
--------------------------

Aucun, met juste le fichier.

**NOTE:** Permet de comparer le contenu des fichiers, mais cela constitue un risque pour la sécurité. L'ordre des parenthèses est important ! Exemple :

```php
if ((include 'file.php') == 'OK') {
    echo 'La valeur est "OK".';
}
```

> Ceci est un risque potentiel pour la sécurité !
>
> Peut être résolu avec **file_get_contents()**, **readfile()** ou **fopen()**. Fopen() ne doit être utilisé que pour les fichiers .txt et .html.
>
> Une lecture plus sûre du fichier peut être résolue en définissant une fonction personnalisée.

Exemple :

```php
$string = get_include_contents('unfichier.php');

function get_include_contents(string $filename): ?string
{
    if (is_file($filename)) {
        ob_start();
        include (string) $filename;
        return ob_get_clean();
    }

    return null;
}
```

Notes et conseils d'autres développeurs
--------------------------

**Jakub Vrána** m'a envoyé un e-mail :

```php
include 'articles/' . $_GET['Article'] . '.html';
```

C'est extrêmement dangereux.

Un attaquant peut transmettre un lien vers un autre répertoire en utilisant `../` ou quelque chose de similaire comme nom d'article, et il est parfois possible de se débarrasser de la fin en passant un octet nul à la fin.

Vous devriez au moins utiliser la fonction `basename()`, mais il est préférable de n'autoriser que les valeurs de la liste blanche.
