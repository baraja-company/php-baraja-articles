数据发送方法（GET和POST）。
=================

> id: '32f9083f-7cb1-469f-911a-765df059123d'
> slug:
> 	cs: metody-odesilani-dat
> 	zh: shu-ju-fa-song-fang-fa-get-hepost
> 
> perex:
> 	- 'Metoda GET a POST, získávání dat z formuláře a URL. Komunikace přes API a zpracování dat.'
> 	- GET和POST方法，从表单和URL检索数据。 API通信和数据处理。
> 
> publicationDate: '2019-11-26 11:38:32'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '81b5f92d7ee05563b6ece295ed5958d3'

除了常规变量外，我们在PHP中还有所谓的**超全局变量**，这些变量承载着当前调用的页面和我们传递的数据信息。

通常情况下，我们在一个页面上有一个用户填写的表单，我们想把这些数据传输到Web服务器上，在那里我们用PHP处理这些数据。

有3种最常用的方法来做到这一点。

- `GET`~数据作为参数在URL中传递。
- `POST`~数据与页面请求一起秘密传递。
- <a href="/ajax-post">Ajax POST</a> ~ 异步的javascript处理

GET方法 - `$_GET`。
--------------------

由GET方法发送的数据在URL中是可见的（作为问号后的参数），在Internet Explorer中最大长度为1024个字符（其他浏览器*几乎没有限制，但更大的文本不应通过这种方式传递）。这种方法的优点主要是简单（你可以看到你正在发送的内容），并且可以提供一个处理结果的链接。该数据被发送到一个变量。

接收页面的地址可能看起来像这样。

`https://____________.com/script.php?promenna=obsah&promenna2=obsah`。

在PHP中，我们可以，例如，将`variable`参数的值写成如下。

```php
echo $_GET['长廊'];	//打印出 "内容"
```

> **强烈警告：**这种直接向HTML页面写入数据的方法是不安全的，因为我们可以在URL中传递，例如，HTML代码，这些代码将被写入页面，然后执行。
>
> 我们必须***在任何输出到页面之前处理数据，`htmlspecialchars()`函数用于此。
>
> 例如：`echo htmlspecialchars($_GET['variable']);`

POST方法 - `$_POST`。
----------------------

由POST方法发送的数据在URL中是不可见的，这就解决了发送数据的最大长度问题。应该始终使用POST方法来发送表单字段，因为这将确保，例如，密码是不可见的，并且不能提供一个链接到处理特定输入结果的页面。

数据在`$_POST`变量中可用，用法与GET方法相同。

验证所发送数据的存在
--------------------------------

在处理任何数据之前，我们应该首先验证数据是否真的被发送，否则我们将访问
 到一个不存在的变量，这将抛出一个错误信息。

`isset()`函数是用来验证一个变量的存在。

```php
if (isset($_GET['命名'])) {
    echo '你的名字。' . htmlspecialchars($_GET['命名']);
} else {
    echo '没有输入名字。';
}
```

数据输入表
------------------------

该表格是用HTML制作的，不是用PHP制作的。它可以是在一个普通的HTML页面上。所有的 "魔法 "都由接受数据的PHP脚本处理。

举个例子，我们可以用一个表单来接收2个数字，通过GET方法发送。

```html
<form action="script.php" method="get">
    První číslo: <input type="text" name="x">
    Druhé číslo: <input type="text" name="y">

    <input type="submit" value="Sečíst čísla">
</form>
```

在第一行，你可以看到数据将被发送到哪里，用什么方法发送。

接下来的2行是简单的表单元素，注意**name=""**属性，那里是变量的名称，将保存现在表单中的内容。

接下来是提交数据的按钮（强制性的）和表单的关闭HTML标签（强制性的，以便浏览器知道还有什么要提交，什么不要提交）。

> 我们可以在一个页面上有任意数量的表单，而且它们不能嵌套。如果发生嵌套，最嵌套的形式总是被发送，其余的被忽略。

服务器上的表格处理
-------------------------------

