PHP中的正则表达式
==========

> id: '32142161-6ace-4cfd-b472-b48031219e9b'
> slug:
> 	cs: regex
> 	zh: php-zhong-de-zheng-ze-biao-da-shi
> 
> perex:
> 	- 'Regulární výrazy a jejich kompletní vysvětlení. Hledání podřetězce, pokročilé nahrazování a generování řetězců.'
> 	- 正则表达式和它们的完整解释。子串搜索，高级替换和字符串生成。
> 
> publicationDate: '2020-03-08 13:37:38'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '392c8f14e6943dd8f345a9a134e0de92'

正则表达式是允许你根据掩码（模式）轻松搜索、验证、比较、分割、折叠和替换字符串的工具。它是一个非常强大和优雅的高级字符串操作工具。

面罩
-----

在开始时，我们首先需要想出我们要执行的实际正则表达式。它被作为一个文本字符串输入，其中有一堆规则和配置选项（这是一个非常复杂的技术）。

对于初学者来说，需要注意的是，正则表达式是按顺序从左到右进行评估的，如果有多种解释字符串的方法，总是使用最大的可能的匹配（它的行为是**饥饿的，试图处理尽可能多的字符）。

正则表达式的行为和处理策略可以受到开关的影响，其中有许多开关。

简单验证该字符串是一个有效的电子邮件
-----------------------------------------------

我们怎样才能简单地检查字符串`jan@barasek.com`是否是一个有效的电子邮件地址，而不必将其分割成复杂的部分或逐个字符进行检查？

正则表达式给出了答案（上面的表达式是为了例子的目的而非常简化的，真正的电子邮件地址验证的实现应该是更复杂一些）。

```php
$mail = 'jan@barasek.com';
$regex = '/^.+@.+.(en|en|com)$/。';

if (preg_match($regex, $mail)) {
   echo '电子邮件是有效的';
} else {
   echo '电子邮件是无效的';
}
```

让我们更详细地检查一下表达式`/^.+@.+\. (en|en|com)$/`。

首先，我们需要用一对`/`字符（在开头和结尾）来包裹整个表达式，告诉表达式的开始和结束位置。表达式末尾的`/`后面是任何修饰语（表达式处理模式设置）。

在处理一个表达式时，你要从左边的字符开始逐个处理。每一种都有自己的含义，在下表中给出。

| 字符 | 意义 | 描述 | 示例 |
|------|-----------------|-------|-------------
| `^` | 字符串的起始点 | 强制要求字符串必须从这一点开始。 | 强制要求字符串必须以`+420`序列开始（例如对数字验证很有用）：`/^+420/`。
| `$` | 字符串或行的末尾 | 强制要求字符串或行必须在此结束。然后用`z`断言行的结束。<a href="https://phpfashion.com/vite-co-znamena-v-regularnim-vyrazu">详细解释</a>。 |文件名必须是一个文本文件（以句号结尾，然后是字符串 "txt"）：`//.txt$/`。 |
| `.` | 任何字符 | 准确捕捉任何字符。 | 验证字符串中是否正好包含一个任何字符：`/^.$/`。
| `d`|数字|检测字符`0-9`|检测不含空格且有9位数字的电话号码：`/^(\+420)?\d{9}$/`。
| `s` | 白色空间 | 捕捉空格、连字符和制表符。 | 允许电话号码中三位数之间的空格： `/^(\d{3}\s?){3}$/`。
| `+` | 多个字符，但至少有一个 | 重复前一个子表达式，并试图尽可能多地捕捉。子表达式必须至少重复一次。 | 抓取尽可能多的数字，但至少是一个。`/\d+/`. |
| `*` | 多个字符，可以是无字符 | 作用与`+`相同，但允许捕捉一个空字符串（数值不一定要存在）。 | 捕捉尽可能多的数字，如果没有，则捕捉一个空字符串：`/\d*/`。
| `(``)` | 括号 | 表示一个子表达式。这可以用来包围几个不同的标签，然后要求，例如，在它们上面进行重复，或者把它们的内容困在一个变量中。 | 让我们根据空格把邮政编码分成两部分，空格是可选的，甚至可以有多个： `/^(\d{3})\s*(\d{2})$/` |
| ``||||||||||||||||||||||||||||||||||||包含一个子表达式，或另一个子表达式。 |以`+420`或`+421`开头的数字：`/^+(420\|421)\s*\d+$/`。
| `\.` | 转义 | 如果我们想在表达式中捕捉一个字符，否则就会有特殊的含义，我们需要用与PHP中字符串相同的方式来转义它。 | 捕捉一对由句号分隔的数字（如果我们没有转义句号，它将被理解为 "任何字符"）： `/\d+\.\d+/`。

为了完整起见，我将给出Nette实现的电子邮件验证规则的完整形式。

