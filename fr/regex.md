Expressions régulières en PHP
=============================

> id: '32142161-6ace-4cfd-b472-b48031219e9b'
> slug:
> 	cs: regex
> 	fr: expressions-regulieres-en-php
> 
> perex: 'Regulární výrazy a jejich kompletní vysvětlení. Hledání podřetězce, pokročilé nahrazování a generování řetězců.'
> publicationDate: '2020-03-08 13:37:38'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '392c8f14e6943dd8f345a9a134e0de92'

Les expressions régulières sont des outils qui vous permettent de rechercher, valider, comparer, diviser, réduire et remplacer facilement des chaînes de caractères en fonction d'un masque (motif). Il s'agit d'un outil très puissant et élégant pour la manipulation avancée des chaînes de caractères.

Masque
-----

Au départ, nous devons d'abord trouver l'expression régulière que nous allons exécuter. Il est saisi sous la forme d'une chaîne de texte, qui comporte un tas de règles et d'options de configuration (il s'agit d'une technique très complexe).

Pour commencer, il est important de noter que l'expression régulière est évaluée séquentiellement de la gauche vers la droite, et s'il existe plusieurs façons d'interpréter la chaîne de caractères, la correspondance la plus large possible est toujours utilisée (elle se comporte de manière **vorace**, en essayant de traiter autant de caractères que possible).

Le comportement et la stratégie de traitement d'une expression régulière peuvent être influencés par des commutateurs, qui sont nombreux.

Vérification simple que la chaîne est un e-mail valide
-----------------------------------------------

Comment vérifier simplement que la chaîne `jan@barasek.com` est une adresse électronique valide sans avoir à la diviser en parties complexes ou à la parcourir caractère par caractère ?

Les expressions régulières donnent la réponse (l'expression ci-dessus est très simplifiée pour les besoins de l'exemple, et une mise en œuvre réelle de la validation des adresses électroniques devrait être un peu plus compliquée) :

```php
$mail = 'jan@barasek.com';
$regex = '/^.+@.+\.(en|en|com)$/';

if (preg_match($regex, $mail)) {
   echo 'L'e-mail est valide';
} else {
   echo 'L'e-mail n'est pas valide';
}
```

Examinons l'expression `/^.+@.+\.(en|en|com)$/` de manière un peu plus détaillée :

