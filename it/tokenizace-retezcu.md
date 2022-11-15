Codifica delle stringhe in PHP
==============================

> id: ba5b9c8d-9ba1-47df-b59f-da5131fdef10
> slug:
> 	cs: tokenizace-retezcu
> 	it: codifica-delle-stringhe-in-php
> 
> publicationDate: '2022-11-15 20:00:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '3e4b250cc549211cc3cd63f46cd6e33e'

Le espressioni regolari non possono essere utilizzate per gestire stringhe molto complesse che presentano una grammatica, come il codice sorgente di un linguaggio di programmazione, le annotazioni che descrivono i tipi di dati composti per i metodi, le espressioni matematiche, i calcoli, le formule e altro ancora. Il motivo è che si tratta di forme di stringa così complesse, contenenti molte regole, che dobbiamo semplicemente elaborarle in pezzi più piccoli.

Quando un computer elabora il codice sorgente PHP, ad esempio, lo scompone prima in tante piccole parti che hanno un proprio significato. Queste parti sono chiamate "tokens" e rappresentano i più piccoli blocchi autoconsistenti del linguaggio.

Il principio del parsing e della tokenizzazione di una stringa
--------------------------------------

Il principio dell'elaborazione delle stringhe e del linguaggio è suddiviso in diverse fasi:

- Nella prima fase, la stringa di origine viene letta carattere per carattere e i singoli token vengono cercati tramite espressioni regolari.
- Una volta trovato il primo token, la stringa viene troncata, il token viene memorizzato in un array e il parser continua.
- Quando si raggiunge la fine della stringa, sappiamo di aver costruito un array di token completo.
- Passiamo i token estratti alla funzione successiva, che ne gestisce l'elaborazione. In genere, si analizza token per token, si controlla la validità della grammatica e si elabora l'output man mano che si procede. Ad esempio, le variabili vengono sostituite, le condizioni vengono valutate e così via.

Un altro grande vantaggio di questo approccio è che conosciamo la posizione del token nella stringa (sia la riga che il carattere specifico di inizio e fine del token) man mano che scorriamo il token, in modo da poter affrontare con precisione la posizione del problema se viene lanciata un'eccezione.

Motivazione della tokenizzazione
--------------------------

Immaginate, ad esempio, di implementare un algoritmo per risolvere un esempio matematico. La matematica ha molte regole, come le priorità degli operatori, le parentesi, le chiamate di funzione e così via.

Se possiamo dividere la stringa di input in token elementari, possiamo lavorare con essa a un livello completamente diverso. Ad esempio, possiamo trovare facilmente le singole parentesi, sottrarre i token dalla parentesi iniziale a quella finale, passare una sottoespressione a una funzione ricorsiva per l'elaborazione e così via.

La tokenizzazione ci permette di risolvere in modo molto elegante anche problemi di parsing complessi.

Come tokenizzare in PHP
---------------------

Non abbiamo bisogno di molte conoscenze per scrivere il nostro tokenizer. In sostanza, è sufficiente conoscere il principio delle espressioni regolari e scrivere un piccolo oggetto di parsing.

Per gli scopi di questo articolo, ho preparato una versione di base di un tokenizzatore basato sul tokenizzatore Latte (Nette). L'autore dell'implementazione originale è David Grudl, che vorrei ringraziare per una funzione così semplice che risolve tutti i problemi.

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
		'o' => '\|',
		'elenco' => '\[\]',
		'tipo' => '[a-zA-Z]+',
		'spazio' => '\s+',
		'virgola' => ',',
		'altro' => '.+?',
	];


	/**
	 * @Ritorno array<int, Token>
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

			throw new \LogicException(sprintf('Inaspettato "%s" alla riga %s, colonna %s.', $token, $line, $col));
		}

		return $tokens;
	}
}
```

Questo tokenizer può analizzare, ad esempio, una stringa così complessa (il formato è volutamente intervallato da spazi per mostrare che il tokenizer può gestire un'ampia gamma di casi):

```txt
array<int,  array<bool,    array<string, float>>  >
```
