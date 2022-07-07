超全球变量
=====

> id: bc2107ee-6551-4559-8c02-9cecdf98d33b
> slug:
> 	cs: superglobalni-promenna
> 	zh: chao-quan-qiu-bian-liang
> 
> publicationDate: '2019-11-01 09:29:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '792e217f1331f38ed0719d7abeb05e67'

超全局变量用于传递全局应用状态和HTTP通信。

这些变量的主要优点是，它们随时随地都可以使用。在实践中，它们是数值的数组，我们通过索引访问特定的信息。在不同的情况下，钥匙的可用性可能有所不同（解释如下）。

超全局变量的类型
--------------------------------

在PHP中所有的超级球体都是数组，用美元符号和下划线表示（除了`$GLOBALS`），并使用大写字母。

在 "PHP 7 "中，特别是有以下内容。

| 变量 | 说明 |
|-------------|-------|
| `$_GET` | URL参数<a href="/methods-odesilani-dat">由GET方法发送</a>。
| `$_POST` | 表格数据<a href="/methods-odesilani-dat">通过POST发送</a>。请注意，<a href="/ajax-post">在ajax中可能会有不同的表现</a>。
| `$_REQUEST` | 由任何方法（`$_GET`、`$_POST`和`$_REQUEST`）发送的表单数据。
| `$_FILES` | 关于当前上传文件的技术信息，例如通过`<input type="file">`结构。
| `$_SERVER` | <a href="/info">网络服务器设置</a>、IP地址、配置......它因环境而异（当从终端调用一个PHP脚本时，它将包含不同的值，例如，关于当前请求的信息将被遗漏）。
| `$_COOKIE` | 配置了<a href="/cookies">cookies</a>。
| `$_SESSION` | 会话数据（<a href="/sessions">session</a>），如果它存在并且在过去被设置过。
| `$GLOBALS` **警告，名称中不包含下划线！**这是所谓的<a href="global-variable">global-variable</a>，是关键词`global'的替代符号。如果你的应用程序中有一个全局变量`$variable`，你也可以用`$GLOBALS["variable"]`结构访问它。然而，使用全局变量在设计上是一个糟糕的、不纯净的解决方案，所以你最好不要这样做。
| `$_ENV` | 有关当前运行 PHP 的环境的信息。

列出所有现有的价值是很容易做到的。

```php
foreach ($_SERVER as $key => $value {
	echo $key . ':' . $value . '<br>';
}
```

> 注意：并非所有的索引都必须始终存在（例如，如果脚本在CLI模式下运行cron，带有页面URL或请求的IP地址的索引将不存在）。

访问变量
-------------------

我建议所有的全局变量（除了`$_SESSION`）都是只读的。这是因为它们包含全局应用数据，其他代码可能会考虑到这一点（例如，另一个已安装的库）。

全局状态的另一个缺点是，你不能总是依赖精确的值，即使它们存在，所以你应该总是用`isset()`结构检查它们的键。

要保存一个新的cookie，请使用`setcookie()`，不要直接插入值。这是因为它是只读的。

汲取的经验教训
-------

千万不要盲目相信超全局变量的值！这是不可能的。

用户可以使用URL和发送的头文件来影响数值的设置。所有的输入都应该总是被仔细验证。

注册球状物--旧版PHP的麻烦
------------------------------------------

在旧版本的PHP中（直到`5.4.0`），有一个特殊的`register-globals`指令（可在`php.ini`中配置），使URL中所有传递的参数都自动注册为变量。

比如说。

一个用户来到了URL：`https://example.com/script.php?var=24`。

而PHP在脚本中自动创建了一个变量`$var`，其值为`24`。

所以它的工作原理是经典的。

```php
echo $var;
```

因此，任何人都可以把任何变量塞进脚本并改变其内容。显然，安全并不总是一个优先事项。并非如此。

其他来源
------------

关于更详细的描述，请参阅<a href="https://www.php.net/manual/en/language.variables.superglobals.php">官方手册</a>。
