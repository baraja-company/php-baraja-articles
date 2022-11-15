Tokenisation des chaînes de caractères en PHP
=============================================

> id: ba5b9c8d-9ba1-47df-b59f-da5131fdef10
> slug:
> 	cs: tokenizace-retezcu
> 	fr: tokenisation-des-chaines-de-caracteres-en-php
> 
> publicationDate: '2022-11-15 20:00:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '3e4b250cc549211cc3cd63f46cd6e33e'

Les expressions régulières ne peuvent pas être utilisées pour traiter des chaînes de caractères très complexes comportant une grammaire, comme le code source d'un langage de programmation, les annotations décrivant des types de données composés pour des méthodes, des expressions mathématiques, des calculs, des formules, etc. La raison en est qu'il s'agit de formes de chaînes si complexes contenant de nombreuses règles que nous devons simplement les traiter en plus petits morceaux.

Lorsqu'un ordinateur traite le code source de PHP, par exemple, il le décompose d'abord en de nombreuses petites parties qui ont leur propre signification. Ces parties sont appelées "tokens" et représentent les plus petits blocs de construction autonomes du langage.

Le principe d'analyse syntaxique et de tokenisation d'une chaîne de caractères
--------------------------------------

Le principe du traitement des chaînes de caractères et des langues est divisé en plusieurs phases :

- Dans la première phase, la chaîne de caractères source est lue caractère par caractère, et les tokens individuels sont recherchés au moyen d'expressions régulières.
- Une fois le premier jeton trouvé, la chaîne est tronquée, le jeton est stocké dans un tableau et l'analyseur syntaxique continue.
- Lorsque la fin de la chaîne est atteinte, nous savons que nous avons construit un tableau de jetons complet.
- Nous passons les jetons extraits à la fonction suivante, qui se charge de leur traitement. En général, nous analysons jeton par jeton, vérifions la validité de la grammaire et traitons la sortie au fur et à mesure. Par exemple, les variables sont remplacées, les conditions sont évaluées, et ainsi de suite.

Un autre grand avantage de cette approche est que nous connaissons la position du jeton dans la chaîne (à la fois la ligne et le caractère spécifique de début et de fin du jeton) au fur et à mesure que nous parcourons le jeton, ce qui nous permet d'aborder avec précision l'emplacement du problème si une exception est levée.

Motivation pour la tokénisation
--------------------------

Imaginez, par exemple, que vous implémentez un algorithme pour résoudre un exemple mathématique. Les mathématiques comportent de nombreuses règles, telles que les priorités des opérateurs, les parenthèses, les appels de fonction, etc.

Si nous pouvons diviser la chaîne de caractères d'entrée en jetons élémentaires, nous pouvons travailler avec elle à un niveau complètement différent. Par exemple, nous pouvons facilement trouver des parenthèses individuelles, soustraire des tokens de la parenthèse initiale à la parenthèse finale, passer une sous-expression à une fonction récursive pour traitement, etc.

La tokenisation nous permet de résoudre de manière très élégante des problèmes d'analyse syntaxique même complexes.

Comment tokeniser en PHP
---------------------

Nous n'avons pas besoin de tant de connaissances pour écrire notre propre tokenizer. En gros, il suffit de connaître le principe des expressions régulières et d'écrire un petit objet d'analyse syntaxique.

Pour les besoins de cet article, j'ai préparé une version de base d'un tokenizer basé sur le tokenizer Latte (Nette). L'auteur de l'implémentation originale est David Grudl, que je tiens à remercier pour cette fonction si simple qui résout tous les problèmes pour vous.

```php
final class Token
{
	public string $value;

	public int $offset;

	public string $type;
}

final class Tokenizer
{
	public const TokenTypes = [
		'tableau' => 'tableau',
		'<' => '\<',
		'>' => '\>',
		'{' => '\{',
		'}' => '\}',
		'ou' => '\|',
		'liste' => '\[\]',
		'type' => '[a-zA-Z]+',
		'espace' => '\s+',
		'virgule' => ',',
		'autre' => '.+ ?',
	];


	/**
	 * @return array<int, Token>
	 */
	public static function tokenize(string $haystack): array
	{
		$re = '~(' . implode(')|(', self::TokenTypes) . ')~A';
		$types = array_keys(self::TokenTypes);

		preg_match_all($re, $haystack, $tokenMatch, PREG_SET_ORDER);

		$len = 0;
		$count = count($types);
		$tokens = [];
		foreach ($tokenMatch as $match) {
			$type = null;
			for ($i = 1; $i <= $count; $i++) {
				if (isset($match[$i]) === false) {
					break;
				}
				if ($match[$i] !== '') {
					$type = $types[$i - 1];
					break;
				}
			}
			$token = new Token;
			$token->value = $match[0];
			$token->offset = $len;
			$token->type = (string) $type;

			$tokens[] = $token;
			$len += strlen($match[0]);
		}

		if ($len !== strlen($haystack)) {
			$text = substr($haystack, 0, $len);
			$line = substr_count($text, "\n") + 1;
			$col = $len - strrpos("\n" . $text, "\n") + 1;
			$token = str_replace("\n", '\n', substr($haystack, $len, 10));

			throw new \LogicException(sprintf('Unxpected "%s" on line %s, column %s.', $token, $line, $col));
		}

		return $tokens;
	}
}
```

Ce tokenizer peut analyser, par exemple, une telle chaîne complexe (le format est délibérément entrecoupé d'espaces pour montrer que le tokenizer peut traiter un large éventail de cas) :

```txt
array<int,  array<bool,    array<string, float>>  >
```
