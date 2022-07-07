PHP中的纯函数
========

> id: d94c18a9-8bc4-4377-8042-e7c8b48320a2
> slug:
> 	cs: pure-funkce
> 	zh: php-zhong-de-chun-han-shu
> 
> publicationDate: '2021-10-27 10:30:00'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: e47a3507ddb189a218ad8e3a38703e5c

在函数式编程中，有一个**纯函数的概念，它指的是一个总是对相同的输入返回相同的输出的函数（即是确定性的），同时不受任何副作用的影响（即不影响其环境）。

纯粹的函数是什么样子的
----------------------

纯函数的例子。

```php
// 这是个纯函数
function add(int $a, int $b): int
{
	return $a + $b;
}
```

这是一个纯函数，因为根据输入参数，输出总是相同的。

什么不是纯函数
-------------------

```php
// 这是一个不纯的函数
function add(int $a, int $b): int
{
	echo '添加...';
	file_put_contents('file.txt', '价值。' . $a);
	return $a + $b;
}
```

这种类型的函数是不纯粹的，因为该函数改变了文件系统。另一种类型的不纯函数是当它与数据库进行交互，打印到屏幕上，等等。
