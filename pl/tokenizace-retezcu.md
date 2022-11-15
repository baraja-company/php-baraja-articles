Tokenizacja ciągów znaków w PHP
===============================

> id: ba5b9c8d-9ba1-47df-b59f-da5131fdef10
> slug:
> 	cs: tokenizace-retezcu
> 	pl: tokenizacja-ciagow-znakow-w-php
> 
> publicationDate: '2022-11-15 20:00:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '3e4b250cc549211cc3cd63f46cd6e33e'

Wyrażenia regularne nie mogą być używane do obsługi bardzo złożonych ciągów znaków, które posiadają gramatykę, takich jak kod źródłowy języka programowania, adnotacje opisujące złożone typy danych dla metod, wyrażenia matematyczne, obliczenia, formuły i inne. Powodem jest to, że są to tak złożone formy ciągów zawierające wiele reguł, że po prostu musimy je przetwarzać w mniejszych kawałkach.

Kiedy komputer przetwarza na przykład kod źródłowy PHP, najpierw rozbija go na wiele małych części, które niosą ze sobą własne znaczenie. Części te nazywane są "tokenami" i stanowią najmniejsze samoistne bloki konstrukcyjne języka.

Zasada parsowania i tokenizacji ciągu znaków
--------------------------------------

Zasada przetwarzania ciągów/języków jest podzielona na kilka faz:

- W pierwszej fazie łańcuch źródłowy jest czytany znak po znaku, a poszczególne tokeny są wyszukiwane za pomocą wyrażeń regularnych.
- Po znalezieniu pierwszego tokena, ciąg jest obcinany, token jest przechowywany w tablicy, a parser kontynuuje.
- Po osiągnięciu końca ciągu wiemy, że zbudowaliśmy kompletną tablicę tokenów.
- Wyodrębnione tokeny przekazujemy do kolejnej funkcji, która zajmuje się ich przetwarzaniem. Zazwyczaj parsujemy token po tokenie, sprawdzamy poprawność gramatyki i przetwarzamy dane wyjściowe w trakcie pracy. Na przykład zmienne są zastępowane, warunki są oceniane i tak dalej.

Kolejną dużą zaletą tego podejścia jest to, że znamy pozycję tokena w łańcuchu (zarówno linię, jak i konkretny znak początkowy i końcowy tokena), gdy przechodzimy przez token, więc możemy dokładnie zająć się lokalizacją problemu, jeśli zostanie rzucony wyjątek.

Motywacja do tokenizacji
--------------------------

Wyobraź sobie na przykład, że implementujesz algorytm do rozwiązania przykładu matematycznego. Matematyka ma wiele zasad, takich jak priorytety operatorów, nawiasy, wywołania funkcji i tak dalej.

Jeśli możemy podzielić ciąg wejściowy na tokeny elementarne, możemy pracować z nim na zupełnie innym poziomie. Na przykład możemy łatwo znaleźć poszczególne nawiasy, odjąć tokeny od początkowego nawiasu do końcowego, przekazać podwyrażenie do funkcji rekurencyjnej w celu przetworzenia i tak dalej.

Tokenizacja pozwala nam bardzo elegancko rozwiązywać nawet złożone problemy parsowania.

Jak tokenizować w PHP
---------------------

Nie potrzebujemy aż tak dużej wiedzy, aby napisać własny tokenizer. W zasadzie wystarczy, że poznamy zasadę działania wyrażeń regularnych i napiszemy mały obiekt parsujący.

Na potrzeby tego artykułu przygotowałem podstawową wersję tokenizatora opartego na tokenizatorze Latte (Nette). Autorem oryginalnej implementacji jest David Grudl, któremu chciałbym podziękować za tak prostą funkcję, która rozwiązuje wszystkie problemy za ciebie.

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
		'macierz' => 'macierz',
		'<' => '\<',
		'>' => '\>',
		'{' => '\{',
		'}' => '\}',
		'lub' => '\|',
		'wykaz' => '\[\]',
		'typ' => '[a-zA-Z]+',
		'przestrzeń' => '\s+',
		'przecinek' => ',',
		'inne' => '.+?',
	];


	/**
	 * @return array<int, Token>.
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

			throw new \LogicException(sprintf('Nieoczekiwane "%s" w linii %s, kolumna %s.', $token, $line, $col));
		}

		return $tokens;
	}
}
```

Ten tokenizer może parsować na przykład taki złożony ciąg (format jest celowo przeplatany spacjami, aby pokazać, że tokenizer może obsługiwać duży zakres przypadków):

```txt
array<int,  array<bool,    array<string, float>>  >
```
