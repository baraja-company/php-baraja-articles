Zeichenketten-Tokenisierung in PHP
==================================

> id: ba5b9c8d-9ba1-47df-b59f-da5131fdef10
> slug:
> 	cs: tokenizace-retezcu
> 	de: zeichenketten-tokenisierung-in-php
> 
> publicationDate: '2022-11-15 20:00:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '3e4b250cc549211cc3cd63f46cd6e33e'

Reguläre Ausdrücke können nicht verwendet werden, um sehr komplexe Zeichenketten mit Grammatik zu behandeln, wie z. B. Quellcode von Programmiersprachen, Anmerkungen, die zusammengesetzte Datentypen für Methoden beschreiben, mathematische Ausdrücke, Berechnungen, Formeln und mehr. Der Grund dafür ist, dass es sich um so komplexe Zeichenfolgen handelt, die viele Regeln enthalten, dass wir sie einfach in kleineren Stücken verarbeiten müssen.

Wenn ein Computer zum Beispiel PHP-Quellcode verarbeitet, zerlegt er ihn zunächst in viele kleine Teile, die ihre eigene Bedeutung haben. Diese Teile werden "Token" genannt und sind die kleinsten in sich geschlossenen Bausteine der Sprache.

Das Prinzip des Parsing und der Tokenisierung einer Zeichenkette
--------------------------------------

Das Prinzip der String-/Sprachverarbeitung ist in mehrere Phasen unterteilt:

- In der ersten Phase wird die Quellzeichenkette Zeichen für Zeichen gelesen, und es wird mit Hilfe regulärer Ausdrücke nach einzelnen Token gesucht.
- Sobald das erste Token gefunden ist, wird die Zeichenkette abgeschnitten, das Token in einem Array gespeichert und der Parser fortgesetzt.
- Wenn das Ende der Zeichenkette erreicht ist, wissen wir, dass wir ein vollständiges Token-Array erstellt haben.
- Wir übergeben die extrahierten Token an die nächste Funktion, die ihre Verarbeitung übernimmt. Normalerweise parsen wir Token für Token, überprüfen die Gültigkeit der Grammatik und verarbeiten die Ausgabe nach und nach. Zum Beispiel werden Variablen ersetzt, Bedingungen ausgewertet usw.

Ein weiterer großer Vorteil dieses Ansatzes ist, dass wir die Position des Tokens in der Zeichenkette kennen (sowohl die Zeile als auch das spezifische Start- und Endzeichen des Tokens), während wir durch das Token gehen, so dass wir den Ort des Problems genau bestimmen können, wenn eine Ausnahme ausgelöst wird.

Motivation zur Tokenisierung
--------------------------

Stellen Sie sich zum Beispiel vor, Sie implementieren einen Algorithmus zur Lösung eines mathematischen Beispiels. In der Mathematik gibt es viele Regeln, z. B. Prioritäten der Operatoren, Klammern, Funktionsaufrufe usw.

Wenn wir die Eingabezeichenfolge in elementare Token aufteilen können, können wir auf einer ganz anderen Ebene mit ihr arbeiten. So können wir beispielsweise leicht einzelne Klammern finden, Token von der ersten bis zur letzten Klammer subtrahieren, einen Unterausdruck zur Verarbeitung an eine rekursive Funktion übergeben usw.

Die Tokenisierung ermöglicht es uns, selbst komplexe Parsing-Probleme sehr elegant zu lösen.

Wie Tokenisierung in PHP
---------------------

Wir brauchen nicht so viel Wissen, um unseren eigenen Tokenizer zu schreiben. Im Grunde müssen wir nur das Prinzip der regulären Ausdrücke kennen und ein kleines Parsing-Objekt schreiben.

Für die Zwecke dieses Artikels habe ich eine Basisversion eines Tokenizers auf der Grundlage des Latte (Nette) Tokenizers vorbereitet. Der Autor der ursprünglichen Implementierung ist David Grudl, dem ich für eine so einfache Funktion danken möchte, die alle Probleme für Sie löst.

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
		'Array' => 'Array',
		'<' => '\<',
		'>' => '\>',
		'{' => '\{',
		'}' => '\}',
		'oder' => '\|',
		'Liste' => '\[\]',
		'Typ' => '[a-zA-Z]+',
		'Raum' => '\s+',
		'Komma' => ',',
		'andere' => '.+?',
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

			throw new \LogicException(sprintf('Unerwarteter "%s" in Zeile %s, Spalte %s.', $token, $line, $col));
		}

		return $tokens;
	}
}
```

Dieser Tokenizer kann z. B. eine solche komplexe Zeichenkette analysieren (das Format ist absichtlich mit Leerzeichen durchsetzt, um zu zeigen, dass der Tokenizer eine große Bandbreite von Fällen verarbeiten kann):

```txt
array<int,  array<bool,    array<string, float>>  >
```
