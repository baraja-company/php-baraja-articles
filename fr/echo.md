Echo - sortie vers le code source
=================================

> id: d6a1a7bf-03bf-49ea-9947-d4d6753ca6e4
> slug:
> 	cs: echo
> 	fr: echo---sortie-vers-le-code-source
> 
> perex: Echo - výpis řetězce do stránky a na standardní výstup. Možnosti escapování a HTTP hlaviček.
> publicationDate: '2020-02-16 18:40:31'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '7d611691825ec537f58f387696d13e28'

La construction `echo` est utilisée pour vider une variable ou une chaîne dans le code source.

| Support : | Toutes les versions
|----------------|------
| Brève description : | Sortie d'une ou plusieurs chaînes de caractères
| Type : | <a href="/commandes-et-fonctions">commande, construction</a> (pas une fonction)

Description
-----

```php
echo 'Bonjour, le monde';
```

Il dit "hello world".

```php
$var = 'Texte';
echo $var;
```

Imprime la valeur de la variable `$var`, c'est-à-dire "Texte".

**Echo** n'est pas une fonction (c'est une <a href="/commandes-et-fonctions">commande</a>), vous pouvez donc utiliser ou non une parenthèse. Ainsi, écrire `echo ('hello world');` est également correct.

**Note supplémentaire : ** PHP considère **Echo** comme une commande (une construction) et le traite donc comme une expression. La parenthèse est facultative dans ce cas. Si nous donnons la notation : `echo ('quelque chose');`, l'instruction **Echo** ne devient pas une fonction et n'est pas traitée comme telle. Dans ce cas, la parenthèse permet d'entourer la valeur exacte de l'expression, comme cela se fait en mathématiques.

Les guillemets
--------

Les chaînes de caractères peuvent être placées entre guillemets et apostrophes.

Alors ça :

```php
echo "Bonjour";
```

C'est la même chose que ça :

```php
echo 'Bonjour';
```

Mais attention, chaque chaîne doit commencer et se terminer par le même type de caractère de citation et le caractère de citation ne doit pas être utilisé dans la chaîne.

Par exemple, si vous souhaitez éditer un lien HTML (ou tout autre code HTML), vous devez faire précéder le guillemet d'une barre oblique. Une barre oblique signifie "exactement ce caractère", elle n'est donc pas comprise comme une expression dans la langue.

```php
echo "<a href="index.php\">lien texte</a>";
```

Note technique : Les guillemets ont une <a href="/quotation-meaning">signification particulière</a> en PHP.

Paramètres
---------

- `arg` Paramètre de sortie.

Valeurs de retour
-----------------

Aucune valeur n'est renvoyée.

Ne peut pas être utilisé comme une variable.

Note
--------

Note : Comme il s'agit d'une **construction linguistique** (construct = commande) (et non d'une fonction), elle ne peut pas être chargée dans une variable.

Exemple
-------

```php
echo "Bonjour, le monde";

echo "echo" peut produire plusieurs lignes de texte.
Mais attention à la balise HTML <br>, elle n'est pas imprimée. C'est à ça que sert la fonction nl2br().";

$a = "php";				// définition de la variable

echo "J'aime" . $a;	// Il écrit : J'aime php
```

**Echo** a également une syntaxe plus courte, où il est possible d'utiliser uniquement le signe égal après la balise d'ouverture php.

```php
Ahoj <?=$jmeno;?>!
```

Ceci est utile si nous devons écrire quelques informations rapides sur la page. Par exemple, l'année en cours :

```php
Píše Jan Barášek © <?=date('Y');?>
```

> Cette syntaxe raccourcie ne fonctionnera que si les balises d'ouverture php raccourcies sont activées, c'est-à-dire si la directive `short_open_tag` est définie sur `on`.

Opération
-------

Toutes les opérations mathématiques courantes peuvent être effectuées à l'aide de la commande **echo**.

Pour une discussion détaillée des mathématiques, voir <a href="/mathematics">un article séparé</a>.

```php
echo 5 + 3 * 2; // imprime 11
```
