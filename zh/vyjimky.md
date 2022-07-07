PHP中的异常情况和捕获它们
==============

> id: '61b0f178-bb1c-4166-9e8a-af49de2e2a8c'
> slug:
> 	cs: vyjimky
> 	zh: php-zhong-de-yi-chang-qing-kuang-he-bu-huo-ta-men
> 
> publicationDate: '2020-02-16 22:18:18'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: d843fe11a092943db429cb8a28384a31

异常是面向对象编程的工具，它提供了一种优雅的方式来抛出和处理（对待）应用程序错误。

异常首先被抛出（`thrown`），被处理（`try`），被捕捉（`catch`）。只有投掷是强制性的。

异常生成理念
-------------------------

在异常出现之前，编程中的错误处理是非常复杂的，因为我们必须依赖函数的返回值，并以自己的方式捕捉它们，并做出相应的行为。

事实上，函数本身并不执行错误处理，这可能会导致致命的问题，但David在<a href="https://phpfashion.com/programatori-chyby-neignoruji">Programmers don't ignore errors</a>中已经写到了这一点。

一个被遗忘的错误处理的例子。

```php
// 从磁盘到磁盘的移动
copy('c:/oldfile', 'd:/newfile');
unlink('c:/oldfile');
// 如果第一次操作失败，该文件将被不可挽回地删除。
```

这是因为处理`copy()`函数输出的正确方法是不继续，并抛出一个错误。在良好的旧功能的情况下，它可能看起来像这样。

```php
function backup(): bool
{
   if (copy('c:/oldfile', 'd:/newfile')) {
      return unlink('c:/oldfile');
   }

   return false;
}
```

我们的 "backup() "函数只有在 "copy() "函数没有失败且 "unlink() "函数返回 "true "时才会返回 "true"。否则，它将返回 "false"。

但现在这样做对应用来说是否安全？它不是!因为现在我们必须在调用`backup()`函数的时候对待它的输出，如果它失败了，我们甚至不知道原因。简而言之，它将返回 "false"，我们必须以某种方式自己检测错误。我想在这种情况下，看到程序员经常放弃错误处理，或者干脆忘记处理一些东西，而应用程序却因此抛出难以检测的错误，这是一件好事。

解决这个问题的方法是使用强制处理的异常，如果不处理，应用程序就会完全崩溃，我们总能找到原因。

基本的例外定义
--------------------------

在PHP中，异常是一种特殊的接口，由我们将要使用的本地`Exception`类实现。

如果程序的某些部分处理失败，我们只需抛出一个描述该问题的异常。

```php
if (copy('c:/oldfile', 'd:/newfile') === false) {
   throw new \Exception('不能复制文件 "oldfile"。');
}
```

抛出一个异常是通过`throw`关键字完成的，然后创建一个带有异常的类的实例。我们也可以通过其他方式获得一个实例（例如，从一个变量中传递），而仅仅创建一个异常实例并不会导致它被抛出。

`Exception'类构造函数的第一个参数接受异常的文本，它应该简明扼要地解释刚刚发生了什么。良好的做法是，也包括正在进行的操作的信息和对数据的参考。例如，如果文件复制失败，传递文件名是很好的做法。如果SQL查询执行失败，我们再次传递正在执行的查询。这对我们以后处理错误时有很大帮助，因为我们可以准确地看到问题所在。

异常处理
-----------------

例如，让我们有一个函数`backup()`来备份数据，并可能抛出一对错误。

```php
function backup(): void
{
   if (copy('c:/oldfile', 'd:/newfile')) {
      if (unlink('c:/oldfile') === false) {
         throw new \Exception('无法删除旧文件。');
      }
   }

   throw new \Exception('不能复制备份文件。');
}
```

注意，该函数不返回任何输出，我们在定义中指定了 "void "类型。该函数不需要返回任何东西，因为成功被认为是一个没有抛出错误的状态，我们不需要对待一个积极的情况。

如果我们在一个没有处理的应用中使用该函数，例如，如下所示。

```php
echo '备份文件...';
backup();
echo '备份完成。';
```

这是它的正常工作方式。然而，如果发生错误，脚本将自动终止，并在输出端显示异常文本。重要的是，它不会继续执行代码，我们知道不会发生数据损坏。

如果我们想继续执行，我们需要**清除错误，我们通过使用 "try "和 "catch "结构来做到这一点。

```php
echo '备份文件...';
try {
   backup();
} catch (\Exception $e) {
   echo '备份失败。' . $e->getMessage();
}
echo '备份完成。';
```

如果一个异常被抛出，`catch()`区域中的代码（如果符合其数据类型，则接受该异常）被调用，内部代码被执行。

我们总是得到一个异常类的实例，例如，它可以用来显示错误信息，这由`getMessage()`方法处理。了解`getFile()`方法也很有用，它返回包含错误的文件的磁盘路径，`getCode()`返回错误状态代码，`getLine()`返回抛出异常的行号。

准备好的例外情况
------------------------

除了基本的 `Exception` 异常之外，PHP还包括其他预定义的异常类型，适合于不同的使用情况。

| 数据类型 | 解释 |
|------------|-----------|
| `LogicException` | 逻辑错误，在程序设计时可以预测的
| `BadFunctionCallException` | 函数调用错误；未找到函数；不允许调用。
| `BadMethodCallException` | 方法调用错误 |
| `InvalidArgumentException` | 传递给函数或方法的错误（无效）参数 |
| `OutOfRangeException` | 数组或集合的值超出了范围 |
| `LengthException` | 值超过了允许的长度 |
| `DomainException` | 值不在所需的域或范围内
| `RuntimeException` | 只能在运行时检测到的错误（例如，未能写入文件） |
| 缓冲区或算术运算溢出，通常是由于处理的数据比预期的多而造成的。
| `UnderflowException` | 缓冲区或算术运算的下溢，传递的数据比预期的少 |
| `OutOfBoundsException` | 索引超出了数组或集合的范围 |
| `RangeException` | 数值不在要求的范围内 |
| `UnexpectedValueException` | 意外的值（如函数的返回值） |

应通过适当的程序设计来防止 "LogicException "和 "RuntimeException "异常。就我个人而言，我只在特殊情况下使用它们，例如无法写入文件和与外部服务通信。

我建议完全不捕捉`RuntimeException`，让应用程序失败。这通常是一个严重的问题，应尽快报告。
