Apostrophes et guillemets
=========================

> id: '526ad995-3412-446e-bb56-9627dff8e29e'
> slug:
> 	cs: apostrofy-a-uvozovky
> 	fr: apostrophes-et-guillemets
> 
> perex: Použití uvozovek a apostrofů pro ohraničení řetězců v PHP. Escapování řetězců a řešení kontextů.
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c5b0bf8b74238134be5348f886591e2a

Vous pouvez utiliser des guillemets ou des apostrophes pour délimiter les chaînes de caractères. Personnellement, je ne préfère que les **apostrophes**, sauf s'il s'agit d'un caractère spécial de saut de ligne ou d'une tabulation.

Il y a plusieurs raisons à cela, passons-les en revue de manière constructive.

> La principale raison de ne pas utiliser les guillemets est la sécurité et la manipulation inappropriée des types de données.

Utilisation de balises HTML à l'intérieur d'une chaîne de caractères
--------------------------------

Si nous devons renvoyer un code HTML dans une chaîne de caractères, et que nous entourons la chaîne de caractères de guillemets, nous devons procéder à un échappement assez maladroit :

```php
return "<a href=\"{$link}\">{$text}</a>";
```

Ou faites la même chose de manière plus lisible, en conservant les guillemets sans échappement :

```php
return '<a href="' . $link . '">' . $text . '</a>';
```

Plus grande distance visuelle de la corde par rapport à la variable
---------------------------------------------

Il n'y a rien de pire que de coller des variables à l'intérieur d'une chaîne directement les unes sur les autres, où vous ne pouvez pas rapidement distinguer visuellement ce qui est une variable et ce qui est une chaîne :

```php
$url = "{$baseImageUrl}/{$dirName}/{$basename}.{$ext}";
```

Cette notation n'a aucun effet négatif sur la fonctionnalité, seulement les variables individuelles et les caractères fixes de la chaîne sont empilés trop près les uns des autres, ce qui dégrade la lisibilité.

Essayons à nouveau et rendons-le plus lisible :

```php
$url = $baseImageUrl . '/' . $dirName . '/' . $basename . '.' . $ext;
```

Le principal avantage que je vois est la propreté autour des variables, où aucun caractère inutile dans le nom ne gêne.

En outre, il sera plus facile de l'emballer et il n'y aura pas de caractères d'emballage dans la chaîne ;)

```php
$url = $baseImageUrl
       . '/' . $dirName
       . '/' . $basename . '.' . $ext;
```

Il n'est pas possible d'appeler une fonction entre guillemets.
---------------------------------------

Mais une variable le peut, c'est pourquoi "quelque chose" est ajouté à l'extérieur de la chaîne (en particulier les fonctions), mais les variables sont réécrites à l'intérieur. Et parfois, le programmeur ajoute même la variable après la chaîne de caractères. En bref, la confusion sur la confusion.

Pourquoi ne pouvons-nous pas faire les choses de manière uniforme ?

Mappage inapproprié d'un autre type de données vers une chaîne de caractères
---------------------------------------------------

Considérons l'appel de fonction suivant :

```php
echo getFullName("{$user->name}");

function getFullName(string $name): string
{
	// ... mise en œuvre ...
}
```

Il est possible d'insérer des variables entre guillemets, ce qui aura pour effet de les réécrire sous forme de chaîne de caractères. Cependant, si la variable se trouve dans la chaîne elle-même et qu'elle est d'un type de données différent de celui de la chaîne, les informations sur le type de données d'origine peuvent être corrompues. Par exemple, si la construction `$user->name` renvoie `false` ou `null`, nous ne pourrions pas dire qu'il s'agit d'une erreur.

Une application devrait toujours reconnaître une condition d'erreur et échouer correctement, plutôt que de l'ignorer. Un signalement correct des erreurs permet un meilleur débogage à l'avenir.

Occasionnellement, vous pouvez rencontrer des surcharges, notamment parce qu'une fonction requiert un type de données particulier :

```php
trim("{$returnText}");
```

Dans ce cas, je suis plus enclin à l'écrire :

```php
trim ((string) $returnText);
```

ce qui n'est pas si "magique" et il est évident, même pour les utilisateurs moins qualifiés, ce qui est arrivé à la variable.

Suppression de la valeur `null` de la base de données
----------------------------------

Supposons que nous récupérons le nom d'un hôtel dans une base de données :

