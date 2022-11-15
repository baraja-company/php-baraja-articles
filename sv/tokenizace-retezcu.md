Tokenisering av strängar i PHP
==============================

> id: ba5b9c8d-9ba1-47df-b59f-da5131fdef10
> slug:
> 	cs: tokenizace-retezcu
> 	sv: tokenisering-av-straengar-i-php
> 
> publicationDate: '2022-11-15 20:00:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '3e4b250cc549211cc3cd63f46cd6e33e'

Reguljära uttryck kan inte användas för att hantera mycket komplexa strängar med grammatik, t.ex. källkod till programmeringsspråk, kommentarer som beskriver sammansatta datatyper för metoder, matematiska uttryck, beräkningar, formler med mera. Anledningen är att dessa strängformer är så komplexa och innehåller många regler att vi helt enkelt måste bearbeta dem i mindre delar.

När en dator till exempel bearbetar PHP-källkod bryter den först upp den i många små delar som har sin egen innebörd. Dessa delar kallas "tokens" och utgör språkets minsta fristående byggstenar.

Principen för parsing och tokenisering av en sträng
--------------------------------------

Principen för bearbetning av strängar/språk är uppdelad i flera faser:

- I den första fasen läses källsträngen tecken för tecken och enskilda tecken söks med hjälp av reguljära uttryck.
- När den första token har hittats förkortas strängen, tokenet lagras i en array och analysatorn fortsätter.
- När slutet av strängen är nått vet vi att vi har byggt en komplett token array.
- Vi skickar de extraherade symbolerna till nästa funktion, som hanterar behandlingen av dem. Vanligtvis analyserar vi token för token, kontrollerar grammatikens giltighet och bearbetar resultatet under tiden. Variabler ersätts till exempel, villkor utvärderas och så vidare.

En annan stor fördel med det här tillvägagångssättet är att vi känner till tokens position i strängen (både raden och det specifika start- och sluttecknet för tokenet) när vi går igenom tokenet, så att vi exakt kan ta reda på var problemet finns om ett undantag uppstår.

Motivering till symbolisering
--------------------------

Tänk dig till exempel att du implementerar en algoritm för att lösa ett matematiskt exempel. Matematiken har många regler, t.ex. prioriteringar av operatörer, parenteser, funktionsanrop och så vidare.

Om vi kan dela upp inmatningssträngen i elementära tecken kan vi arbeta med den på en helt annan nivå. Vi kan till exempel enkelt hitta enskilda parenteser, subtrahera tecken från den första parentesen till den sista, skicka ett deluttryck till en rekursiv funktion för behandling och så vidare.

Tokenisering gör det möjligt att lösa även komplexa tolkningsproblem på ett mycket elegant sätt.

Hur man gör en tokenisering i PHP
---------------------

Vi behöver inte så mycket kunskap för att skriva vår egen tokenizer. I princip behöver vi bara känna till principen för reguljära uttryck och skriva ett litet analysobjekt.

I den här artikeln har jag förberett en grundversion av en tokenizer som bygger på Latte (Nette) tokenizer. Författaren till den ursprungliga implementeringen är David Grudl, som jag vill tacka för en så enkel funktion som löser alla problem åt dig.

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
		'matris' => 'matris',
		'<' => '\<',
		'>' => '\>',
		'{' => '\{',
		'}' => '\}',
		'eller .' => '\|',
		'lista' => '\[\]',
		'typ' => '[a-zA-Z]+',
		'utrymme' => '\s+',
		'kommatecken' => ',',
		'andra' => '.+?',
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

			throw new \LogicException(sprintf('Oväntad "%s" på rad %s, kolumn %s.', $token, $line, $col));
		}

		return $tokens;
	}
}
```

Den här tokenizern kan till exempel analysera en sådan komplex sträng (formatet är medvetet genomsyrat av mellanslag för att visa att tokenizern kan hantera ett stort antal fall):

```txt
array<int,  array<bool,    array<string, float>>  >
```
