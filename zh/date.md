PHP函数date()，日期和时间
=================

> id: '9b0ec1c7-3431-4d7d-9bcc-6093285f14f1'
> slug:
> 	cs: date
> 	zh: php-han-shudate-ri-qi-he-shi-jian
> 
> perex:
> 	- 'Zjištění data a času, aktuální datum, formátování data a času a převod tvaru.'
> 	- 日期和时间检测、当前日期、日期和时间格式化以及形状转换。
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: d0323ba88fcdba84ef45439991301f6c

`date()`函数是一个用于处理日期和时间的工具。它在两种情况下使用。

- **查找当前状态**，即当前日期、时间......。并以特定格式输出。
- **将一个特定的日期转换为另一种格式（例如，月号到名称，年份符号格式，12和24小时系统，...）。

样品
------

```html
Já jsem speciální stránka. Vím, že právě je
<?php
    echo date('H:i'); // Hodina:minuta
?>
```

> 警告：PHP不会打印你的时间，而是服务器上的时间。因此，你可能会得到一个与你的电脑上设置的时间不同的时间。

输入语法
--------------------------

该函数以正常方式被调用，各个请求被作为函数参数输入。

```php
echo date('格式化标记', atribut konkrétního času);
```

格式化标记表明将以何种格式打印日期。标记可以包括空格、句号、冒号、连字符和其他本身不是格式化标记的字符（如果你想使用格式化标记，你必须<a href="/carriage-notes">取消它</a>）。下面是每个标签的概述。

第二个（可选）属性表示手动输入的日期或时间，它将按照第一个参数的格式进行转换和输出。它必须被指定为*时间戳*（可以通过格式化标签 "U "获得）。

例子。

```php
echo date('d. m.Y', 1405856605); //在20.07.2014之前
```

允许的格式化标记表
--------------------------

| 角色 | 描述
|------|---------------------
| `Y` | 年份为四位数（如1998年）。
| `y` | 年份为两位数（如98）。
| `M` | 月份名称的英文缩写（例如：Jan）。
| `m` | 月号 (01-12)
| `F` | 英文月份名称（例如：一月）
| `D` | 星期的英文缩写（如：Fri）。
| `l` | 一周的英文名称（例如：星期五）。
| `N` | 一周的天数 (1 - 星期一, 7 - 星期日)
| `w` | 一周的天数（0-周日，1-周一，6-周六）。
| `d` | 本月的日子 (01-31)
| `j` | 每月的天数（1-31）。
| `z` | 年月日(001-365)
| `H` | 小时 (00-23)
| `h` | 小时 (01-12)
| `i` | 分钟 (00-59)
| `s` | 第二 (00-59)
| `U` | *时间戳：*从时间开始的秒数（从1970年1月1日起）。
| `S` | 月的序数的英文结尾
| `A` | AM/PM指标
| `a` | 上午/下午指标（上午/下午）。
| `P` | 与格林威治时间（GMT）的差异，在小时和分钟之间有分隔符（在PHP 5.1.3中加入），例如：`+02:00`。
| `g` | 12小时格式的小时（1-12）。
| `G` | 24小时格式的小时(0-23)

网站地图的时间格式化
---------------------------------

很多时候，你需要对`sitemap.xml`文件的时间进行格式化，该文件包含`<lastmod>`标签中最后一次修改的日期和时间。

从 PHP 5.1.3 起，可以用以下语法来做这件事。

```php
date('ǞǞ ǞǞ');
```

日期和时间将显示到最接近的第二位，还将包括服务器所在的时区信息（时区由服务器的操作系统决定）。

获取捷克的日和月的名称
----------------------------------

在PHP中不可能以正常方式获得日和月的捷克语名称，所以我们必须自己写出这样的值。最好的方法是将条目存储在一个数组中，并使用索引调用来检索它们。

```php
$mesice = [
    1 => '一月', '2月', '三月', '四月', '5月',
    '6月', '七月', '八月', '9月', '10月',
    '11月', '12月'
];

$dny = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
```

这个例子是<a href="https://php.vrana.cz">***Jakub Vrana***</a>一文中的简化例子，<a href="https://php.vrana.cz/ceske-nazvy-mesicu-a-dnu-v-tydnu.php">捷克的月份和星期的名称</a>。

将日期从英语翻译成英文
--------------------------------------

我们经常有一个格式为 "9月13日，星期五 "的日期，我们想把它翻译成英文，但如何做呢？最好的方法是调用一些函数，例如`datumcesky()`，我们把英文日期传给它，它就会翻译出来。

```php
function datumCesky(string $date): string
{
    $men = [
        '一月', '2月', '三月', '四月', '5月',
        '6月', '七月', '八月', '9月', '10月',
        '11月', '12月'
    ];

    $mcz = [
        '一月', '2月', '三月', '四月', '5月',
        '6月', '七月', '八月', '9月', '10月',
        '11月', '12月'
    ];

    $date = str_replace($men, $mcz, $date);

    $den = [
        '星期一', '星期二', '星期三', '星期四',
        '星期五', '星期六', '星期日'
    ];

    $dcz = [
        '星期一', '星期二', '星期三', '星期四',
        '星期五', '星期六', '星期日'
    ];

    return str_replace($den, $dcz, $date);
}
```

