バラハ・ドクトリン接続の設定
==============

> id: fbd0961a-53fe-4713-8526-82e36bd1fb9b
> slug:
> 	cs: konfigurace-spojeni-s-baraja-doctrine
> 	ja: barahadokutorin-jie-xuno-she-ding
> 
> publicationDate: '2020-09-10 11:38:44'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: cee6f56731ae88d1de9aac0da49e0874

バラハ・ドクトリン](https://github.com/baraja-core/doctrine)内のデータベースへの接続を確立するためには、Neon設定ファイルという、ネットフレームワークの共通部分を使用する必要があります。

このような構成にすることができます。

```neon
baraja.database:
    connection:
        host: localhost
        dbname: my-database
        user: root
        password: ******
```

DI Containerがコンパイルされると、構成が検証され、特定のエラーを記述したエラーメッセージが投げられます。

ログイン認証情報は、コンテナのコンパイル時に安全に検証され、その後、コンテナ内に物理的に保存されます。データベースへの接続を提供するサービスだけがログインにアクセスでき、外部サービスやトレイシーバーからの不正な訪問者が簡単に取得することはできません。

後方互換性
----------

従来は、パラメータを使った定義が使われていたなど。

```neon
parameters:
    database:
        primary:
            host: localhost
            ...
```

しかし、この設定はアプリケーションのセキュリティを高めるために **deprecated** と表示されています。パラメータを使用する場合、任意のサービス（あるいはアプリケーションの一部）がログイン認証情報を要求したり、ページ上のアクティブトレーシーバーがそれを教えてしまう可能性があります。
