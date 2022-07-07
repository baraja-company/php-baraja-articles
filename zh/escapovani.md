在PHP中对字符串中的字符进行转义
=================

> id: '40f9361e-e286-4b5e-a0c0-1f6cda8106af'
> slug:
> 	cs: escapovani
> 	zh: zaiphp-zhong-dui-zi-fu-chuan-zhong-de-zi-fu-jin-xing-zhuan-yi
> 
> publicationDate: '2019-11-26 11:56:52'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '026284252541bc9c918a87298b9e261c'

逃逸是用来书写在不同语境中具有不同含义的字符。

例如，我们想在一个由引号括起来的字符串中插入另一个引号。如何做到这一点？

有2个选项。

```php
echo "李维斯的牛仔裤"; // 引号类型的组合

echo 'Levi/s牛仔裤'; // 反斜线转义
```

当把变量写入HTML模板时，转义也很重要，因为字符串内容可能处于不同的环境中，意味着一些特殊的东西。

因此，例如，当列出HTML代码（我们在一个变量中）时，我们需要处理列表，否则HTML代码将执行。

比如说。

```php
$message = '嗨，<b>汤米！</b>。';

echo $message; // 错了!

echo htmlspecialchars($message); // 对了 :)
```

逃亡的问题非常复杂，我建议阅读David Grudel的文章<a href="https://phpfashion.com/escapovani-definitivni-prirucka">逃亡--明确的指南</a>。
