PHP 8 est sorti - aperçu complet
================================

> id: '8b6ce751-195f-41d2-82c6-1af4be3e86b5'
> slug:
> 	cs: php-8-kompletni-prehled-novinek
> 	fr: php-8-est-sorti---apercu-complet
> 
> publicationDate: '2020-11-26 11:53:54'
> mainCategoryId: '17545205-215b-4962-b910-0d67ad1e933a'
> sourceContentHash: f657145ac1d2a1109fcc2a1ff3b6e6cf

Aujourd'hui, 26 novembre 2020, la nouvelle version majeure de PHP 8 est sortie après plusieurs années, et elle comprend un ensemble audacieux de nouvelles fonctionnalités. Cette mise à jour est l'une des plus importantes depuis longtemps et mérite un article spécial.

Dans cet article, nous allons résumer toutes les nouvelles fonctionnalités majeures et les différences de syntaxe et d'options par rapport à l'ancienne version. La plupart des nouvelles fonctionnalités sont rétrocompatibles et apportent des améliorations comportementales que vous apprécierez.

> **Information importante:** PHP 8 est maintenant dans une phase de `feature freeze`, ce qui signifie que de nouveaux comportements ne peuvent plus être ajoutés et que seuls les bogues sont corrigés. Vous pouvez donc compter sur la compatibilité et déboguer entièrement vos applications.

Types d'union
----------

PHP en général est passé ces dernières années d'un langage purement dynamique où n'importe quelle variable pouvait contenir n'importe quoi à une forme stricte où nous savons à l'avance quel type de données sera dans telle variable, tel paramètre, tel argument ou telle propriété. L'utilisation de [data-types](/datove-types) reste facultative, mais je recommande l'utilisation de la typographie forte et l'utilise moi-même sur tous les projets.


Les types d'union expriment une collection de types multiples, acceptant n'importe quel argument ou propriété dans ceux-ci.

Par exemple :

```php
function validatePsc(string|int $psc): bool
{
	// mise en œuvre
}
```

La fonction `validatePsc()` dans la variable `$psc` accepte les types de données `string` (chaîne de caractères) et `int` (nombre entier).

Dans la version précédente de PHP 7.4, cette notation n'était pas possible et était typiquement contournée par [comment](/document-comment) :

```php
/**
 * @param string|int $psc
 */
function validatePsc($psc): bool
{
	// mise en œuvre
}
```

Cependant, ce commentaire d'annotation est ignoré par PHP (c'est un commentaire, après tout) et nous avons dû effectuer une vérification supplémentaire avec un outil externe tel que PhpStan, que de nombreux développeurs ont ignoré. Désormais, la vérification est effectuée directement au moment de l'exécution (lorsque l'application est en cours d'exécution) et ne peut être contournée.

Cependant, PHP connaît un certain type d'union depuis la version 7, quand il était possible de dire que le type principal pouvait aussi être `nullable`, c'est-à-dire qu'il accepte le type de données principal plus la valeur `null`.

Cela a été écrit de deux façons, chacune ayant une signification différente :

```php
function setPhone(?string $phone): void
{
	// mise en œuvre
}

// ou

function setPhone(string $phone = null): void
{
	// mise en œuvre
}

// ou combinaison

function setPhone(?string $phone = null): void
{
	// mise en œuvre
}
```

Toutes les entrées disent que le téléphone `int` (entier) est soit une `string` soit `null`.

- La première notation nécessite toujours de passer la valeur
- La deuxième notation ne nécessite pas de valeur à passer ; si rien n'est passé, la valeur par défaut est `null` (c'est un argument optionnel).
- La troisième entrée est une combinaison des options et se comporte comme la deuxième entrée

Lors de l'utilisation de types d'union, nous ne pourrons plus utiliser une notation avec un point d'interrogation et devrons définir strictement le type de données `null`, par exemple :

```php
function setPhone(string|int|null $phone = null): void
{
	// mise en œuvre
}
```

