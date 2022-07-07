建设内容包括
======

> id: '881cc6ef-b1dc-4a71-ab22-d1943ce8095b'
> slug:
> 	cs: include-function
> 	zh: jian-she-nei-rong-bao-kuo
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '53294e511809a77c08883100fd7416c6'

| 支持 | PHP 4, PHP 5, PHP 7
|---------------|---------
| 简要说明 | 将另一个文本文件或脚本附加到脚本中。
| 要求 | 其他要插入的文本文件或脚本。
| 注意 | 不能加载外部文件。

描述
--------------------------

在页面中插入另一个文本文件或脚本。支持普通/文本文件。

嵌入文件的行为就像它们直接在页面中一样。

嵌入式脚本会自动执行。

嵌入脚本转移变量的值。

> 无法从外部存储器加载。只能读取未经格式化的纯文本和PHP文件。

允许的路径格式。

- `script.php` - 同一目录下的文件，将被执行。
- `script.html` - 在同一目录下的文件，将不会被执行。
- `./file.php` - 同一目录下的文件，将被执行。
- `.../page.html`。
- `folder/file/file.php` - 在Windows中写入
- `address/DalsiAddress/file.php` - 在Unix系统上写入

`****`和`**/**`类型的斜线可以相互转换，所以你不必处理这个问题。

非法的路径条目：`https://domena.pripona/slozka/soubor.php`。

类似功能
--------------------------

- <a href="/file-get-contents">file_get_contents()</a>

例子
--------------------------

```php
include 'file.php';
```

它将`file.php`脚本插入到页面并运行。

`vars.php`。

```php
$color = '绿色';
$fruit = '苹果';
```

`test.php`。

```php
$color = '';
$fruit = '';

echo 'A' . $color . '' . $fruit; // A

include 'vars.php';

echo 'A' . $color . '' . $fruit; // 一个青苹果
```

> 警告：下面的符号是不可能的，通过定义变量来转移变量的内容。

```php
include 'file.php?parameter=neco';
```

返回值
--------------------------

没有，只是把文件放进去。

**注意：**允许你比较文件内容，但这是一个安全风险。括号的顺序很重要!例子。

```php
if ((include 'file.php') == '认可') {
    echo '其值为 "OK"';
}
```

> 这是一个潜在的安全风险!
>
> 可以用**file_get_contents()**、**readfile()**或**fopen()**解决。Fopen()应该只用于.txt和.html文件。
>
> 更安全的读取文件可以通过定义一个自定义函数来解决。

例子。

```php
$string = get_include_contents('somefile.php');

function get_include_contents(string $filename): ?string
{
    if (is_file($filename)) {
        ob_start();
        include (string) $filename;
        return ob_get_clean();
    }

    return null;
}
```

其他开发者的笔记和提示
--------------------------

**雅库布-弗拉纳**给我发了电子邮件。

```php
include '文章/' . $_GET['文章'] . '.html';
```

这是极危险的。

攻击者可以使用`.../`或类似的东西作为文章名称传递到另一个目录的链接，有时可以通过在结尾传递一个空字节来摆脱结尾。

你至少应该使用`basename()`函数，但最好只允许白名单的值。
