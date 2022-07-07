如何编写你的第一个PHP脚本
==============

> id: '2d6986dc-f24b-4ae5-b24c-d5bb9bf94512'
> slug:
> 	cs: prvni-script
> 	zh: ru-he-bian-xie-ni-de-di-yi-gephp-jiao-ben
> 
> perex:
> 	- Jak napsat první PHP script? Úvod do PHP pro začátečníky.
> 	- 如何编写你的第一个PHP脚本？初学者的PHP介绍。
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: a5fab0e5edf99323140e497a6fe52322

在我们写第一个PHP脚本之前，我们首先需要从理论上解释如何用PHP加载一个页面。

1.首先，用户在其网络浏览器中调用一个特定的URL，例如`https://baraja.cz`。
2.然后，网络浏览器建立一个 "请求"，这是向网络服务器发出的一个特殊请求，被发送到互联网。它包含关于请求的页面、基本的浏览器识别和设置、关于cookies的信息等等。
3.这个 "请求 "在互联网上传播到一个网络服务器（最常见的是 "Apache"），它读取请求并开始编译一个响应。
4.由于被调用的URL是一个PHP脚本，并且请求的是一个名为`index.php`的文件，`Apache`从磁盘的根目录中读取`index.php`文件，并将其传递给`PHP解释器`，这是一个能够处理PHP代码本身并在此基础上建立`HTML代码`的程序，并将其发回给用户。
5.一旦HTML代码被编译，响应就会被送回给用户（这被称为 "响应"），网络浏览器会以正常的方式渲染页面，就像它是纯HTML一样。

> 注意，网络浏览器不会了解到任何关于PHP脚本的内容，而只是处理生成的HTML，所以你的脚本和服务器内容仍然是安全的。

让我们创建第一个脚本
----------------------------

> 编写你的第一个脚本时，假定你的计算机上有一个网络服务器在运行。对于Windows来说，<a href="https://www.apachefriends.org/index.html">XAMPP</a>是最好的（下载PHP7.0或更高版本），并且<a href="https://www.apachefriends.org/index.html">XAMPP</a>在Mac上的工作原理与Windows完全相同。对于Linux，我推荐<a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">LAMP服务器</a>（这个网站也在Lamp服务器上运行）。

PHP脚本文件的名称必须以`.php`扩展名结尾，这样Web服务器才知道我们要按照PHP规则来处理它。因此，让我们创建一个`index.php`文件，例如，它将包含我们网站的主页面的代码。

打开文件
--------------------------

在一个合适的文本编辑器中打开这个文件，用于编写源代码。

例如，在Windows中，<a href="https://www.sublimetext.com">Sublime Text</a>是一个很好的开始，因为它很好地给语法（语言规则）着色，使代码更容易阅读。后来，我建议购买<a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>，它在公司里用得很多，提供了在多人中编程的可能性。

编写一个HTML页面的基本结构
--------------------------

你可能已经知道HTML页面的基本结构。

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>

    </body>
</html>
```

所有的HTML代码都将以正常的方式处理，对设计网站有很大帮助。PHP在很大程度上使用了HTML和CSS的原理。

将PHP脚本与HTML代码分开
--------------------------

PHP主要是一种模板语言，在代码的适当位置生成自定义内容。为了能够清楚地分辨什么是HTML，什么是PHP，我们需要使用分隔符。

目前，最好使用`<?php`和`?>`的符号。

```php
// 这里是PHP代码
?>
```

> 如果我们想使用一些其他的HTML代码，我们就使用`?>`终止符。如果在PHP脚本的末尾没有更多的HTML代码，最好不要包括`?>`标签，这样就不会在页面的末尾出现不必要的留白和换行，让文本编辑器可以插入。

在过去，`<?`标签被经常使用，而不是`<?php`，但它可能不总是被支持。

封装标签可以放置在HTML代码的任何地方，例如页面的主体。

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>
    <?php
        // tady bude PHP kód
    ?>
    </body>
</html>
```

基本结构
--------------------------

其中非常基本的构件是。

- <a href="/echo">在源代码中列举内容</a>。
- <a href="/variable">Variables</a>。
- <a href="/if">条件</a>

在本节中，我们将演示使用变量将内容简单地列到源代码中。

源代码建设的原则
--------------------------------

所有的结构体（语言表达式）、语句和函数都用分号隔开，以使当前的结构体从哪里有效，到哪里就不明确了。

分号后面通常会有一个换行符。

象征性地写道。

```php
příkaz;
další příkaz;
proměnná x = její hodnota;

vypsat proměnnou x;

uložit do souboru;
```

提取到源代码
--------------------------

<a href="/echo">echo</a>结构被用来列出内容。

它非常容易使用。

```php
echo '你好，世界!';
```

然后，它将文本 "Hello world！"打印到HTML代码中。试试这个样本。

> 所有其他的演示将只包含PHP代码的内部。周围的HTML代码是自由的，你可以自己决定（例如，使用本文开头的样本）。

变量
--------------------------

变量是存储数据的虚拟内存位置，用于移动数据。变量的名称总是以 "元 "开头，后面是 "名称 "本身，然后是其 "值"。

> 我在<a href="/variable">关于变量的单独文章</a>中总结了变量工作的详细描述。

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo;
echo '<br>';
echo $jmenoAutora;
```

> 变量名称应表达变量实际包含的内容，使代码更清晰。还要注意插入HTML标签`<br>`来缩进文本。你应该已经熟悉了HTML中的这个标签。

在 "echo "结构中输出的东西被称为字符串（一个字符序列）。单个字符串可以用句号（`.`）连接起来，以减少输出到一个行。

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo . '<br>' . $jmenoAutora;
```

> 用点连接字符串后，整个东西将被视为一个大字符串。

可变业务
--------------------------

在变量之间，所有基本的<a href="/mathematics">数学运算</a>都能像预期的那样完全直观地工作。

让我们定义2个变量并将数字放入其中。

```php
$x = 5; //定义变量x的值为5
$y = 3; //定义了一个数值为3的变量y

echo $x + $y; // 添加变量并打印出8
```

> 请注意，等号（`=`）不是用来进行数学运算的，所以你不能写方程，比如说。在这方面，PHP只是像一个计算器一样行事。

如果我们不想使用变量，我们可以直接进行操作。所以操作的地点并不重要，他们会在任何地方进行评估。

```php
echo 5 + 3; //打印出8
```

另外，我们也可以将这些变量相加，并将结果存储在另一个变量中。

```php
$x = 5;
$y = 3;
$z = $x + $y; // 变量$z包含8

echo $z; //打印出8
```

在下一部分，我们将学习<a href="/principles-of-variables">变量定义和使用</a>的完整基础知识。
