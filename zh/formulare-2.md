表格，PHP中的表格处理
============

> id: d1871a8d-bcfe-408d-ac12-6b827633f77e
> slug:
> 	cs: formulare-2
> 	zh: biao-ge-php-zhong-de-biao-ge-chu-li
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '91f43ee06328e88a6a1058ecbf028222'

我想我们已经创建了一个HTML表单，我们把它发送出去，现在我们想处理数据。有一篇关于创建HTML表单的<a href="/formulare">单独的文章</a>。

接收数据--不同的方式
----------------------------

表格的发送方式是直接在HTML中设置的

有2个选项。

- **GET** - 它在问号后的地址栏中可见。
 例如：`php.baraja.cz/search.php?query=formulare`。
- **POST** - 隐藏（不可见），大多数表格都是通过邮寄发送的。

然后我们要用同样的方法在PHP中读取它们。

从用户那里获得数据并将其转移到脚本中去
------------------------------------------------------

基础是一个HTML表单，如何制作它，你可以在另一篇文章中阅读<a href="/formulare">。

对于初学者，让我们假设一个简单的表单来输入用户的名字。

```html
<form action="welcome.php" method="GET">
   Zadejte jméno: <input type="text" name="username">
   <input type="submit" value="odeslat">
</form>
```

将出现一个文本框，输入一个名称，然后点击提交。当按钮被点击时，该字段的内容被发送到脚本`welcome.php`。

现在是对`welcome.php`文件的实际处理。

```php
$username = $_GET['帐号'];

echo '输入的名字是。' . $username;
```

注意特殊变量`$_GET`。这是一个**的超全局变量**，包含表单中的数据，可以作为一个数组来访问。

然而，这种解决方案的问题是，收到的数据是**不安全的，类似的形式很容易被攻击。例如，潜在的攻击者可以在字段中输入javascript代码而不是名字，这将被写入页面并执行。

因此，在将任何用户数据输出到HTML代码之前，我们必须始终对其进行消毒处理。

```php
$username = $_GET['帐号'] ?? '不详';

echo '输入的名字是。' . htmlspecialchars($username);
```

进一步处理
----------------

我们可以对收到的数据做任何事情，并像对待任何普通变量一样对待它。

例如，在两个字段中添加值。

```php
echo $_GET['x'] + $_GET['y'];
```

或保存到文件、数据库、电子邮件，...

以下函数在这方面很有用。

- <a href="/file-put-contents">file_put_contents</a> - 将数据保存到一个文件的函数
- <a href="/hashovani">MD5</a> - 校验和计算，例如用于密码
- <a href="/cookies">Cookies</a> - 将数据保存到**cookies**（网络浏览器内的小文件）。