使用实例。

```php
echo datumCesky('9月13日，星期五'); // 12月13日，星期五
```

这个功能绝对不是理想的翻译，因为它只是将英语单词替换成捷克语，但对于许多部署来说可能已经足够。对于更高级的翻译，你应该始终保证准确的语法，以翻译成统一的风格，例如：`12月13日，星期五'。

寻找每月的第一天
-----------------------------

2018年4月的第一天是个星期天，但如何轻松找到？

从 PHP 5.1.0 开始，有了一个简单的解决方案。

```php
echo date('N', strtotime('2018-04-01')); // 1（星期一），7（星期日）。
```

在旧版本中，我们必须自己实现这个功能。

```php
/**
 * @作者Jan Barášek
 */
function getFirstDayPosition(?int $year = null, ?int $month = null): int
{
    $day = (int) date('w', strtotime($year . '-' . $month . '-1')) - 1;

    return $day < 0 ? 7 : $day + 1;
}
```

由于`w`修饰符以美国格式返回输出，我仍然通过简单的计算来调整日数。该函数返回1（星期一）和7（星期日）之间的一个整数。

时间偏移/格式转换和日期验证
--------------------------------------------------

通常我们需要跳过一个相对的时间（比如说+5天），或者从用户的文本输入中提取一个日期，使其有效。要做到这一点，<a href="https://www.php.net/manual/en/function.strtotime.php">strtotime()</a>函数允许以下语法。

```php
echo strtotime('现在');
echo strtotime('2000年9月10日');
echo strtotime('+1天');
echo strtotime('+1周');
echo strtotime('+1周2天4小时2秒');
echo strtotime('下周四');
echo strtotime('上星期一');
```

该函数的输出是**时间戳** *（世界时间）*的指定时间的整数。

> 一般来说，我建议在所有我们以某种方式与用户的时间打交道的表单上部署这个功能。当然，强迫用户以任何特定的格式来写日期不是一个好主意，但总是自动创建这样的格式，这样用户就可以写他们想要的东西。因为通常情况下，比如说，文本是从某个地方复制过来的，如果能自动完成的话，手动重新格式化就太麻烦了。

从时间开始计算的秒数
--------------------------

自1970年1月1日起，每一秒都被加到1，得到一个巨大的整数，表示从那时起已经过去的时间。这对于简单计算时间差特别有用。这是一个相当可靠的解决方案，但它也有风险（2038年的问题）。

**此符号的一般属性：**
- 它是一个整数，所以很容易操作。
- 它采用十进制系统，所以人类很容易读懂（除了你无法迅速判断准确的时间）。
- 它不能用来代表1970年1月1日之前的时期，因为它不能是负数*（一些新版本的算法实现了负数时间，但你不能总是依赖这个）*。
- 在2038年，它将停止在32位计算机上工作，并会导致程序崩溃（由于堆栈溢出和最大整数大小）。

2038年的问题
--------------------------

<h2 id="universalTime" style="background: #222; margin-top: 32px; padding: 32px; color: white; text-align: center; border-radius: 3px;">我不读了...</h1>。

<script>
	setting(function() {
		window.document.getElementById('universalTime').innerHTML = Math.floor(Date.now() / 1000)。
	}, 250);
</脚本>

> 当前的时间值显示在这个文本的上方，每秒钟递增+1。当前值是由javascript加载的，所以它不一定准确（它表示你的计算机的系统时间）。

计算机以二进制形式存储整数，如果是整数，它通常有32位（32个1和0）。可能的最高32位数是：`011 111 111 111 111 111 11`。

然而，如果我们每秒向这个常数添加+1，我们总有一天会得到这样一个数字*（将是2038年1月19日的03:14:07）*。然而，当我们试图再加一个1时，比特范围将不再足够（数字将大于32比特可以存储的范围），因此将出现**栈溢出**，即一个1将被添加到数字的开头，在二进制形式中意味着一个**负数**，（因此这可能会导致程序崩溃），让我们在1901年的某个地方（唉！）。

这个问题有两个解决方案。

- 将整数范围从32位扩展到64位，给我们一个几千年的跨度
- 使用`DateTime`数据类型（通常在PHP中可用），它提供了一个面向对象的日期管理方法，与数据库很好地兼容，并提供了从`0001`到`9999`的年份范围，这足以满足大多数现实世界应用的需要。

我个人主张使用`DateTime`数据类型，完全不使用整数存储。

不同的时区
-----------------

PHP是智能的，所以它总是试图使用服务器所在的当前最佳时区。然而，有时可能会发生时区指定不正确的情况，或者我们正在编写一个全球性的应用程序，来自世界各地的用户访问它，因此我们需要手动改变时区。

这可以通过调用一个函数对整个PHP脚本进行全局处理。

```php
date_default_timezone_set('UTC');
```

偶尔，你也会遇到这种解决方案（检测到服务器有一个非UTC的时区）。

```php
$defaultTimeZone = 'UTC';

if (date_default_timezone_get() != $defaultTimeZone) {
    date_default_timezone_set($defaultTimeZone);
}
```

改变日期和时间
--------------------------

负责获取当前日期和时间的不是PHP本身，而是它所运行的操作系统。因此，如果你需要手动改变时间，就在那里改变。
