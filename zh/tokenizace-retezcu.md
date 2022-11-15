PHP中的字符串标记化
===========

> id: ba5b9c8d-9ba1-47df-b59f-da5131fdef10
> slug:
> 	cs: tokenizace-retezcu
> 	zh: php-zhong-de-zi-fu-chuan-biao-ji-hua
> 
> publicationDate: '2022-11-15 20:00:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '3e4b250cc549211cc3cd63f46cd6e33e'

正则表达式不能用来处理有语法的非常复杂的字符串，如编程语言源代码、描述方法的复合数据类型的注释、数学表达式、计算、公式等等。原因是这些复杂的字符串形式包含了许多规则，我们只需在较小的块中处理它们。

例如，当计算机处理PHP源代码时，它首先将其分解成许多小部分，这些小部分具有各自的意义。这些部分被称为 "令牌"，它们代表了语言中最小的自成一体的构建块。

解析和标记字符串的原则
--------------------------------------

字符串/语言处理的原理分为几个阶段。

- 在第一阶段，源字符串被逐个字符读取，并通过正则表达式搜索个别标记。
- 一旦找到第一个标记，字符串就会被截断，标记被存储在一个数组中，然后解析器继续。
- 当到达字符串的末端时，我们知道我们已经建立了一个完整的令牌阵列。
- 我们将提取的标记传递给下一个函数，由它来处理这些标记。通常情况下，我们逐个令牌进行解析，检查语法的有效性，并在输出时进行处理。例如，变量被替换，条件被评估，等等。

这种方法的另一大优势是，我们在浏览令牌时知道令牌在字符串中的位置（包括行和具体的令牌开始和结束字符），因此如果出现异常，我们可以准确解决问题的位置。

符号化的动机
--------------------------

例如，想象一下，你正在实施一种算法来解决一个数学例子。数学有很多规则，如运算符优先级、括号、函数调用等等。

如果我们能将输入的字符串分割成元素标记，我们就可以在一个完全不同的层面上处理它。例如，我们可以很容易地找到单个小括号，从最初的小括号减去最后的小括号，将一个子表达式传递给一个递归函数进行处理，等等。

标记化使我们能够非常优雅地解决甚至复杂的解析问题。

如何在PHP中进行标记化
---------------------

我们不需要那么多知识来编写我们自己的标记器。基本上，我们只需要知道正则表达式的原理并编写一个小的解析对象。

为了这篇文章的目的，我准备了一个基于LATT（Nette）标记器的基本版本。原始实现的作者是David Grudl，我想感谢他提供了这样一个简单的函数，为你解决了所有的问题。

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
		'阵列' => '阵列',
		'<' => '\<',
		'>' => '\>',
		'{' => '\{',
		'}' => '\}',
		'或' => '\|',
		'列表' => '\[\]',
		'类型' => '[a-zA-Z]+',
		'空间' => '\s+',
		'逗号' => ',',
		'其他' => '.+?',
	];


	/**
	 *返回数组<int, Token>。
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

			throw new \LogicException(sprintf('在第%s行，第%s列出现了意外的"%s"。', $token, $line, $col));
		}

		return $tokens;
	}
}
```

例如，这个标记器可以解析这样一个复杂的字符串（格式中特意穿插了一些空格，以显示标记器可以处理大范围的情况）。

```txt
array<int,  array<bool,    array<string, float>>  >
```