Le numéro de téléphone doit maintenant être `string`, `int` ou `null`.

Les types d'union ont encore un certain nombre d'utilisations, que les développeurs avancés pourront lire dans la documentation ou l'implémentation de bibliothèques spécifiques.


JIT - traitement plus rapide des scripts
----------------------------------

Le compilateur JIT (just in time) apporte une amélioration significative des performances de la complication des scripts (analyse et compréhension). Toutefois, ce comportement peut varier dans le contexte des requêtes web.

Vous pouvez maintenant voir si le JIT est activé dans la barre Tracy du cadre Nette, et voir [article séparé] (https://stitcher.io/blog/php-jit) pour plus de détails.

La chose générale à dire à propos de la compilation est que PHP essaie de traiter le code en amont, de sorte que lorsqu'il traite une requête particulière, il n'a pas à parcourir un fichier script physique, le parser et l'interpréter. Dans le passé, cela était géré par l'extension OPCache (que les serveurs et les hôtes ont par défaut) et cela améliorait la vitesse de traitement d'environ la moitié.

En règle générale, si vous avez une application lente, il est toujours préférable de choisir un algorithme approprié pour traiter une tâche particulière plutôt que d'effectuer des micro-optimisations dans le code. En général, les gros retards sont dus à l'attente de la base de données et de ses requêtes lentes, à la sauvegarde des sessions, à l'attente de la libération de l'espace du disque dur et à d'autres opérations matérielles.

Opérateur Nullsafe (chaînage optionnel)
-------------------------------------

Très souvent dans une application réelle, il est nécessaire de vérifier l'existence d'une valeur de retour (qu'elle n'est pas `null`) d'une méthode et d'en appeler une autre de manière conditionnelle. Les [opérateurs ternaires](/opérateur ternaire) sont parfaits pour cela, mais ils ne fonctionnent qu'avec une seule condition et ne peuvent pas être imbriqués. L'opérateur nullsafe permet l'imbrication de manière native.

> **TIP:** Le même comportement est déjà pris en charge par le système de modèles Latte, mais il remplace ce type de syntaxe dans le code PHP natif, de sorte que vous pouvez utiliser l'opérateur nullsafe sur les anciennes versions de PHP (à partir de PHP 7). Félicitations à David pour cette modification !

Cela le rend facile à utiliser :

```php
$orderId = $order?->getId();
```

La variable `$orderId` contient soit la valeur retournée par la méthode `getId()`, soit `null` si la variable `$order` contient la valeur `null` et que la méthode `getId()` n'a pas pu être appelée.

Ce type de problème a été contourné en PHP 7 par la syntaxe suivante via l'opérateur ternaire :

```php
$orderId = isset($order) ? $order->getId() : null;
```

Peut-être une condition :

```php
if (isset($order)) {
	$orderId = $order->getId();
} else {
	$orderId = null;
}
```

L'entrée peut être écrite plus loin dans l'appel. J'ai pris l'échantillon de [Latte documentation] (https://latte.nette.org/cs/syntax#toc-volitelne-retezeni-s-nullsafe-operatorem), qui le décrit parfaitement :

```php
$orderName = $order->item?->name;

// même que :

$orderName = isset($order->item) ? $order->item->name : null;
```

L'utilisation typique est la liste des structures plus complexes dans un modèle, par exemple dans Latte, cela ressemble à ceci (exemple tiré de la documentation) :

```php
{$user?->address?->street}
// signifie approx ($user !== null) && ($user->address !== null) ? $user->address->street : null

{$items[2]?->count}
// remplacer approx ($items[2] !== null) ? $items[2]->count : null

{$user->getIdentity()?->name}
// remplacer approx $user->getIdentity() !== null ? $user->getIdentity()->name : null
```

Dans le code réel, cela peut ressembler à ceci, par exemple, que nous voulons connaître le pays du client en lisant son profil (et vous avez les données dans la base de données stockées correctement via des sessions, comme il est censé le faire), alors dans l'ancien PHP, cela ressemblait à ceci :

```php
$country =  null;
if ($session !== null) {
    $user = $session->user;
    if ($user !== null) {
        $address = $user->getAddress();
        if ($address !== null) {
            $country = $address->country;
        }
    }
}
```

Il peut maintenant être raccourci en une seule ligne :

```php
$country = $session?->user?->getAddress()?->country;
```

L'utilisation de l'opérateur nullsafe permet également d'éviter divers bogues qui ne pouvaient pas être facilement détectés par un développeur inexpérimenté en PHP 7.

Par exemple, cette entrée générera une erreur fatale :

```php
var_dump($invoice->getDate()->format('Y-m-d') ?? null);

// return : fatal error : uncaught Error : appel à une fonction membre format() sur null
```

La syntaxe correcte est la suivante :

```php
var_dump($invoice->getDate()?->format('Y-m-d'));

// renvoie : null
```

Arguments nommés
---------------------

Dans le bon vieux PHP, les appels de fonction avec arguments devaient être écrits en passant les arguments dans l'ordre exact défini par la fonction cible. Il n'y a rien de mal à cela, cependant, lorsque l'on utilise un certain nombre de paramètres ayant des valeurs similaires, cela peut nuire à la lisibilité. Ou si nous voulions passer jusqu'au nième paramètre dans l'ordre, tous les paramètres optionnels devaient être passés avant, ce qui pouvait avoir un effet négatif sur la lisibilité et la compatibilité avec l'avenir.

Imaginez, par exemple, la fonction `setCookie()` de Nette, qui a beaucoup d'arguments :

```php
public function setCookie(
	string $name,
	string $value,
	$time,
	string $path = null,
	string $domain = null,
	bool $secure = null,
	bool $httpOnly = null,
	string $sameSite = null
)
```

Les trois premiers arguments (`$name`, `$value` et `$time`) sont obligatoires, mais si nous voulons passer l'argument `$httpOnly`, nous devons passer tous les précédents et calculer l'ordre correctement :

```php
$http->setCookie(
	'monCookie',
	'David aime les chevaux',
	'maintenant',
	null, // chemin
	null, // domaine
	null, // sécurisé
	true
);
```

Ce que vous ne voulez tout simplement pas faire si vous n'y êtes pas obligé.

L'écriture élégante ressemble alors à :

```php
$http->setCookie(
    name: 'monCookie',
    value: 'David aime les chevaux',
    time: 'maintenant',
    httpOnly: true
);
```

Ce type de syntaxe exige que les noms des arguments de la fonction cible ne changent jamais, car ils seront toujours écrits lorsqu'ils seront appelés. Au moins, les développeurs seront en mesure de mieux les nommer.

Si nous voulons utiliser un seul des arguments, la syntaxe peut être combinée et condensée sur une seule ligne :

```php
$http->setCookie('monCookie', 'David aime les chevaux', 'maintenant', httpOnly: true);
```

Les 3 premiers arguments sont passés de la manière originale, puis l'argument optionnel `httpOnly` est passé (parce qu'il est nommé).

Attributs
---------

La plupart des grands langages, tels que Java ou C#, intègrent déjà de manière native ce que l'on appelle des annotations, c'est-à-dire une syntaxe du langage natif qui permet d'ajouter des méta-informations à d'autres constructions du langage.

En PHP, ce type de syntaxe a longtemps manqué, et a été contourné en utilisant les commentaires DOC, qui sont des commentaires classiques sur une méthode, sauf qu'ils ont deux astérisques `/**`.

Ces commentaires sont ignorés pendant le traitement du script et une logique utilisateur spéciale doit être ajoutée pour les analyser et les interpréter au moment de l'exécution via la réflexion. Vous pouvez probablement comprendre l'impact sur les performances que cela peut avoir, de plus la syntaxe des commentaires ne peut être exigée et est très difficile à vérifier au moment de la compilation (lorsque le script est traité avant d'être exécuté), et encore une fois vous devez utiliser des outils supplémentaires en dehors de la boîte à outils PHP normale pour le faire.

Pour préserver la compatibilité ascendante, PHP fournit des attributs avec une syntaxe similaire à la notation alternative des commentaires, ce qui n'empêche pas l'exécution du script sur les anciens systèmes PHP.

La notation originale (utilisée, par exemple, pour les dépendances Inject dans Nette Presenter) :

```php
final class HomepagePresenter extends BasePresenter
{
	/** @injecter */
	public EntityManager $entityManager;
}
```

Vous pouvez maintenant supprimer le commentaire et utiliser l'attribut natif :

```php
use App\Attributes\Inject;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Il est très important que l'attribut ne soit plus seulement un morceau de chaîne dans un commentaire, mais une classe physique qui est un code PHP valide.

C'est une bonne chose, car vous pouvez maintenant valider en toute sécurité les entrées d'un attribut, et l'utilisation d'un attribut devient en fait un appel à son constructeur où d'autres logiques peuvent être utilisées. J'attends avec impatience de voir cela supporté nativement par Doctrine, qui utilise les annotations pour tout.

L'implémentation de l'attribut lui-même pourrait alors ressembler à quelque chose comme ceci :

```php
#[Attribute]
class Inject
{
    public string $value;

    public function __construct(string $value)
    {
        $this->value = $value;
    }
}
```

La logique stricte peut être utilisée à nouveau dans les attributs, par exemple pour vérifier les types de données des arguments, les types d'union et d'autres caractéristiques du langage.

Expression de correspondance
----------------

La nouvelle construction du langage `match()` est une amélioration modernisée du bon vieux `switch()` (que j'essaie de ne pas utiliser), et apporte un certain nombre de fonctionnalités intéressantes (qui me feront recommencer à l'utiliser).

Par exemple, nous voulons modifier la valeur d'une variable en fonction de l'entrée :

```php
$pozdrav = match(bool $formal) {
    true => 'Bonjour',
    false => 'Bonjour',
};
```

Une nouvelle caractéristique importante de la syntaxe est que nous n'avons pas besoin d'utiliser `break` (comme l'ancien `switch`) et la syntaxe est généralement beaucoup plus économique.

En même temps, nous pouvons valider plusieurs entrées à la fois dans une condition (séparées par une virgule) et éventuellement renvoyer une valeur par défaut (si aucune ne satisfait).

Cela s'avère pratique pour réécrire le code d'état HTTP en un message d'erreur, par exemple (vous l'apprécierez certainement lors de la gestion des codes d'exception) :

```php
$message = match ($statusCode) {
    200, 300 => null,
    400 => 'non trouvé',
    500 => 'erreur de serveur',
    default => 'code de statut inconnu',
};
```

La comparaison des valeurs est faite strictement par l'opérateur `===` (switch n'utilise que `==`), ce qui montre encore une fois que PHP suit le chemin strict de la conception. Par conséquent, l'entrée `'200'` (une chaîne contenant un nombre) ne sera pas acceptée dans le cas précédent.

Si vous ne spécifiez pas de valeur pour `default` et qu'il n'y a pas de correspondance, un `UnhandledMatchError` est lancé.

La nouvelle syntaxe permet également d'utiliser une expression ou un appel de fonction pour la correspondance (elle se comporte comme une condition). En cas d'erreur, nous pouvons alors lancer une exception (puisque le jeton `throw` est maintenant une expression et peut être utilisé de cette façon) :

```php
$message = match ($statusCode) {
    200 => null,
    $this->checkServerError($statusCode) => throw new ServerError(),
    default => 'code de statut inconnu',
};
```

Propagation des propriétés dans le constructeur
-------------------------------------------------

Il s'agit simplement d'un sucre syntaxique qui sera utile pour définir rapidement et facilement une entité et ses propriétés directement dans le constructeur.


Par exemple, l'entité originale :

```php
final class User
{
    public string $name;

    public function __construct(
        string $name,
    ) {
        $this->name = $name;
    }
}
```

On ne peut que l'abréger :

```php
final class User
{
    public function __construct(
        public string $name
    ) {}
}
```

La propriété `$name` est validée par rapport au type de données `string` et sa valeur peut être lue directement depuis l'instance car c'est une propriété publique. Si vous utilisez un SmartObject supplémentaire dans Nette (ce que je ne recommande pas pour PHP 8), vous pouvez aussi accéder aux propriétés privées en appelant d'abord leur méthode getter, et cette syntaxe simplifie encore les choses.

Type de retour statique
--------

Dans le passé, nous pouvions utiliser le type de données `self` comme valeur de retour d'une méthode, mais il renvoie une instance de la classe même où il est défini. Le type de données `static` peut généralement le faire même en cas d'héritage, et retournera le type de données de la classe à partir de laquelle l'instance a été exécutée, et non son ancêtre.

Par exemple :

```php
class BaseEndpoint
{
    public function getInstance(): static
    {
        return new static();
    }
}
```

Type de données mixte
------------------

Le type `mixed` peut maintenant être utilisé comme argument de fonction ou de méthode. Cela signifie que la méthode doit toujours accepter une entrée (et est donc un argument obligatoire).

Si vous le pouvez au moins un peu, utilisez toujours un type de données direct, ou au moins une union. Le mélange n'est utile que si la fonction accepte vraiment n'importe quoi. En pratique, cette utilisation est utile, par exemple, pour diverses fonctions de vidage qui acceptent une entrée arbitraire et doivent être capables de l'afficher.

Le type `mixed` accepte les types suivants : `string`, `int`, `float`, `null`, `bool`, `array`, `callable`, `object`, `resource`.

David utilisera alors le type mixte pour sa fonction :

```php
function bdump(mixed $var): mixed
{
	Tracy\Debugger::barDump($var);
	return $var;
}
```

Lancer un jeton comme une expression
------------------------

Le jeton `throw` est maintenant devenu une expression, ce qui signifie en pratique qu'une exception peut être levée lorsque la fonction lambda `fn()` est tronquée, ou lorsqu'un opérateur ternaire est vérifié :

```php
$error = fn () => throw new \InvalidArgumentException('Cela entraîne toujours une erreur.');

$userName = $user['nom'] ?? throw new \LogicException('L'utilisateur doit avoir un nom.');
```

La fonction str_contains()
-----------------------

PHP inclut enfin une fonction native pour vérifier que la chaîne par défaut contient une sous-chaîne.

Par exemple :

```php
if (str_contains('Honzik aime les chats.', 'chats')) {
	echo 'La fonction s'occupe des chats.';
}
```

Dans le passé, l'occurrence d'une sous-chaîne était vérifiée par la fonction [strpos](/strpos-function) :

```php
if (strpos('Honzik aime les chats.', 'chats') !== false) {
	echo 'La fonction s'occupe des chats.';
}
```

Fonctions str_starts_with() et str_ends_with()
----------------------------------------------

Une paire de nouvelles fonctions pour vérifier si une chaîne commence ou se termine par une sous-chaîne :

```php
str_starts_with('Honzik aime les chats.', 'Honzik'); // vrai

str_ends_with('Honzik aime les chats.', 'chats.'); // vrai
```

Fonction get_debug_type()
-------------------------

Améliore la sortie de la fonction [gettype](/function-gettype) existante, qui ne retournait que le type générique de la variable passée. La fonction est utilisée, par exemple, pour lancer une exception, lorsque nous recevons une entrée non valide et que nous voulons dire à l'utilisateur ce qu'il a réellement passé.

Lorsque nous appelons la fonction `gettype()` avec une variable contenant une instance de la classe `App\User`, la fonction renvoie `objet`, nous ne savons donc pas de quelle classe il s'agit. La nouvelle fonction `get_debug_type()` renvoie le nom de la classe.

La fonction get_resource_id()
--------------------------

Cette fonction renvoie l'identifiant d'une ressource externe à partir d'une variable.

Par exemple, la connexion à une base de données MySql est gérée par PHP en utilisant un type de données spécial `resource`, il est maintenant possible de savoir quel ID lui a été attribué.

> **Note historique:**
>
> Le type `resource` de PHP a été créé à un moment où il ne savait pas encore comment utiliser les objets, et a dû trouver un moyen de passer des références à quelque chose comme un type `data`. À l'avenir, vous pouvez vous attendre à ce que `resource` soit complètement supprimé du langage, il est donc préférable de ne pas utiliser cette fonctionnalité.

L'extension ext-json est toujours disponible
-------------------------------------

Dans le passé, PHP pouvait être compilé sans le support de json. Maintenant, json sera toujours disponible, donc vous pouvez supprimer la dépendance `ext-json` de vos fichiers `composer.json` et toujours savoir que json peut être utilisé.

Préséance de la concaténation
-------------------------------------------------------

Imaginez quelque chose comme :

```php
echo 'La somme totale :' . $a + $b;
```

Est-ce que l'addition des nombres se fait d'abord, ou est-ce que la variable `$a` est d'abord ajoutée à la chaîne et ensuite la nouvelle chaîne entière est ajoutée à `$b` ?

On pourrait s'attendre à ce que l'addition soit faite en premier, mais c'est une belle supposition. PHP fait en fait quelque chose comme ça :

```php
echo ('La somme totale :' . $a) + $b;
```

PHP 8 se comporte maintenant de manière prévisible :

```php
echo 'La somme totale :' . ($a + $b);
```

En général, cependant, il est toujours préférable d'utiliser des parenthèses pour délimiter une expression.

Ordre stable
---------------

Avant PHP 8, le tri des chaînes de caractères était effectué en utilisant l'algorithme dit instable, ce qui signifie que PHP ne garantissait pas que les éléments ayant la même valeur (ou une valeur équivalente) ne seraient pas échangés. La nouvelle version modifie le comportement de toutes les fonctions de tri en le rendant stable, de sorte que le tri est toujours effectué de manière déterministe et que vous obtenez toujours le même résultat.

Cela résout, par exemple, les cas où nous classions les évaluations des utilisateurs par pertinence, mais où certaines évaluations avaient le même score. Maintenant, ils apparaîtront dans le même ordre à chaque fois que vous les trierez et ne sauteront pas continuellement.

Autres nouvelles fonctionnalités
---------------

PHP a de nombreuses autres nouvelles fonctionnalités et améliorations mineures. Par exemple, les erreurs seront lancées différemment (mais cela ne nous dérange pas, nous qui écrivons du code sans erreur, n'est-ce pas ?)

Vous pouvez toujours consulter la liste complète des changements dans la documentation officielle et le post RFC.

Ce qui me manque dans le nouveau PHP
-----------------------

J'aimerais que PHP supporte enfin les types de tableaux composites, par exemple lorsqu'une méthode retourne un tableau d'identifiants, nous devons toujours spécifier juste `getIds() : array` et quelque chose comme `getIds() : int[]` serait bien mieux. Peut-être que nous verrons cela bientôt et que la vérification forte des types sera complète.

Plus de ressources
------------

David Grudl a fait un exposé intéressant sur les nouveautés de Posobot. Je vous recommande de regarder l'enregistrement :

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/ZYkwmcCUTCg" frameborder="0" allow="accelerometer ; autoplay ; clipboard-write ; encrypted-media ; gyroscope ; picture-in-picture" allowfullscreen></iframe>

Je tiens à remercier David pour sa conférence, car j'en ai tiré certaines informations pour cet article. En particulier, des informations sur le passage de Nette à PHP 8 et d'autres conseils sur les coulisses de PHP.