Tout d'abord, nous devons envelopper l'expression entière dans une paire de caractères `/` (au début et à la fin) qui indiquent où l'expression commence et se termine. Le `/` à la fin de l'expression est suivi de tout modificateur (paramètres du mode de traitement de l'expression).

Lorsque vous traitez une expression, vous procédez du côté gauche, caractère par caractère. Chacun a sa propre signification, qui est donnée dans le tableau suivant :

| Caractère | Signification | Description | Exemple / Description du caractère
|------|-----------------|-------|-------------
| `^` | Début de la chaîne de caractères | Force la chaîne de caractères à commencer à ce point. | Force la chaîne de caractères à commencer par la séquence `+420` (utile pour la validation des nombres, par exemple) : `/^+420/`. |.
| `$` | Fin de chaîne ou de ligne | Force que la chaîne ou la ligne doit se terminer ici. La fin de ligne est alors échappée avec `\z`. <a href="https://phpfashion.com/vite-co-znamena-v-regularnim-vyrazu">Explication détaillée</a>. | Le nom du fichier doit être un fichier texte (se terminant par un point puis la chaîne "txt") : `/\.txt$/`. |
| `.` | Tout caractère | Attrape exactement n'importe quel caractère. | Vérifie que la chaîne contient exactement un de n'importe quel caractère : `/^.$/`. |.
| `\d` | Numéro | Détecte les caractères `0-9` | Détecte un numéro de téléphone qui ne contient pas d'espace et qui a 9 chiffres : `/^(\+420)?\d{9}$/`. |
| Les espaces, les traits d'union et les tabulations sont interdits. Les espaces sont autorisés entre les chiffres d'un numéro de téléphone à trois chiffres : `/^(\d{3}\s ?){3}$/`. |.
| `+` | Plusieurs caractères, mais au moins un | Répète la sous-expression précédente et essaie d'en attraper le plus possible. La sous-expression doit être répétée au moins une fois. | Capture autant de chiffres que possible, mais au moins un : `/\d+/`. |
| `*` | Plusieurs caractères, peut être aucun | Fonctionne de la même manière que `+`, mais permet d'attraper une chaîne vide (la valeur ne doit pas être présente) | Attrape autant de chiffres que possible, s'il n'y en a pas, attrape une chaîne vide : `/\d*/`.
| (`(``)` | parenthèses | Indique une sous-expression. Ceci peut être utilisé pour enfermer plusieurs balises différentes et ensuite exiger, par exemple, la répétition sur elles, ou pour piéger leur contenu dans une variable. | Divisons le code postal en 2 parties selon l'espace, qui est facultatif et il peut même y en avoir plus d'un : `/^(\d{3})\s*(\d{2})$/` |
| `\|` | Ou | Contient une sous-expression, ou une autre sous-expression. | Nombre commençant par `+420` ou `+421` : `/^+(420\|421)\s*\d+$/`. |
| `\.` | Échapper | Si nous voulons attraper un caractère dans une expression qui a autrement une signification spéciale, nous devons l'échapper de la même manière que, par exemple, les chaînes de caractères en PHP. | Attrape une paire de nombres séparés par un point (si nous n'avions pas échappé le point, il serait compris comme "n'importe quel caractère") : `/\d+\.\d+/`. |

Par souci d'exhaustivité, je vais donner la forme complète de la règle de validation pour le courrier électronique telle qu'elle est mise en œuvre par Nette :

```php
/**
 * Détermine si une chaîne de caractères est une adresse électronique valide.
 */
public static function isEmail(string $value): bool
{
   $atom = "[-a-z0-9!#$%&'*+/=?^_`{|}~]"; // RFC 5322 caractères non cités dans la partie locale
   $alpha = "a-z\x80-\xFF"; // sur-ensemble d'IDN
   return (bool) preg_match("(^
      (\"([ !#-[\\]-~]*|\\\\[ -~])+\"|$atom+(\\.$atom+)*)  # quoted or unquoted
      @
      ([0-9$alpha]([-0-9$alpha]{0,61}[0-9$alpha])?\\.)+    # domain - RFC 1034
      [$alpha]([-0-9$alpha]{0,17}[$alpha])?                # top domain
   \\z)ix", $value);
}
```

`preg_match()` - validation par motif
------------------------------------

La fonction de base pour la validation et l'analyse du format est `preg_match()`, elle a 2 paramètres obligatoires et le troisième peut être utilisé pour spécifier le champ de sortie.

Exemple :

```php
$psc = '272 01'; // Kladno

if (preg_match('/^(\d{3})\s*(\d{2})$/', $psc, $parser)) {
   echo 'Le code postal est valide [' . $parser[1] . ','. $parser[2] . ']';
} else {
   echo 'Le code postal n'est pas valide';
}
```

Le code renvoie : "Le code est valide [272, 01]".

Notez les parenthèses simples, que nous avons utilisées pour diviser l'expression en plusieurs parties plus petites. Cela permet ensuite d'obtenir les sous-expressions individuelles en tant qu'entrées de tableau. La fonction entière renvoie alors "vrai" ou "faux" selon que la chaîne de caractères a été capturée avec succès.

Parfois, cependant, il est très difficile de s'y retrouver dans l'ordre numérique des parenthèses, car le nombre peut changer ou il peut tout simplement y en avoir trop. Dans ce cas, il est possible de nommer les parenthèses individuellement et d'accéder ensuite aux clés en utilisant leurs noms.

Par exemple :

```php
$phone = '777 123 456';

preg_match('/^(?<operator>\d{3})\s*(?<number>[0-9 ]+)$/', $phone, $parser);

