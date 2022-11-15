Tokenizácia reťazcov v PHP
==========================

> id: ba5b9c8d-9ba1-47df-b59f-da5131fdef10
> slug:
> 	cs: tokenizace-retezcu
> 	sk: tokenizacia-retazcov-v-php
> 
> publicationDate: '2022-11-15 20:00:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '3e4b250cc549211cc3cd63f46cd6e33e'

Regulárne výrazy sa nedajú použiť na spracovanie veľmi zložitých reťazcov s gramatikou, ako sú zdrojové kódy programovacích jazykov, anotácie popisujúce zložené dátové typy metód, matematické výrazy, výpočty, vzorce a ďalšie. Dôvodom je, že ide o tak zložité reťazcové formuláre obsahujúce mnoho pravidiel, že ich jednoducho musíme spracovať po menších častiach.

Keď počítač spracováva napríklad zdrojový kód PHP, najprv ho rozdelí na mnoho malých častí, ktoré majú svoj vlastný význam. Tieto časti sa nazývajú "tokeny" a predstavujú najmenšie samostatné stavebné bloky jazyka.

Princíp rozboru a tokenizácie reťazca
--------------------------------------

Princíp spracovania reťazcov/jazykov je rozdelený do niekoľkých fáz:

- V prvej fáze sa zdrojový reťazec číta znak po znaku a jednotlivé tokeny sa vyhľadávajú pomocou regulárnych výrazov.
- Po nájdení prvého tokenu sa reťazec skráti, token sa uloží do poľa a analyzátor pokračuje.
- Po dosiahnutí konca reťazca vieme, že sme vytvorili kompletné pole tokenov.
- Extrahované tokeny odovzdáme ďalšej funkcii, ktorá ich spracuje. Zvyčajne analyzujeme token po tokene, kontrolujeme platnosť gramatiky a spracovávame výstup za pochodu. Napríklad sa nahradia premenné, vyhodnotia sa podmienky atď.

Ďalšou veľkou výhodou tohto prístupu je, že poznáme pozíciu tokenu v reťazci (riadok aj konkrétny začiatočný a koncový znak tokenu), keď prechádzame tokenom, takže v prípade vyhodenia výnimky môžeme presne určiť miesto problému.

Motivácia k tokenizácii
--------------------------

Predstavte si napríklad, že implementujete algoritmus na riešenie matematického príkladu. Matematika má veľa pravidiel, ako sú priority operátorov, zátvorky, volania funkcií atď.

Ak dokážeme vstupný reťazec rozdeliť na elementárne tokeny, môžeme s ním pracovať na úplne inej úrovni. Môžeme napríklad jednoducho nájsť jednotlivé zátvorky, odčítať tokeny od počiatočnej zátvorky po konečnú, odovzdať podvýraz rekurzívnej funkcii na spracovanie a podobne.

Tokenizácia nám umožňuje veľmi elegantne riešiť aj zložité problémy parsovania.

Ako tokenizovať v PHP
---------------------

Na napísanie vlastného tokenizéra nepotrebujeme toľko znalostí. V podstate nám stačí poznať princíp regulárnych výrazov a napísať malý objekt na parsovanie.

Pre účely tohto článku som pripravil základnú verziu tokenizéra založenú na tokenizéri Latte (Nette). Autorom pôvodnej implementácie je David Grudl, ktorému by som chcel poďakovať za takú jednoduchú funkciu, ktorá rieši všetky problémy za vás.

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
		'pole' => 'pole',
		'<' => '\<',
		'>' => '\>',
		'{' => '\{',
		'}' => '\}',
		'alebo' => '\|',
		'zoznam' => '\[\]',
		'typ' => '[a-zA-Z]+',
		'priestor' => '\s+',
		'čiarka' => ',',
		'iné' => '.+?',
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

			throw new \LogicException(sprintf('Neočakávaný výraz "%s" na riadku %s, stĺpec %s.', $token, $line, $col));
		}

		return $tokens;
	}
}
```

Tento tokenizér dokáže analyzovať napríklad takýto zložitý reťazec (formát je zámerne popretkávaný medzerami, aby sa ukázalo, že tokenizér dokáže spracovať veľký rozsah prípadov):

```txt
array<int,  array<bool,    array<string, float>>  >
```