现在我们有一个完成的HTML表单，我们把它发送到`script.php`，它使用GET方法接收数据。页面请求的地址可能看起来像这样。

`https://________.com/script.php?x=5&y=3`

**script.php**

```php
$x = $_GET['x'];	// 5
$y = $_GET['y'];	// 3

echo $x + $y;		//打印出8
```

正确的做法是，我们应该首先验证两个表单字段都已填写，这是用`isset()`函数完成的。

```php
if (isset($_GET['x']) && isset($_GET['y'])) {
    $x = $_GET['x'];	// 5
    $y = $_GET['y'];	// 3

    echo $x + $y;		//打印出8
} else {
    echo '该表格没有正确填写。';
}
```

> **TIP:**你可以向`isset()`结构传递多个参数，以验证它们是否都存在。
>
> 因此，代替`isset($_GET['x'])&&isset($_GET['y'])`，你可以直接指定。
>
> `isset($_GET['x'], $_GET['y'])`.

处理由POST方法收到的数据
--------------------------------------

如果数据是通过POST方法接收的，要处理的脚本的URL总是这样的。

`https://________.com/script.php`。

而绝不是其他。只是没有。这些数据隐藏在HTTP请求中，我们无法看到。

> 出于安全考虑，发送用户名和密码时需要使用隐藏的POST方法。
>
> **安全：**如果你在你的网站上使用密码，登录和注册表格应该托管在HTTPS上，你必须对密码进行适当的散列（例如，使用BCrypt）。

处理ajax请求
------------------------------

在某些情况下，在处理ajax请求时，可能不容易检索到数据。这是因为ajax库通常以`json payload`形式发送数据，而超全局变量`$_POST`只包含表单数据。

数据仍然可以被访问，我在<a href="/ajax-post">处理ajax POST请求</a>一文中描述了细节。

获得原始输入
-----------------------------

有时会发生这样的情况：用户使用不合适的HTTP方法发送请求，并在上面添加自己的输入。或者，例如，发送一个二进制文件，或坏的HTTP头。

在这种情况下，使用本地输入是很好的，在PHP中得到的输入如下。

```php
$input = file_get_contents('php://input');
```

在实现REST API库时，我还遇到了一些特殊情况，不同类型的网络服务器会错误地决定输入的HTTP头，或者用户会错误地提交表单数据，等等。

对于这种情况，我能够实现这个函数，它几乎解决了所有的情况（这个实现依赖于`Nette\Http\RequestFactory'，但你可以在你的具体项目中用其他东西替换这个依赖）。

```php
/**
 * 直接从HTTP头获取POST数据，或尝试从字符串中解析数据。
 * 一些传统的客户端以json的形式发送数据，而json是基本的字符串格式，所以字段转换为数组是强制性的。
 
 *返回数组<string|int, mixed>。
 */
private function getBodyParams(string $method): array
{
	if ($method === '争取' || $method === 'DELETE') {
		return [];
	}

	$request = (new RequestFactory())->fromGlobals();
	$return = array_merge((array) $request->getPost(), $request->getFiles());
	try {
		$post = array_keys($_POST)[0] ?? '';
		if (str_starts_with($post, '{') && str_ends_with($post, '}')) { //对传统客户的支持
			$json = json_decode($post, true, 512, JSON_THROW_ON_ERROR);
			if (is_array($json) === false) {
				throw new LogicException('Json不是有效的数组。');
			}
			unset($_POST[$post]);
			foreach ($json as $key => $value) {
				$return[$key] = $value;
			}
		}
	} catch (Throwable $e) {
		// 沉默是金。
	}
	try {
		$input = (string) file_get_contents('php://input');
		if ($input !== '') {
			$phpInputArgs = (array) json_decode($input, true, 512, JSON_THROW_ON_ERROR);
			foreach ($phpInputArgs as $key => $value) {
				$return[$key] = $value;
			}
		}
	} catch (Throwable $e) {
		// 沉默是金。
	}

	return $return;
}
```
