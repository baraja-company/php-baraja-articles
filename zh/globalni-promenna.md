PHP中的全局变量
=========

> id: '1cdfc51a-31f4-4a5d-81f6-cfd92f86a9d4'
> slug:
> 	cs: globalni-promenna
> 	zh: php-zhong-de-quan-ju-bian-liang
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '31e78a9abf19252ef62315230a3731b5'

全局变量在任何时候都可以在应用程序的任何部分使用，不需要传递。

> **警告：**一个设计良好的应用程序不应该使用全局变量，因为它们违反了封装原则，如果处理不慎，会导致难以检测的错误。

使用实例。

```php
$a = 1;
$b = 2;

function suma(): void
{
	global $a, $b;
	$b = $a + $b;
}

suma();

echo $b; //打印数字3，因为$b变量是全局的。
```

请注意，我们已经在自然环境之外获得了变量`$a`和`$b`。这种行为被称为 "魔术"，因为如果另一个函数覆盖了当前正在使用的变量，应用程序就会出现意外情况。

正确的做法是，应用程序应该**封装**，每次都传递变量。

```php
$a = 1;
$b = 2;

function suma(int $a, int $b): int
{
	return $a + $b;
}

echo suma($a, $b); //打印出3
```

正因为如此，我们可以用不同的输入参数动态地调用该函数，其输出只取决于输入，而不取决于环境。

从URL中获取输入参数
---------------------------------

也许全局变量唯一合理的用途是在解析用户输入时，在这种情况下，我们谈论的是<a href="/superglobal-variable">superglobal变量</a>。

在这种情况下，这是一个简洁的设计，因为这个变量应该是只读的，而不是只写的，而且，它在整个应用程序中是相同的。

```php
function getNameFromUrl(): string
{
    return isset($_GET['名称'])
    	? htmlspecialchars($_GET['名称'])
    	: '';
}

echo getNameFromUrl();
```
