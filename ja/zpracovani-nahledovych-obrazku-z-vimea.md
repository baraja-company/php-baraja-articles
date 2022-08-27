Vimeoのサムネイル画像とメタ情報の処理
=====================

> id: '8b8cbef0-210a-4883-be07-5004e8f43739'
> slug:
> 	cs: zpracovani-nahledovych-obrazku-z-vimea
> 	ja: vimeonosamuneiru-hua-xiangtometa-qing-baono-chu-li
> 
> publicationDate: '2020-09-19 17:32:31'
> mainCategoryId: a0143f3c-ac75-46dc-a514-d3c9417ded4e
> sourceContentHash: '93c29218f162fb5dc8d0f265968b0f8d'

Vimeoのビデオをページに埋め込む場合（HTML埋め込みとして）、画像や、ビデオの長さ、完全なタイトル、著者などのその他の有用な情報も取得したい場合がよくあります。

幸いなことに、Vimeo はシンプルな HTTP API を提供しており、そこからビデオ トークンに基づくすべてのデータを読み出すことができます。

自分でAPIを書く必要がないようにするには、APIを完全に統合した[ready package](https://github.com/baraja-core/vimeo-video-api)を使えばいいのです。

コマンドでパッケージをインストールするのです。

```shell
composer require baraja-core/vimeo-video-api
```

使い勝手がいいんです。ドキュメントに従ってVimeoと通信するために `Baraja³³VimeoAPI³³VimeoVideoAPI` サービスのインスタンスを作成し、 `getInfo()` メソッドを呼び出してビデオトークンを渡し、そこから利用できるすべての情報を読み込める `VideoInfo` エンティティとして詳細情報を取得します（各ビデオについてすべての情報が常に利用できるわけではありません）。

こうすることで、プライベートな動画や公開されていない動画も照会することができます。しかし、必ず相手のトークンを知る必要があります。

利用可能なすべての情報をリストアップ
---------

ライブラリの基本的な使い方は次のようになります。

```php
$api = new \Baraja\VimeoAPI\VimeoVideoAPI;

$token = 0; // ビデオトークンを整数値で表示
$info = $api->getInfo($token);

echo var_dump($info); // すべてをリストアップ

// 動画の長さを秒単位で表示する。
echo '映像の長さは' . $info->getDuration();
```

変数 `$info` は、特定のビデオに関するすべての記述的情報を格納します。利用可能なすべてのメソッドの概要は[実装に記載されています](https://github.com/baraja-core/vimeo-video-api/blob/master/src/VideoInfo.php)。
