String tokenization in PHP
==========================

> id: ba5b9c8d-9ba1-47df-b59f-da5131fdef10
> slug:
> 	cs: tokenizace-retezcu
> 	en: string-tokenization-in-php
> 
> publicationDate: '2022-11-15 20:00:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '3e4b250cc549211cc3cd63f46cd6e33e'

Regular expressions cannot be used to handle very complex strings that have grammar, such as programming language source code, annotations describing compound data types for methods, mathematical expressions, calculations, formulas, and more. The reason is that these are such complex string forms containing many rules that we simply have to process them in smaller chunks.

When a computer processes PHP source code, for example, it first breaks it into many small parts that carry their own meaning. These parts are called "tokens", and they represent the smallest self-contained building blocks of the language.

The principle of parsing and tokenizing a string
--------------------------------------

The principle of string/language processing is divided into several phases:

- In the first phase, the source string is read character by character, and individual tokens are searched for via regular expressions.
- Once the first token is found, the string is truncated, the token is stored in an array, and the parser continues.
- When the end of the string is reached, we know we have built a complete token array.
- We pass the extracted tokens to the next function, which handles their processing. Typically, we parse token by token, check the validity of the grammar, and process the output as we go. For example, variables are replaced, conditions are evaluated, and so on.

Another big advantage of this approach is that we know the token's position in the string (both the line and the specific token start and end character) as we go through the token, so we can accurately address the location of the problem if an exception is thrown.

Motivation to tokenise
--------------------------

Imagine, for example, that you are implementing an algorithm to solve a mathematical example. Mathematics has a lot of rules, such as operator priorities, parentheses, function calls, and so on.

If we can split the input string into elemental tokens, we can work with it on a completely different level. For example, we can easily find individual parentheses, subtract tokens from the initial parenthesis to the final one, pass a subexpression to a recursive function for processing, and so on.

Tokenization allows us to solve even complex parsing problems very elegantly.

How to tokenize in PHP
---------------------

We don't need that much knowledge to write our own tokenizer. Basically, we just need to know the principle of regular expressions and write a small parsing object.

For the purposes of this article, I have prepared a basic version of a tokenizer based on the Latte (Nette) tokenizer. The author of the original implementation is David Grudl, whom I would like to thank for such a simple function that solves all the problems for you.

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
		'array' => 'array',
		'<' => '\<',
		'>' => '\>',
		'{' => '\{',
		'}' => '\}',
		'or' => '\|',
		'list' => '\[\]',
		'type' => '[a-zA-Z]+',
		'space' => '\s+',
		'comma' => ',',
		'other' => '.+?',
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

			throw new \LogicException(sprintf('Unexpected "%s" on line %s, column %s.', $token, $line, $col));
		}

		return $tokens;
	}
}
```

This tokenizer can parse, for example, such a complex string (the format is deliberately interspersed with spaces to show that the tokenizer can handle a large range of cases):

```txt
array<int,  array<bool,    array<string, float>>  >
```
