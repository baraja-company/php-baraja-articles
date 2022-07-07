Htmlspecialchars
================

> id: '46f04729-3956-4889-bb40-58362cb46b2a'
> slug:
> 	cs: htmlspecialchars
> 	zh: htmlspecialchars
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0743dd02-12d0-4766-ba42-8fd7e9c4ae8a'
> sourceContentHash: '566e10c57527a45f32906beb6c21430f'

Htmlspecialchars()是一个将特殊字符转换为HTML实体的函数。

描述
-----

```php
$promenna = htmlspecialchars($text);
```

一些特殊的字符对浏览器有特殊的意义，所以它们应该被转换为实体。这可以防止一般的脚本安全，防止页面被错误地呈现。

它最常被用来保护表格和任何用户插入文本的地方，并有可能插入HTML标签。

| 字符 | 注释 | 更改为
|------|-------------------------|-----------
| `&` | 逗号 | `&amp;`
| `"` | 双引号（当`ENT_NOQUOTES`被禁用时就会改变） | `&quot;`
| `'` | 撇号（当启用`ENT_QUOTES'时将会改变） | `&#039;`
| `<` | 小于，HTML括号 | `&lt;`
| `>` | 大于，HTML括号 | `&gt;`

参数
--------

**要转换的**字符串

**标志**不同的行为设置

**charset** 指定字符集（编码）。默认的字符集是`ISO-8859-1`。

你可以使用`ISO-8859-1`，`ISO-8859-15`，`UTF-8`，`cp866`，`CP1251`，`CP1252`，和`KOI8-R`。

> 注：只支持PHP 4.3.0及以后的版本。任何其他字符集都不被识别和支持。

**double_encode** 当`double_encode`被禁用时，PHP将不会对现有的HTML实体进行编码，默认是转换所有内容。

返回值
-----------------

转换字符串。

如果字符串包含无效的单位，在`ENT_IGNORE`（未设置）中给定的字符集内，将返回一个空字符串。

版本的变化
----------------

| 版本 | 备注
|-------|---------
| 5.4.0 | 增加常量`ENT_SUBSTITUTE`、`ENT_DISALLOWED`、`ENT_HTML401`、`ENT_XML1`、`ENT_XHTML`和`ENT_HTML5`。
| 5.3.0 | 增加了`ENT_IGNORE`常量。
| 5.2.3 | 增加`double_encode`参数。
| 4.1.0 | 增加`charset`参数。

例子
-------

```php
$new = htmlspecialchars(
	'<a href="test">测试</a>',
	ENT_QUOTES
);

echo $new; // <a href="test">测试</a>
```
