Tokenisering af strenge i PHP
=============================

> id: ba5b9c8d-9ba1-47df-b59f-da5131fdef10
> slug:
> 	cs: tokenizace-retezcu
> 	da: tokenisering-af-strenge-i-php
> 
> publicationDate: '2022-11-15 20:00:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '3e4b250cc549211cc3cd63f46cd6e33e'

Regulære udtryk kan ikke bruges til at håndtere meget komplekse strenge med grammatik, f.eks. kildekode til programmeringssprog, annotationer, der beskriver sammensatte datatyper for metoder, matematiske udtryk, beregninger, formler m.m. Årsagen er, at der er tale om så komplekse strengeformer, der indeholder mange regler, at vi simpelthen er nødt til at behandle dem i mindre bidder.

Når en computer f.eks. behandler PHP-kildekode, opdeler den den først i mange små dele, der har deres egen betydning. Disse dele kaldes "tokens", og de repræsenterer de mindste selvstændige byggeklodser i sproget.

Princippet om parsing og tokenisering af en streng
--------------------------------------

Princippet for behandling af strenge/sprog er opdelt i flere faser:

- I den første fase læses kildestrengen tegn for tegn, og der søges efter individuelle tokens ved hjælp af regulære udtryk.
- Når det første token er fundet, afkortes strengen, tokenet gemmes i et array, og analysatoren fortsætter.
- Når vi når slutningen af strengen, ved vi, at vi har opbygget et komplet token-array.
- Vi sender de ekstraherede tokens til den næste funktion, som håndterer behandlingen af dem. Typisk analyserer vi token for token, kontrollerer grammatikkens gyldighed og behandler output undervejs. F.eks. erstattes variabler, betingelser evalueres osv.

En anden stor fordel ved denne fremgangsmåde er, at vi kender tokens position i strengen (både linjen og det specifikke start- og sluttegn for tokenet), mens vi gennemløber tokenet, så vi præcist kan finde frem til problemets placering, hvis der opstår en undtagelse.

Motivation til at gøre det til et symbolsk element
--------------------------

Forestil dig f.eks., at du er ved at implementere en algoritme til at løse et matematisk eksempel. Matematikken har en masse regler, f.eks. operatørprioriteter, parenteser, funktionsopkald osv.

Hvis vi kan opdele inputstrengen i elementære tokens, kan vi arbejde med den på et helt andet niveau. Vi kan f.eks. nemt finde individuelle parenteser, trække tokens fra den første parentes til den sidste parentes, sende et deludtryk til en rekursiv funktion til behandling osv.

Tokenisering giver os mulighed for at løse selv komplekse parsingproblemer på en meget elegant måde.

Sådan tokeniseres i PHP
---------------------

Vi har ikke brug for så meget viden for at skrive vores egen tokenizer. I bund og grund skal vi blot kende princippet om regulære udtryk og skrive et lille parsingobjekt.

Til brug for denne artikel har jeg udarbejdet en grundlæggende version af en tokenizer baseret på Latte (Nette) tokenizer. Forfatteren af den oprindelige implementering er David Grudl, som jeg gerne vil takke for en så enkel funktion, der løser alle problemerne for dig.

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
		'eller' => '\|',
		'liste' => '\[\]',
		'type' => '[a-zA-Z]+',
		'rum' => '\s+',
		'komma' => ',',
		'andre' => '.+?',
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

			throw new \LogicException(sprintf('Uventet "%s" på linje %s, kolonne %s.', $token, $line, $col));
		}

		return $tokens;
	}
}
```

Denne tokenizer kan f.eks. analysere en sådan kompleks streng (formatet er med vilje krydret med mellemrum for at vise, at tokenizer kan håndtere en lang række tilfælde):

```txt
array<int,  array<bool,    array<string, float>>  >
```