```php
/**
 * 查找一个字符串是否是一个有效的电子邮件地址。
 */
public static function isEmail(string $value): bool
{
   $atom = "[-a-z0-9!#$%&'*+/=?^_`{|}~]"; // RFC 5322在本地部分没有引号的字符
   $alpha = "a-z\x80-xFF"; // IDN的超集
   return (bool) preg_match("(^
      (\"([ !#-[\\]-~]*|\\\\[ -~])+\"|$atom+(\\.$atom+)*)  # quoted or unquoted
      @
      ([0-9$alpha]([-0-9$alpha]{0,61}[0-9$alpha])?\\.)+    # domain - RFC 1034
      [$alpha]([-0-9$alpha]{0,17}[$alpha])?                # top domain
   \\z)ix", $value);
}
```

`preg_match()` - 通过模式进行验证
------------------------------------

格式验证和解析的基本函数是`preg_match()`，它有2个强制参数，第三个参数可以用来指定输出字段。

例子。

```php
$psc = '272 01'; // 克拉德诺

if (preg_match('/^(\d{3})\s*(\d{2})$/', $psc, $parser)) {
   echo '邮政编码是有效的[' . $parser[1] . ','. $parser[2] . ']';
} else {
   echo '邮政编码无效';
}
```

代码将返回："代码是有效的[272, 01]"。

注意单括号，我们用它来把表达式分成几个小部分。这样就有可能将各个子表达式作为数组条目获得。然后，整个函数根据字符串是否被成功捕获，返回 "true "或 "false"。

然而，有时候，浏览括号的数字顺序是非常具有挑战性的，因为数字可能会改变，或者可能只是有太多的括号。在这种情况下，可以单独给括号命名，然后用它们的名字访问键。

比如说。

```php
$phone = '777 123 456';

preg_match('/^(?<operator>d{3})/s*(?<number>[0-9 ]+)$/', $phone, $parser);

echo $parser['经营者']; // 返回 777
```

`preg_replace()` - 按模式替换
----------------------------------------

也可以使用regex替换字符串，这对于各种用户后期的格式修正特别有用。

假设我们想把用户输入的电话号码存储为一个整数，因为这是一个第三方库所要求的，但用户可以用一些相当疯狂的格式输入。

在这种情况下，我坚持使用这个口诀。

> "受之以厚，发之以严"。

这就是我们自动调整格式的原因。首先，我们使用解析法将字符串分解成各个部分，然后根据括号内的数字将其折回。

```php
function formatPhoneNumber(string $phoneNumber): int
{
   return (int) preg_replace(
      '/^(\+\d{3})\s*(\d{3})\s*(\d{3})\s*(\d{3})$/',
      '$2$3$4',
      $phoneNumber
   );
}

echo formatPhoneNumber('+420 777 123 456');
```

根据正则表达式构筑一个字符串
----------------------------------------

当根据复杂的模式生成新的字符串时，Regexes也有很大的意义。

纯粹的PHP没有对此的支持，但是我们可以下载一个第三方的<a href="https://github.com/icomefromthenet/ReverseRegex">ReverseRegex</a>库，可以做到这一点。

例如，我们可能想根据重词`[a-z]{10}`生成一组密码，没有什么可以阻止我们。

```txt
jmceohykoa
aclohnotga
jqegzuklcv
ixdbpbgpkl
kcyrxqqfyw
jcxsjrtrqb
kvaczmawlz
itwrowxfxh
auinmymonl
dujyzuhoag
vaygybwkfm
```

使用情况如下。

```php
use ReverseRegex\Lexer;
use ReverseRegex\Random\SimpleRandom;
use ReverseRegex\Parser;
use ReverseRegex\Generator\Scope;

require 'vendor/autoload.php';

$lexer = new  Lexer('[a-z]{10}');
$gen   = new SimpleRandom(10007);
$result = '';

$parser = new Parser($lexer, new Scope(), new Scope());
$parser->parse()->getResult()->generate($result, $gen);

echo $result;
```

例如，我在Presenter中用Nette生成我的数学例子，这种方式可以真正轻松地实现。

```php
public function actionRegex(): void
{
   $lexer = new Lexer('\d{1,3}[\+\-\*\/]');
   $parser = new Parser($lexer, new Scope(), new Scope());
   for ($i = 0; $i <= 10; $i++) {
      $result = '';
      $gen = new SimpleRandom($i);
      $parser->parse()->getResult()->generate($result, $gen);
      dump($result);
   }
   $this->terminate();
}
```

对库来说，重要的是它对相同的输入仍然产生相同的输出（尽管看起来对每个正则表达式有许多可能的字符串可以匹配）。如果我们想随机改变生成的表达式，我们也需要改变生成输出字符串的`种子'。遍历种子区间或者`rand(1, 1e6)`函数在这方面都很有用。

捕捉和处理错误
-----------------------------

在PHP中，抓取regexes中的错误是相当地狱的，但仍有一个解决方案。

这在David Grudel写的<a href="https://phpfashion.com/zradne-regularni-vyrazy-v-php">PHP中的可扩展正则表达式</a>一文中有详细的解释。
