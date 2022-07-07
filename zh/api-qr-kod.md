二维码生成器 - API
============

> id: a9b1804f-438b-49fe-86e7-5a1b30a07210
> slug:
> 	cs: api-qr-kod
> 	zh: er-wei-ma-sheng-cheng-qi-api
> 
> publicationDate: '2019-09-11 10:04:00'
> mainCategoryId: '8598ac8d-7a70-4e2a-be9a-9f4f9ecee935'
> sourceContentHash: '89824ae94a740439f4ad3920bd311d3d'

二维码是一种特殊的二维码，用于传送短信息，例如传送到移动电话。

二维码可以通过简单地在谷歌的服务器上插入一个图像而轻松生成。

比如说。

<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR代码">

```html
<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR code">
```

我们可以设置3个参数。

- 尺寸，单位为 "px"（垂直和水平）。
- 编码（我建议使用 "UTF-8"）。
- 地址（页面的URL，电话号码，...）。

> 提示：如果有一个移动版本的网站，则链接到该网站。
>
> 在大多数情况下，你的移动电话会处理你的二维码。

我们可以非常容易地编写我们自己的嵌入函数。

```php
function getQrCode(string $url, int $size = 128, string $charset = 'UTF-8'): string
{
    $size = $size < 16 ? 16 : ($size > 2048 ? 2048 : $size);

    return '<img src="https://chart.apis.google.com/chart?cht=qr&chs='
       . $size . 'x' . $size
       . '＆choe=' . urlencode($charset)
       . '&chld=H%7C0&chl=' . urlencode($url)
       . '">';
}

echo getQrCode('https://php.baraja.cz');
```
