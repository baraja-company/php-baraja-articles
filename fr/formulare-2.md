Formulaires, traitement des formulaires en PHP
==============================================

> id: d1871a8d-bcfe-408d-ac12-6b827633f77e
> slug:
> 	cs: formulare-2
> 	fr: formulaires-traitement-des-formulaires-en-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '91f43ee06328e88a6a1058ecbf028222'

Je suppose que nous avons créé un formulaire HTML, que nous envoyons et maintenant nous voulons traiter les données. Il existe un <a href="/formulare">article séparé</a> sur la création d'un formulaire HTML.

Réception des données - différentes manières
----------------------------

La manière dont le formulaire est envoyé est définie directement dans le HTML

Il y a 2 options :

- **GET** - Il est visible dans la barre d'adresse après le point d'interrogation.
 Par exemple : "php.baraja.cz/search.php?query=formulare".
- **POST** - Caché (non visible), la plupart des formulaires sont envoyés par la poste.

Nous devons ensuite les lire en PHP en utilisant la même méthode.

Récupérer les données de l'utilisateur et les transférer au script
------------------------------------------------------

La base est un formulaire HTML, comment le faire vous pouvez lire <a href="/formulare">dans un article séparé</a>.

Pour commencer, supposons un simple formulaire permettant de saisir le nom de l'utilisateur :

```html
<form action="welcome.php" method="GET">
   Zadejte jméno: <input type="text" name="username">
   <input type="submit" value="odeslat">
</form>
```

Une zone de texte apparaîtra pour saisir un nom et cliquer sur soumettre. Lorsque le bouton est cliqué, le contenu du champ est envoyé au script `welcome.php`.

Maintenant pour le traitement réel dans le fichier `welcome.php` :

```php
$username = $_GET['nom d'utilisateur'];

echo 'Le nom saisi est :' . $username;
```

Notez la variable spéciale `$_GET`. Il s'agit d'une variable **superglobale** qui contient les données du formulaire et à laquelle on peut accéder sous forme de tableau.

Le problème de cette solution, toutefois, est que les données reçues ne sont **pas sécurisées** et qu'un formulaire similaire peut être facilement attaqué. Par exemple, un attaquant potentiel peut saisir un code javascript dans le champ au lieu d'un nom, qui sera écrit sur la page et exécuté.

Par conséquent, nous devons toujours assainir les données de l'utilisateur avant de les intégrer au code HTML :

```php
$username = $_GET['nom d'utilisateur'] ?? 'Inconnu';

echo 'Le nom saisi est :' . htmlspecialchars($username);
```

Traitement ultérieur
----------------

Nous pouvons faire n'importe quoi avec les données reçues et les traiter comme n'importe quelle variable ordinaire.

Par exemple, ajoutez la valeur dans deux champs :

```php
echo $_GET['x'] + $_GET['y'];
```

Ou enregistrez dans un fichier, une base de données, un courriel, ...

Les fonctions suivantes sont utiles à cet effet :

- <a href="/file-put-contents">file_put_contents</a> - fonction permettant d'enregistrer des données dans un fichier
- <a href="/hashovani">MD5</a> - calcul de la somme de contrôle, par exemple pour les mots de passe
- <a href="/cookies">Cookies</a> - enregistrer des données dans les **cookies** (petits fichiers à l'intérieur du navigateur web).
