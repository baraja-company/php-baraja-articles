PHP中的Cookies
============

> id: '392dd88b-d2f5-4943-a993-01aaad7ccd32'
> slug:
> 	cs: cookies
> 	zh: php-zhong-decookies
> 
> perex:
> 	- 'Cookies je krátká textová informace v paměti prohlížeče, kam můžeme v PHP zapisovat a označit si uživatele.'
> 	- Cookie是浏览器内存中的一小段文本信息，我们可以在PHP中写入并标记用户。
> 
> publicationDate: '2019-09-11 10:18:29'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '78f9aad0278acd51b98179e7d29fe3d8'

> **警告：**这篇文章是多年前写的，一些信息可能已经过时或不正确。阅读时请牢记这一点。

Cookies是存储在网站访问者的浏览器中的一小段文本信息。它们总是随着每个页面的再次加载而转移，并且可以被用户随时删除、改变和读取，因此它们不太适合存储个人信息。

*警告：如果您的网站使用cookies来跟踪用户或第三方插件（如Facebook的喜欢按钮，谷歌分析的流量表，广告横幅），您必须告知用户这一点。

> "还有一点要注意：在获得同意之前，你的网站不应包含广告或测量代码。而这很糟糕。"
>
>--<a href="https://phpfashion.com/jak-na-souhlas-s-cookie-ve-zkurvene-eu">David Grudl</a>

从cookie中读取一个值
--------------------------

所有的cookie都存储在超全局变量`$_COOKIE`中，该变量将每个键存储为一个数组。

例如，如果我们在cookie中的`user`键下存储了当前登录用户的名字，我们就可以很容易地检索到它。

```php
echo $_COOKIE['用户'];
```

请注意：Cookies可能并不总是存在（例如，如果你是一个新用户）。因此，我们应该在任何列表前始终检查是否存在cookies，并在必要时提供一个替代的错误信息。

```php
if (isset($_COOKIE['用户']) && $_COOKIE['用户']) {
    echo '登录的用户。' . $_COOKIE['用户'];
} else {
    echo '没有人签入。';
}
```

获取所有可用的cookies
--------------------------------

由于所有的cookie都存储在超全局变量`$_COOKIE`中，因此可以很容易地列出它们。

```php
var_dump($_COOKIE);
```

或者，也可以通过循环，获得所有的键和值。

```php
foreach($_COOKIE as $key => $value) {
    echo $key . ':' . $value;	// 写出键和值
    echo '<br>';				//包住这一行
}
```

在一个cookie中存储值
--------------------------

`setcookie()`函数用于在cookies中存储数据。

第一个参数是cookie密钥，用于从`$_COOKIE`字段中读取，第二个参数是数据本身的字符串。

通过第三个参数，我们可以（选择性地）设置cookie的有效期。可用的时间是以<a href="/date">时间戳</a>的形式给出的，所以如果我们想设置一个有效期为1小时的cookie，从这一刻开始，我们只需要写`time() + 3600`。

```php
$data = '我们想储存的一些内容。';

setcookie('测试Cookie', $data);
setcookie('测试Cookie', $data, time() + 3600);
```

存储更大的数据
-------------------

Cookie不适合存储较大的数据（浏览器通常只允许存储4 kB和最多20个Cookie，该大小还包括Cookie名称、有效性设置等）。

最好是在服务器上存储更大的数据，只在cookie中放一个标识符，通过它我们可以知道它属于哪个用户。这个方法叫做"$_SESSION"，将在另一篇文章中讨论。

如果你不一定需要在服务器上始终同步地存储数据，你可以使用javascript中可用的**<a href="https://jecas.cz/localstorage">localstorage</a>**存储。它的容量在MB的数量级，数据不会过期。
