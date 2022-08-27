QRコード生成ツール - API
================

> id: a9b1804f-438b-49fe-86e7-5a1b30a07210
> slug:
> 	cs: api-qr-kod
> 	ja: qrkodo-sheng-chengtsuru-api
> 
> publicationDate: '2019-09-11 10:04:00'
> mainCategoryId: '8598ac8d-7a70-4e2a-be9a-9f4f9ecee935'
> sourceContentHash: '89824ae94a740439f4ad3920bd311d3d'

QRコードとは、携帯電話などに短い情報を送信するための特殊な2次元コードです。

QRコードは、Googleのサーバー上にある画像を挿入するだけで簡単に生成できます。

例えば、こんな感じです。

<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QRコード">。

```html
<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR code">
```

3つのパラメータを設定することができます。

- サイズ（縦・横）：`px`単位
- エンコーディング (`UTF-8` を推奨)
- アドレス（ページのURL、電話番号、...）

> ヒント：モバイル版のサイトがある場合は、代わりにそちらへリンクしてください。
>
> ほとんどの場合、QRコードは携帯電話で処理されます。

エンベッディングのための独自の関数を書くのは非常に簡単です。

```php
function getQrCode(string $url, int $size = 128, string $charset = 'ユーティーエフエイト'): string
{
    $size = $size < 16 ? 16 : ($size > 2048 ? 2048 : $size);

    return '<img src="https://chart.apis.google.com/chart?cht=qr&chs='
       . $size . 'x' . $size
       . '蝸牛' . urlencode($charset)
       . '&chld=H%7C0&chl=。' . urlencode($url)
       . '">';
}

echo getQrCode('https://php.baraja.cz');
```
