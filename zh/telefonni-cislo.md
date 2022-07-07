电话号码的验证和格式化
===========

> id: bbfe35c3-3a8c-4fe5-a9bd-5bff0fc234e0
> slug:
> 	cs: telefonni-cisla
> 	zh: dian-hua-hao-ma-de-yan-zheng-he-ge-shi-hua
> 
> publicationDate: '2021-06-18 09:20:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '224b3857a6bb898d9d322499cd1d3757'

在PHP中没有简单的方法来验证和格式化电话号码，所以我写了一个简单的库，没有依赖性，但仍然可以处理这个角色。

其目的是检查电话号码的格式，或将其转换为基本的规范形式（总是有效的）。

安装
---------

仅仅通过作曲家。

```txt
$ composer require baraja-core/phone-number
```

或者下载[Download on GitHub](https://github.com/baraja-core/phone-number)软件包。

如何使用图书馆
----------

这个工具的原理是基于格式化和验证来自用户的电话号码，或来自你无法控制的来源。

最常见的用途是纠正电话号码的格式。

```php
$original = '+420 777123456';
$formatted = \Baraja\PhoneNumber\PhoneNumberFormatter::fix($original);

echo $original . '<br>';
echo $formatted;
```

该函数修正了数字的格式，并返回字符串`+420 777 123 456`。

如果用户没有指定前缀，则假定使用`+420`前缀。你可以用第二个参数改变默认的偏好。

```php
//返回: +421 777 123 456
\Baraja\PhoneNumber\PhoneNumberFormatter::fix('+420 777123456', 421);
```

只有当用户没有输入电话代码，而电话代码又不能被自动检测到时，才会被改写。

输入和输出格式化
----------------------------

输入字符串可以看起来（几乎）任何方式。内置的算法可以自动删除非有效字符（例如，有些用户在电话号码旁边写了一个备注，这将被自动删除）。 因此，你完全不必担心输入的格式问题，但输出将始终是一致的。

输出看起来总是一样的（它被规范化为规范格式）。

一般的格式是。

```txt
   +420 777 123 456
     |  \_________/
  Prefix     |
      National number
```

如果你提交了一个无效的输入（或一个不能自动纠正的输入），将抛出一个异常。

错误诱捕
----------------

如果一个数字不能被安全地规范化为基本形式，或者不存在，则抛出一个`InvalidArgumentException`异常。

如果你想把异常转换为布尔值，请使用内置的资产验证器。

```php
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('123'); // 假的
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('777123456'); // true
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777123456'); // true
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777 123 456'); // true
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 77 712 34 56'); // true
```
