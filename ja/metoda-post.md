POSTメソッドでデータを受信する
=================

> id: c2f273fa-7730-4521-82d0-1b3096269fac
> slug:
> 	cs: metoda-post
> 	ja: postmesoddodedetawo-shou-xinsuru
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '51a30cb759cdbb184ee5bb0bf3070af1'

POSTによるデータ送信は、<a href="/method-get">GET</a>とは全く異なり、より安全で、テキストは長くでき、その値はフォームやヘッダー（間違って取得することはありません）を除いて決定することができないのです。

出典
--------------------------

Sourceは<a href="/method-get">GET</a>メソッドとそれほど違いはない。URLにパラメータが表示されず、ファイル名だけが見える以外は、ほとんど同じです。

```php
echo $_POST['記事'] ?? '';
```

特徴・メリット・デメリット
--------------------------

- パラメータはリンクできないが、フォームを送信する必要がある
- インデックスを作成できない（前項と関連あり）
- <a href="/method-get">GET</a> よりも安全（データは隠された方法で送信され、値は履歴に表示されない）
- SSLと混同しないように、POSTは暗号化されていません。
