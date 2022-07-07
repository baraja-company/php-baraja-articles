在PHP中处理ajax POST请求
==================

> id: c9c8fdb4-020d-4361-b425-4f4406a090ba
> slug:
> 	cs: ajax-post
> 	zh: zaiphp-zhong-chu-liajax-post-qing-qiu
> 
> publicationDate: '2019-11-01 09:56:02'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '89254da0b9b22b1926410b3ef7e511ec'

在开发ajax Vue.js应用程序时，多年后我终于发现如何在PHP中使用ajax以及如何通过POST方法接收数据。

超全局变量`$_POST`仅适用于表单
-------------------------------------------------------------

在PHP中，<a href="/superglobal-variable">superglobal变量`$_POST`</a>通常可用于保存表单中的提交数据。

它是***容易使用的。

在HTML方面，你需要创建一个表单。

```html
<form action="process.php" method="post">
    Jméno: <input type="text" name="username">
    <input type="submit" value="Odeslat">
</form>
```

然后在`process.php`文件中，这些值可以作为数组元素被访问。

```php
echo htmlspecialchars($_POST['帐号'] ?? '');
```

> **警告：**
>
> 通过这种简单的方法，大家可能会觉得POST的数据被自动定义为`$_POST`变量中的数组索引。但这是不正确的!

事实上，使用POST方法发送的数据之所以被写入`$_POST'变量，是因为浏览器在提交HTML表单时自动发送HTTP头`'内容类型'：'application/x-www-form-urlencoded'。

如果没有一个正确设置的头，这些值根本无法被访问，我们不得不使用一个棘手的解决方案。

通过Ajax发送数据
-------------------

当试图使用ajax发送数据时，我们需要在PHP方面改变一下方法。你可以<a href="https://www.facebook.com/groups/frontendisti/permalink/2372671669611010/">阅读Facebook上的讨论</a>，了解详情。

例如，在javascript中，你可以使用<a href="https://github.com/axios/axios">axios</a>库来使用ajax发送数据。要轻松使用它，只需从CDN服务器链接javascript，就可以直接使用它。

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script>
    axios.post('/api/form-process', {
        username: 'Jméno uživatele'
    })
    .then(response => {
        // Zpracování odpovědi z API
        alert(response.data.message); // Vyhodí hlášku se zprávou
    });
</script>
```

在这个简单的案例中，URL `'/api/form-process'`被使用ajax调用，POST方法传递对象`{ username: 'User name' }`。库本身已经处理了自动传递数据的物流，所以它被序列化为Json发送。在前端开发人员的术语中，这被称为**json payload**。

在PHP方面，我希望把它作为一个表单来使用（毕竟它是一个POST方法）。

```php
echo htmlspecialchars($_POST['帐号'] ?? '');
```

然而，在这种情况下，`$_POST`字段将是空的，没有数据被传递。`$_POST`变量只用于从表单中获取的数据（你可以通过我们没有丢弃的HTTP头来判断）。

所以在这种情况下，我们需要直接从HTTP请求中获取数据，***技巧方案就是用于此。然后，它很容易使用。

```php
$data = json_decode(file_get_contents('php://input'), true);

echo htmlspecialchars($data['帐号'] ?? '');

header('Content-Type: application/json');
echo json_encode([
    '信息' => '服务器爬行者',
]);
die;
```

作为一个例子，我还提供了一个简单的javascript答案。重要的是正确抛出HTTP头"'Content-Type: application/json'"，并在所有数据发送完毕后退出脚本。

强制使用`$_POST`。
-------------------------

如果你仍然想把提交的数据直接作为一个表单来处理，有一种方法可以转移它。在这种情况下，你需要修改创建ajax查询本身，并正确传递HTTP头信息。

```js
axios.post(
    '/api/form-process',
    {
        username: 'Jméno uživatele'
    },
    {
        headers: {'Content-Type': 'application/x-www-form-urlencoded'}
    }
).then(response => {
    // Nějaké zpracování
});
```

然而，在这种情况下，PHP方面的处理并不令人愉快，因为我们仍然必须纠正数据并将其转换为数组。

我设法想出了这个恐怖的办法。

```php
if (\count($_POST) === 1
    && preg_match('/^\{.*\}$/', $post = array_keys($_POST)[0])
    && ($json = json_decode($post)) instanceof \stdClass
) {
    foreach ($json as $key => $value) {
        $_POST[$key] = $value;
        unset($_POST[$post]);
    }
}

echo htmlspecialchars($_POST['帐号'] ?? '');
```

然而，坚持使用第一种方法并使用`'php://input''方法来检索数据要好得多。
