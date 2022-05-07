Méthodes d'envoi des données (GET et POST)
==========================================

> id: '32f9083f-7cb1-469f-911a-765df059123d'
> slug:
> 	cs: metody-odesilani-dat
> 	fr: methodes-d-envoi-des-donnees-get-et-post
> 
> perex: 'Metoda GET a POST, získávání dat z formuláře a URL. Komunikace přes API a zpracování dat.'
> publicationDate: '2019-11-26 11:38:32'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '2282538597ebfe95877ae0e005ddd352'

En plus des variables normales, nous avons aussi des variables dites **superglobales** en PHP, qui portent des informations sur la page actuellement appelée et les données que nous passons.

Typiquement, nous avons un formulaire sur une page que l'utilisateur remplit et nous voulons transférer ces données au serveur web où nous les traitons en PHP.

Il existe 3 méthodes les plus couramment utilisées pour ce faire :

- `GET` ~ les données sont transmises dans l'URL en tant que paramètres.
- `POST` ~ les données sont transmises secrètement avec la requête de la page.
- <a href="/ajax-post">Ajax POST</a> ~ traitement javascript asynchrone

Méthode GET - `$_GET`.
--------------------

Les données envoyées par la méthode GET sont visibles dans l'URL (en tant que paramètres après le point d'interrogation), la longueur maximale est de 1024 caractères dans Internet Explorer (les autres navigateurs ne sont *presque* pas limités, mais les textes plus longs ne doivent pas être transmis de cette façon). L'avantage de cette méthode est surtout la simplicité (vous pouvez voir ce que vous envoyez) et la possibilité de fournir un lien vers le résultat du traitement. Les données sont envoyées à une variable.

L'adresse de la page de réception pourrait ressembler à ceci :

`https://____________.com/script.php?promenna=obsah&promenna2=obsah`

En PHP, nous pouvons alors, par exemple, écrire la valeur du paramètre `variable` comme suit :

```php
echo $_GET['promenade'];	// imprime "contenu".
```

> **Avertissement fort:** Cette méthode d'écriture des données directement dans la page HTML n'est pas sécurisée, car on peut passer, par exemple, du code HTML dans l'URL qui serait écrit dans la page et ensuite exécuté.
>
> Nous devons **toujours** traiter les données avant toute sortie sur la page, la fonction `htmlspecialchars()` est utilisée pour cela.
>
> Par exemple : `echo htmlspecialchars($_GET['variable']);``

Méthode POST - `$_POST`.
----------------------

Les données envoyées par la méthode POST ne sont pas visibles dans l'URL, ce qui résout le problème de la longueur maximale des données envoyées. La méthode POST doit toujours être utilisée pour envoyer les champs de formulaire, car elle permet de s'assurer que, par exemple, les mots de passe ne sont pas visibles et qu'un lien ne peut être fourni vers la page traitant le résultat d'une saisie particulière.

Les données sont disponibles dans la variable `$_POST` et l'utilisation est la même que pour la méthode GET.

Vérification de l'existence des données soumises
--------------------------------

Avant de traiter des données, nous devons d'abord vérifier que les données ont effectivement été envoyées, sinon nous accédons à des données de l'UE.
 à une variable inexistante, ce qui entraînerait un message d'erreur.

La fonction `isset()` est utilisée pour vérifier l'existence d'une variable.

```php
if (isset($_GET['Nom'])) {
    echo 'Votre nom :' . htmlspecialchars($_GET['Nom']);
} else {
    echo 'Aucun nom n'a été saisi.';
}
```

Formulaire de saisie des données
------------------------

Le formulaire est réalisé en HTML, pas en PHP. Il peut s'agir d'une simple page HTML. Toute la "magie" est gérée par le script PHP qui accepte les données.

Par exemple, nous pouvons utiliser un formulaire pour recevoir 2 numéros, envoyés par la méthode GET :

```html
<form action="script.php" method="get">
    První číslo: <input type="text" name="x">
    Druhé číslo: <input type="text" name="y">

    <input type="submit" value="Sečíst čísla">
</form>
```

Dans la première ligne, vous pouvez voir où les données seront envoyées et par quelle méthode.

Les 2 lignes suivantes sont des éléments de formulaire simples, notez l'attribut **name=""**, c'est le nom de la variable qui contiendra ce qui est maintenant dans le formulaire.

Viennent ensuite le bouton pour soumettre les données (obligatoire) et la balise HTML de fermeture du formulaire (obligatoire pour que le navigateur sache ce qu'il doit soumettre ou non).

> Nous pouvons avoir un nombre quelconque de formulaires sur une page, et ils ne peuvent pas être imbriqués. En cas d'imbrication, le formulaire le plus imbriqué est toujours envoyé et les autres sont ignorés.

Traitement des formulaires sur le serveur
-------------------------------

Maintenant nous avons un formulaire HTML terminé et nous l'envoyons à `script.php`, qui reçoit les données en utilisant la méthode GET. L'adresse de la requête de la page pourrait ressembler à ceci :

`https://________.com/script.php?x=5&y=3`

**script.php**

```php
$x = $_GET['x'];	// 5
$y = $_GET['y'];	// 3

echo $x + $y;		// imprime 8
```

Correctement, nous devons d'abord vérifier que les deux champs du formulaire ont été remplis, ce qui est fait avec la fonction `isset()` :

```php
if (isset($_GET['x']) && isset($_GET['y'])) {
    $x = $_GET['x'];	// 5
    $y = $_GET['y'];	// 3

    echo $x + $y;		// imprime 8
} else {
    echo 'Le formulaire n'a pas été rempli correctement.';
}
```

> **TIP:** Vous pouvez passer plusieurs paramètres à la construction `isset()` pour vérifier qu'ils existent tous.
>
> Par conséquent, au lieu de `isset($_GET['x']) && isset($_GET['y'])`, vous pouvez simplement spécifier :
>
> `isset($_GET['x'], $_GET['y'])`.

Traitement des données reçues par la méthode POST
--------------------------------------

Si les données sont reçues par POST, l'URL du script à traiter ressemblera toujours à ceci :

`https://________.com/script.php`

Et jamais autrement. Juste non. Les données sont cachées dans la requête HTTP et nous ne pouvons pas les voir.

> La méthode POST cachée est nécessaire pour envoyer les noms d'utilisateur et les mots de passe pour des raisons de sécurité.
>
> **Sécurité:** Si vous travaillez avec des mots de passe sur votre site, le formulaire de connexion et d'inscription doit être hébergé sur HTTPS et vous devez hacher les mots de passe de manière appropriée (par exemple, avec BCrypt).

Traitement des requêtes ajax
------------------------------

Dans certains cas, lors du traitement des requêtes ajax, il peut s'avérer difficile de récupérer les données. Cela s'explique par le fait que les bibliothèques ajax envoient généralement des données sous forme de données utiles `json`, alors que la variable superglobale `$_POST` ne contient que des données de formulaire.

Les données sont toujours accessibles, j'ai décrit les détails dans l'article <a href="/ajax-post">Gestion des requêtes POST ajax</a>.
