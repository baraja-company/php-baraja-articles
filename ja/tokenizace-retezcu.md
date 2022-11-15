PHP での文字列トークン化
==============

> id: ba5b9c8d-9ba1-47df-b59f-da5131fdef10
> slug:
> 	cs: tokenizace-retezcu
> 	ja: php-deno-wen-zi-lietokun-hua
> 
> publicationDate: '2022-11-15 20:00:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '3e4b250cc549211cc3cd63f46cd6e33e'

プログラミング言語のソースコード、メソッドの複合データ型を記述するアノテーション、数式、計算、計算式など、文法を持つ非常に複雑な文字列は正規表現では扱えません。なぜなら、これらは多くの規則を含む複雑な文字列形式であるため、単純に小さな塊で処理しなければならないからである。

例えば、コンピュータがPHPのソースコードを処理する場合、まずソースコードをそれぞれの意味を持つ多くの小さなパーツに分解します。これらの部品は「トークン」と呼ばれ、言語の最小の自己充足的な構成単位を表しています。

文字列のパースとトークン化の原理
--------------------------------------

文字列・言語処理の原理は、いくつかのフェーズに分けられる。

- 第一段階では、ソース文字列を一文字ずつ読み取り、正規表現で個々のトークンを検索する。
- 最初のトークンが見つかると、文字列は切り捨てられ、トークンは配列に格納され、パーサーは処理を続行します。
- 文字列の末尾に到達したとき、完全なトークン配列が構築されたことを知ることができる。
- 抽出されたトークンは次の関数に渡され、その処理が行われる。通常、トークン単位で構文解析を行い、文法の妥当性をチェックし、出力を処理しながら進めていく。例えば、変数の置き換え、条件の評価などです。

この方法のもう一つの大きな利点は、トークンを通過する際に、文字列中のトークンの位置（行と特定のトークン開始・終了文字の両方）がわかるので、例外が発生した場合に問題の場所に正確に対処できることです。

トークン化する動機
--------------------------

例えば、数学の例題を解くためにアルゴリズムを実装することを想像してください。数学には、演算子の優先順位、括弧、関数の呼び出しなど、いろいろなルールがあります。

入力文字列を要素トークンに分割できれば、まったく別の次元で作業ができるようになる。例えば、個々の括弧を見つける、最初の括弧から最後の括弧までトークンを引く、部分式を再帰的な関数に渡して処理する、といったことが簡単にできる。

トークン化によって、複雑な構文解析の問題でも非常にエレガントに解決することができるようになりました。

PHPでトークン化する方法
---------------------

独自のトークナイザーを書くには、そこまでの知識は必要ないのです。基本的には、正規表現の原理を知って、小さな解析オブジェクトを書けばよいのです。

今回は、この記事のために、Latte（ネット）トークナイザーをベースにした基本版を用意しました。オリジナルの実装の作者はDavid Grudlで、このようなシンプルな関数ですべての問題を解決してくれることに感謝したい。

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
		'勢揃い' => '勢揃い',
		'<' => '\<',
		'>' => '\>',
		'{' => '\{',
		'}' => '\}',
		'または' => '\|',
		'リスト' => '\[\]',
		'タイプ' => '[a-zA-Z］+',
		'空間' => '\s+',
		'カンマ' => ',',
		'こと' => '.+?',
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

			throw new \LogicException(sprintf('行 %s、列 %s で予期しない "%s" が発生しました。', $token, $line, $col));
		}

		return $tokens;
	}
}
```

このトークナイザーは、たとえばこのような複雑な文字列を解析することができます（この書式は、このトークナイザーが広い範囲のケースを処理できることを示すために、意図的にスペースを散りばめています）。

```txt
array<int,  array<bool,    array<string, float>>  >
```