echo $parser['opérateur']; // retour 777
```

`preg_replace()` - remplacement par motif
----------------------------------------

Il est également possible de remplacer des chaînes de caractères à l'aide de regex, ce qui est particulièrement utile pour diverses corrections de format post-utilisateur.

Supposons que nous voulions stocker un numéro de téléphone saisi par l'utilisateur dans un nombre entier, puisque cela est requis par une bibliothèque tierce, mais que les utilisateurs peuvent le saisir dans des formats assez fous.

Dans ce cas, je m'en tiens au dicton :

> "Soyez généreux dans ce que vous recevez et rigoureux dans ce que vous envoyez".

C'est pourquoi nous ajustons automatiquement le format. Tout d'abord, nous utilisons l'analyse syntaxique pour décomposer la chaîne en ses différentes parties, puis nous la repassons en fonction des numéros de parenthèses :

```php
function formatPhoneNumber(string $phoneNumber): int
{
   return (int) preg_replace(
      '/^(\+\d{3})\s*(\d{3})\s*(\d{3})\s*(\d{3})$/',
      '$2$3$4',
      $phoneNumber
   );
}

echo formatPhoneNumber('+420 777 123 456');
```

Construction d'une chaîne de caractères en fonction d'une expression régulière
----------------------------------------

Les regex sont également très utiles pour générer de nouvelles chaînes de caractères selon un modèle complexe.

Pure PHP n'a pas de support pour cela, mais nous pouvons télécharger une bibliothèque tierce <a href="https://github.com/icomefromthenet/ReverseRegex">ReverseRegex</a> qui peut le faire.

Par exemple, nous pouvons vouloir générer un ensemble de mots de passe basés sur la regex `[a-z]{10}` et rien ne nous arrêtera :

```txt
jmceohykoa
aclohnotga
jqegzuklcv
ixdbpbgpkl
kcyrxqqfyw
jcxsjrtrqb
kvaczmawlz
itwrowxfxh
auinmymonl
dujyzuhoag
vaygybwkfm
```

L'utilisation est la suivante :

```php
use ReverseRegex\Lexer;
use ReverseRegex\Random\SimpleRandom;
use ReverseRegex\Parser;
use ReverseRegex\Generator\Scope;

require 'vendor/autoload.php';

$lexer = new  Lexer('[a-z]{10}');
$gen   = new SimpleRandom(10007);
$result = '';

$parser = new Parser($lexer, new Scope(), new Scope());
$parser->parse()->getResult()->generate($result, $gen);

echo $result;
```

Je génère mes exemples mathématiques dans Nette in Presenter, par exemple, de cette façon, et c'est possible avec une réelle facilité :

```php
public function actionRegex(): void
{
   $lexer = new Lexer('\d{1,3}[\+\-\*\/]');
   $parser = new Parser($lexer, new Scope(), new Scope());
   for ($i = 0; $i <= 10; $i++) {
      $result = '';
      $gen = new SimpleRandom($i);
      $parser->parse()->getResult()->generate($result, $gen);
      dump($result);
   }
   $this->terminate();
}
```

Ce qui est important pour la bibliothèque, c'est qu'elle génère toujours le même résultat pour la même entrée (même s'il peut sembler qu'il existe de nombreuses chaînes possibles à faire correspondre pour chaque expression régulière). Si nous voulons changer l'expression générée de façon aléatoire, nous devons également changer la `seed` par laquelle la chaîne de sortie est générée. Pour cela, il est utile de parcourir l'intervalle de la graine ou d'utiliser la fonction `rand(1, 1e6)`.

Capture et traitement des erreurs
-----------------------------

En PHP, attraper les erreurs dans les regex est plutôt infernal, mais il y a toujours une solution.

Ceci est expliqué en détail dans l'article <a href="https://phpfashion.com/zradne-regularni-vyrazy-v-php">Expressions régulières atteignables en PHP</a> de David Grudel.
