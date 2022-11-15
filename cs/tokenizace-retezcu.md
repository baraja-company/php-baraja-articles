Tokenizace řetězců v PHP
========================

> id: "ba5b9c8d-9ba1-47df-b59f-da5131fdef10"
> slug:
> 	cs: tokenizace-retezcu
>
> publicationDate: "2022-11-15 20:00:00"
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec

Pro zpracování velmi složitých řetězců, které mají gramatiku, jako je například zdrojový kód programovacího jazyka, anotace popisující složené datové typy u metod, matematické výrazy, výpočty, vzorce a další, nelze použít regulární výrazy. Důvod je ten, že jde o natolik komplexní tvary řetězců obsahující řadu pravidel, že je zkrátka musíme zpracovávat po menších částech.

Když počítač zpracovává například zdrojový kód PHP, tak ho nejprve rozdělí na mnoho malých částí, které nesou vlastní význam. Těmto částem se říká "tokeny", a vyjadřují nejmenší samostatně stojící stavební kameny jazyka.

Princip parsování a tokenizace řetězce
--------------------------------------

Princip zpracování řetězce/jazyka je rozdělen do několika fází:

- V první fázi se čte zdrojový řetězec postupně znak po znaku, a přes regulární výrazy se hledají jednotlivé tokeny.
- Jakmile se najde první token, řetězec se ořízne, token se uloží do pole, a parser pokračuje dál.
- Až se dojde na konec řetězce, víme, že jsme sestavili kompletní pole tokenů.
- Získané tokeny předáme do další funkce, která řeší jejich zpracování. Typicky se prochází token po tokenu, kontroluje se validita gramatiky, a během toho se zpracovává výstup. Například se nahrazují proměnné, vyhodnocují podmínky a podobně.

Velká výhoda tohoto přístupu je také v tom, že během procházení tokenů známe jejich pozice v řetězci (jak řádek, tak i konkrétní znak počátku i konce tokenu), tak můžeme v případě vyhození výjimky přesně adresovat místo problému.

Motivace, proč tokenizovat
--------------------------

Představte si, že například implementujete algoritmus pro řešení matematického příkladu. Matematika má spoustu pravidel, jako jsou priority operátorů, závorky, provolávání funkcí a podobně.

Pokud vstupní řetězec dokážeme rozdělit na jednotlivé elementální tokeny, můžeme s ním pracovat na úplně jiné úrovni. Například snadno najdeme jednotlivé závorky, odpočítáme si tokeny od počáteční závorky po konečnou, podvýraz předáme ke zpracování do rekurzivní funkce a podobně.

Tokenizace nám umožňuje velmi elegantně řešit i komplexní parsovací problémy.

Jak tokenizovat v PHP
---------------------

Pro napsání vlastního tokenizéru nepotřebujeme zas tolik znalostí. V podstatě nám stačí znát princip regulárních výrazů a napsat si menší parsovací objekt.

Pro potřeby tohoto článku jsem připravil základní variantu tokenizéru, který vychází z tokenizeru jazyka Latte (Nette). Autorem původní implementace je David Grudl, kterému tímto děkuji za takto jednoduchou funkci, která řeší všechny problémy přímo za vás.

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

Tento tokenizer zvládne vyparsovat například takto složitý řetězec (formátoví je záměrně prokládáno mezerami, aby bylo vidět, že si tokenizér poradí s velkou škálou případů):

```
array<int,  array<bool,    array<string, float>>  >
```
