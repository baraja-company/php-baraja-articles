PHP Include - 在页面中插入一个文件
========================

> id: '7a53145c-8552-425d-b864-283f73a7a7de'
> slug:
> 	cs: include
> 	zh: php-include-zai-ye-mian-zhong-cha-ru-yi-ge-wen-jian
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '6e18e67ec03b3a7592473230559fc364'

`include`结构自动插入额外的文件/脚本到当前页面。

就 PHP 而言，它将被自动执行和理解，就像它一直在那个位置一样。

例子。

```php
include '新闻.html';
```

实际使用
-----------------

- 常见的菜单和功能表。
- 新闻、更新、新闻和相同的内容。
- 页眉或页脚，...

动态文件加载
--------------------------

我们经常需要动态地加载文件，例如基于一个变量。

比如说。

```php
$clanek = '颈椎病';

include $clanek . '.html';
```

或者我们可以动态地将文章加载到页面中。

```php
include '文章/' . $_GET['页'] . '.html';
```

当一个带有`page`参数的URL被调用时，文章被自动插入，例如：`https://example.com/?page=novinky`。
