PHP关键词
======

> id: d58620d7-57a0-410a-ba73-31a1f9d984fb
> slug:
> 	cs: keywordy-jazyka-php
> 	zh: php-guan-jian-ci
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '38fbdb53b4455903f3300c3c8b4a8ad2'

几乎所有的编程语言都由 "关键词 "组成，它们是具有特殊意义的语言表达。

关键词的一个例子是 "string"、"if "或 "echo"。

需要注意的是，关键字（有时也是`命令'）<a href="/commands-and-functions">不是函数</a>，因此，例如`echo'也不是一个函数。

关键字的列表很好理解，因为它们在 PHP 中有特殊的含义，不能总是用于所有的事情。例如，一个类的名称不能与现有的某个关键词相同。

解析错误的例子
-------------------

当试图处理一个名为 "String "的类时，会发生一个 "解析错误"，因为PHP不知道它是一个类名还是一个数据类型。

这是不对的。

```php
class String
{
   // ...
}
```

关键字列表
-------------------

更新了 PHP 7.1 的关键词列表。

`and`, `or`, `xor`, `for`, `do`, `while`, `foreach`, `as`, `return`, `die`, `exit`, `if`, `then`, `else`, `elseif`, `new`, `delete`。`try`, `throw`, `catch`, `finally`, `class`, `function`, `string`, `array`, `object`, `resource`, `var`, `bool`, `boolean`, `int`。`整数`，`浮点`，`双数`，`实数`，`字符串`，`数组`，`全局`，`静态`，`公共`，`私有`，`保护`，`公布`。延伸", "切换", "true", "false", "null", "void", "this", "self", "struct", "char", "signed", "unsigned", "short", "long
