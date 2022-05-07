Principes des variables d'écriture
==================================

> id: '4c8e631b-b305-4169-8885-4f9506155999'
> slug:
> 	cs: zasady-promennych
> 	fr: principes-des-variables-d-ecriture
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '5528d9b73c1d1c07c330a58e4aeaa06a'

Ceci est la deuxième partie d'une série de tutoriels sur PHP. Dans cet épisode, nous allons examiner les règles de base de l'écriture des variables.

Cette page n'est qu'un aperçu rapide. Si vous recherchez une description technique détaillée de toutes les fonctionnalités, j'ai écrit un <a href="/variable">article séparé</a>.

Syntaxe de base
--------------------------

Les variables en PHP commencent par le signe dollar `$` suivi immédiatement du nom.

```php
$zvire = 'chat';
```

Les chaînes de caractères (séquences de caractères) sont placées entre guillemets ou apostrophes :

```php
$a = "Les guillemets";
$b = 'apostrophes';
```

Les chiffres ne sont pas placés entre guillemets :

```php
$a = 5;
$b = 10;
$c = 3.14159;
```

Le nom de la variable ne peut être composé que de caractères de l'alphabet anglais et de chiffres. Le nom commence toujours par une lettre.

Si le nom est composé de plusieurs mots, il est d'usage d'utiliser la syntaxe `camelCase` (première lettre en minuscule et chaque autre mot commençant par une majuscule) :

```php
$kocka = 'Kitty';
$rychlyPocitac = 'Bien sûr que c'est le mien !';
$pocetRohuJednorozce = 1;
```

Le nom ne doit pas contenir d'espaces, de tirets, de crochets, de virgules, de guillemets, de parenthèses ou d'autres caractères spéciaux. Le seul caractère spécial autorisé est le "soulignement".

Les nombres décimaux doivent être écrits avec un point :

```php
$pi = 3.14159;
```

Il peut souvent être utile d'effectuer des opérations mathématiques directement lors de la définition d'une variable :

```php
$a = 5;
$b = 3;
$c = $a + $b;	// ajouter 5 + 3
echo $c;		// imprime 8
```

Insertion correcte d'un guillemet ou d'une apostrophe
--------------------------

Les guillemets et les apostrophes ne doivent pas être combinés de manière arbitraire. Par exemple, si nous décidons d'utiliser un guillemet, nous devons également terminer la chaîne par le guillemet et ne pas l'utiliser à l'intérieur.

C'est donc une erreur :

```php
echo "<img src="obrazek.gif">";
```

Parce qu'il n'est pas clair où la chaîne commence et finit. Les guillemets et les apostrophes ne peuvent pas être imbriqués.

Une solution possible est appelée <a href="/escapovani">escaping</a>, où le caractère problématique est précédé d'une barre oblique inversée.

```php
echo "<img src="image.gif">";
```

La barre oblique inversée indique que le caractère suivant sera exactement celui que nous voulons utiliser.

Toutefois, pour l'édition du code HTML, il est préférable d'entourer toute la chaîne de caractères d'apostrophes, puis d'utiliser les guillemets de la manière habituelle :

```php
echo '<img src="image.gif">';
```

Il peut également être inversé :

```php
echo "<img src='picture.gif'>";
```

Remplir une variable à partir d'une adresse url ou d'un formulaire
--------------------------

Les adresses contenant un point d'interrogation contiennent des informations sur les variables d'entrée, ainsi par exemple `index.php?page=contacts` désigne la variable `page` avec la valeur `contacts`. La valeur de cette variable est lue comme `$_GET['page']`.

Le caractère point d'interrogation n'est en aucun cas lié au nom du fichier sur le disque. C'est toujours le même fichier auquel on passe des paramètres dans l'adresse.

J'aborde cette question en détail dans mon article sur <a href="/methods-odesilani-dat">méthodes d'envoi de données</a>.

Définir le contenu d'une variable à partir d'une adresse
--------------------------

Certaines variables sont disponibles au moment de l'exécution du script (et peuvent donc être utilisées immédiatement), elles sont appelées **variables superglobales**. Par exemple, si nous voulons lire une valeur depuis une URL, nous utilisons la variable `$_GET`.
L'utilisation est la suivante :

```php
$a = $_GET['a'];

echo $a;
```

Ce script imprime ce qu'il a dans l'URL après le point d'interrogation dans le code source.

> Attention, cet échantillon n'est pas sûr ! Si un visiteur mal intentionné passait, par exemple, un code HTML dans l'URL, celui-ci serait inséré dans la page et exécuté. Par conséquent, nous devons toujours traiter la sortie ; la fonction `htmlspecialchars()` est utilisée pour cela.

```php
$a = $_GET['a'];

echo htmlspecialchars($a);
```

> Si nous accédons à la page sans spécifier le paramètre `?a=anything`, la variable `$_GET['a']` n'existera pas et PHP enverra un message d'erreur. Nous devons traiter cette condition avec une condition et ne rien faire si la variable n'existe pas (ou alternativement, sortir un contenu alternatif). L'existence de la variable peut être vérifiée avec la fonction `isset()`.

```php
if (isset($_GET['a'])) {
    $a = $_GET['a'];

    echo htmlspecialchars($a);
} else {
    echo 'La variable "a" n'existe pas !';
}
```

Exemple avec comptage
--------------------------

Avec les variables de l'adresse URL, nous pouvons faire des morceaux de chien, par exemple, les additionner et écrire directement le résultat :

```php
echo $_GET['a'] + $_GET['b'];
```

Si nous voulons inclure plusieurs paramètres d'entrée dans l'URL, nous devons les séparer par une esperluette (`&`). L'adresse peut ressembler à ceci : `index.php?a=5&b=3`.

Lier les entrées de texte (chaînes de caractères)
--------------------------

Nous pouvons aussi facilement lier 2 entrées de texte (chaînes de caractères). Pour ce faire, on utilise l'opérateur point. Vous pouvez créer un lien dans une variable ou lors de l'inscription.

```php
$a = 'chien';
$b = 'chat';

echo $a . 'a' . $b;
```

C'est écrit "chien et chat".
