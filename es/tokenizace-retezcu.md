Tokenización de cadenas en PHP
==============================

> id: ba5b9c8d-9ba1-47df-b59f-da5131fdef10
> slug:
> 	cs: tokenizace-retezcu
> 	es: tokenizacion-de-cadenas-en-php
> 
> publicationDate: '2022-11-15 20:00:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '3e4b250cc549211cc3cd63f46cd6e33e'

Las expresiones regulares no pueden utilizarse para manejar cadenas muy complejas que tienen gramática, como el código fuente de los lenguajes de programación, anotaciones que describen tipos de datos compuestos para métodos, expresiones matemáticas, cálculos, fórmulas, etc. La razón es que se trata de formas de cadena tan complejas que contienen muchas reglas que simplemente tenemos que procesarlas en trozos más pequeños.

Cuando un ordenador procesa el código fuente de PHP, por ejemplo, primero lo divide en muchas partes pequeñas que tienen su propio significado. Estas piezas se denominan "tokens" y representan los bloques de construcción más pequeños y autónomos del lenguaje.

El principio de analizar y tokenizar una cadena
--------------------------------------

El principio del procesamiento de cadenas/lenguaje se divide en varias fases:

- En la primera fase, la cadena de origen se lee carácter por carácter, y se buscan los tokens individuales mediante expresiones regulares.
- Una vez que se encuentra el primer token, la cadena se trunca, el token se almacena en una matriz y el analizador continúa.
- Cuando se llega al final de la cadena, sabemos que hemos construido una matriz de fichas completa.
- Pasamos los tokens extraídos a la siguiente función, que se encarga de su procesamiento. Normalmente, analizamos token por token, comprobamos la validez de la gramática y procesamos el resultado sobre la marcha. Por ejemplo, se sustituyen las variables, se evalúan las condiciones, etc.

Otra gran ventaja de este enfoque es que conocemos la posición del token en la cadena (tanto la línea como el carácter específico de inicio y fin del token) a medida que avanzamos por el token, por lo que podemos abordar con precisión la ubicación del problema si se lanza una excepción.

Motivación para la tokenización
--------------------------

Imagine, por ejemplo, que está implementando un algoritmo para resolver un ejemplo matemático. Las matemáticas tienen muchas reglas, como las prioridades de los operadores, los paréntesis, las llamadas a funciones, etc.

Si podemos dividir la cadena de entrada en tokens elementales, podemos trabajar con ella a un nivel completamente diferente. Por ejemplo, podemos encontrar fácilmente paréntesis individuales, restar tokens desde el paréntesis inicial hasta el final, pasar una subexpresión a una función recursiva para su procesamiento, etc.

La tokenización nos permite resolver incluso problemas complejos de análisis sintáctico de forma muy elegante.

Cómo tokenizar en PHP
---------------------

No necesitamos tantos conocimientos para escribir nuestro propio tokenizador. Básicamente, sólo necesitamos conocer el principio de las expresiones regulares y escribir un pequeño objeto de análisis.

Para los fines de este artículo, he preparado una versión básica de un tokenizador basado en el tokenizador Latte (Nette). El autor de la implementación original es David Grudl, a quien me gustaría dar las gracias por una función tan sencilla que te resuelve todos los problemas.

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
		'matriz' => 'matriz',
		'<' => '\<',
		'>' => '\>',
		'{' => '\{',
		'}' => '\}',
		'o' => '\|',
		'lista' => '\[\]',
		'tipo' => '[a-zA-Z]+',
		'espacio' => '\s+',
		'coma' => ',',
		'otros' => '.+?',
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

			throw new \LogicException(sprintf('Inesperado "%s" en la línea %s, columna %s.', $token, $line, $col));
		}

		return $tokens;
	}
}
```

Este tokenizador puede analizar, por ejemplo, una cadena compleja de este tipo (el formato está deliberadamente intercalado con espacios para mostrar que el tokenizador puede manejar una amplia gama de casos):

```txt
array<int,  array<bool,    array<string, float>>  >
```
