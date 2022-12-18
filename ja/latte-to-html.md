ラテテンプレートを文字列にレンダリングする方法
=======================

> id: '847b028d-c4e6-4480-afe4-797cf83fce81'
> slug:
> 	cs: latte-to-html
> 	ja: ratetenpuretowo-wen-zi-lienirendaringusuru-fang-fa
> 
> publicationDate: '2022-12-18 11:30:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '0647f751eb754e9cf4afa9f3d0b45e17'

テンプレートシステム「Latte」は、Web上のほぼすべての種類のテンプレートのレンダリングに適しています。例えばフロントエンドのテンプレートのレンダリングには、ここ数年ReactやVue.jsが最適とされてきましたが、バックエンドのメールテンプレートのレンダリングには、やはりLatteが勝りますね。

では、特定のHTMLテンプレートを、電子メールで送信可能な文字列に確実にレンダリングするには、どうすればよいのでしょうか。

簡単です。

```php
$latte = new Engine();
$latte->setLoader(new StringLoader());

$template = '<p>私の名前です。{firstName}:{$lastName}!';

$html = $latte->renderToString($template, [
	'ファーストネーム' => 'ヤン',
	'ラストネーム' => 'テスト',
]);

echo $html;
```
