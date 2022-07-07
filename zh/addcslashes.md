添加斜杠
====

> id: '48278937-b1c5-479b-bac2-9b1ec552df4c'
> slug:
> 	cs: addcslashes
> 	zh: tian-jia-xie-gang
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1f53fe14dd5fbf79958c645d15c4d204'

支持PHP4、PHP5

`addcslashes` - C风格的斜线字符串

描述
--------------------------

```php
string addcslashes (string $str, string $charlist)
```

返回一个在charlist参数中指定的字符前带有反斜线的字符串。

参数
--------------------------

**str** 文本字符串

**charlist**

要删除的字符。如果charlist包含字符`n`，`r`，和其他字符，它们将被转换为C-style。其他长度小于32和大于126的非字母数字ASCI字符的变化。

当你在charlist参数中定义一个字符序列时，确保你知道你把什么字符作为范围的开始和结束。

```php
echo addcslashes('foo[ ]。', 'A..z');

// 价值：\f\o\o[ \]。
// 移除所有小写和大写字母
```

返回值
--------------------------

返回修改后的字符串。

例子
--------------------------

```php
$escaped = addcslashes($not_escaped, "\0..\37!@\177..\377");
```

charlist `0...\37!@\177...\377`，删除所有ASCII码在0到31之间的字符。
