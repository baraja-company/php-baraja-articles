PHP中的局部变量
=========

> id: '9c3fbf4e-8612-4599-8127-d66b9ec5e980'
> slug:
> 	cs: lokalni-promenna
> 	zh: php-zhong-de-ju-bu-bian-liang
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: cffee22c4dbcc2ec331b73235349c409

本地变量只在**<a href="/prikazy-a-function">function</a>**或**method**（在面向对象编程中）的主体内有效。

如果我们是在常规脚本的背景下工作，一切都会如期发生。

```php
$x = 5;

echo $x; //打印出5
```

但当我们定义一个自定义函数时，行为就会发生轻微变化。

```php
$x = 5;

function mojeFunkce(): int
{
    $x = 3;

    echo $x; //打印出3
}

echo $x;     //打印出5
```

原因是$x变量只存在于当前函数或方法的上下文中。这种行为是故意的。

如果我们想从周围的代码中传递一个值给一个函数，我们必须用必要的参数来调用它。

```php
echo mojeFunkce(5);	//打印出6

function mojeFunkce(int $x): int
{
    return $x + 1;
}
```

数值是用参数输入到函数中的。如果你想在参数之外向函数传递额外的变量，你可以使用<a href="/global-variable">global variables</a>，但这并不是一个好主意。

> 在对一个较大的应用程序进行编程时，使用局部变量会产生巨大的差异。如果我们不区分变量在不同语境下的有效性，就不可能保证一个变量不会在我们不指望它的地方被覆盖（这就是为什么全局变量是邪恶的）。

```php
$x = 5;
$y = 3;

function soucet(int $x, int $y): int
{
    return $x + $y;
}

echo $x;             //打印出5
echo soucet($x, $y); //打印出8
```