```php
return "{$row->hotel->name}";
```

Mais que se passe-t-il si le nom n'existe pas et est `null` ? En utilisant des guillemets, nous avons créé une erreur potentiellement difficile à détecter, car elle sera réécrite en une chaîne vide. Cependant, nous aimerions retourner un vrai `null`. Cela fait une différence si l'enregistrement n'existe pas, ou s'il existe mais est vide.

La manière correcte de procéder serait donc de ne rien spécifier :

```php
return $row->hotel->name;
```

La fonction exige-t-elle le retour d'un type de données spécifique et est-elle stricte à ce sujet ? Si nous ne pouvons pas satisfaire à la spécification, nous devons lever une exception.

```php
function getHotelName(int $hotelId): string
{
   // mise en œuvre

   if ($row->hotel->name === null) {
      throw new \Exception('Le nom de l'hôtel n'existe pas.');
   }

   return $row->hotel->name;
}
```

Incohérences dans l'encodage des caractères et des types de données
--------------------------------------------

Il n'est pas fréquent que je rencontre une construction du type :

```php
echo "{$returnCode}" . self::CRLF;
```

Ce qui est mieux écrit comme :

```php
echo $returnCode . "\n";
```

L'utilisation de guillemets autour de la variable n'est pas nécessaire dans ce cas et ne fait que rendre la lecture plus difficile car il s'agit de caractères supplémentaires inutiles. En même temps, le retour à la ligne de type `CRLF` n'est pas moderne et il est préférable d'utiliser uniquement `n`, qui est natif du codage `UTF-8`.

Correctement, nous pourrions encore valider le type de données pour le code. Par exemple, qu'il s'agit d'un nombre et éventuellement renvoyer un remplacement. Cela peut être fait avec l'opérateur <a href="/ternary-operator">opérateurternaire</a> :

```php
echo (is_int($returnCode) ? $returnCode : '?') . "\n";
```

> Note : L'utilisation de la fonction `is_int()` indique une mauvaise conception de l'application, car il ne devrait jamais arriver que l'on ne connaisse pas le type de données d'une variable.

Envelopper la chaîne de sortie dans des apostrophes
---------------------------------------

Il est souvent nécessaire de placer la chaîne de sortie d'une variable entre guillemets ou entre apostrophes dans le texte de l'exception. Cependant, cela crée inutilement un "gain" pour les deux types de délimiteurs de chaîne, et si la chaîne commence et se termine par le même caractère, il n'est pas possible de déterminer rapidement et sans ambiguïté où la chaîne a commencé et s'est terminée dans le cas de chaînes multiples si une apostrophe figure à l'intérieur :

```php
throw new \Exception(__METHOD__ . ": Unsupported data type '{$fileType}'");
```

Personnellement, je résous ce problème en l'enfermant dans un type de caractère différent (les crochets fonctionnent bien pour moi), de sorte que le début et la fin de la chaîne puissent être vus de manière élégante :

```php
throw new \Exception(__METHOD__ . ': Type de données non supporté [' . $fileType . ']');
```

Ou :

```php
throw new \Exception("Status '{$status}' is invalid");
```

Peut être échangé contre :

```php
throw new \Exception('Statut [' . $status . '] n'est pas valide');
```

Analyse syntaxique par lignes
--------------------

Les guillemets sont utiles, par exemple, pour l'analyse d'une nouvelle ligne :

```php
foreach(explode("\n", $text) as $line) {
	// mise en œuvre
}
```

Ils ont été spécialement créés à cet effet.

Toutefois, si vous devez traiter un document plus complexe (tel que le code source d'un langage de programmation ou une page HTML), je vous recommande d'utiliser **Tokenizer** conjointement avec <a href="/regex">des expressions régulières</a>.

Ne pas combiner deux méthodes de fermeture
-----------------------------------

Si, pour une raison ou une autre, vous devez quand même utiliser des guillemets, veillez au moins à ce qu'ils soient cohérents.

C'est une affaire de dissuasion :

```php
throw new \Exception("L'image sur l'URL n'existe pas. ResponseSize :" . strlen($result) . ')');
```

Où le début de la chaîne est délimité par des guillemets et la fin par des apostrophes.

Ce défaut se produit notamment lors de l'utilisation d'outils de formatage automatique (tels que **Code Sniffer**) qui tentent d'unifier les conventions, mais n'y parviennent pas toujours.
