定義されたすべての関数のリストを取得する
====================

> id: '99ed7887-33c8-44d7-b0cb-ac37b2336f48'
> slug:
> 	cs: ziskani-seznamu-vsech-definovanych-funkci
> 	ja: ding-yisaretasubeteno-guan-shunorisutowo-qu-desuru
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '9dbffadf6cf6473eedadbc0bf7eccbb4'

現在の環境で利用可能なすべての機能のリストを取得することが便利な場合があります。特に、他人のサーバーを管理しているときに、自分の立ち位置を確認する必要がある場合です。

関数のリストは `get_defined_functions()` 関数を呼び出して、配列の形でデータを返すことで得ることができる。

```txt
[
   internal => [
      ...,
   ],
   user => [
      ...,
   ]
]
```

機能一覧は大きく2つに分かれています。

- 内部関数とは、PHP 自身やインストールされている拡張モジュールで定義されている関数のことです。
- ユーザー（`user`）関数は、ユーザーコード自体で定義される関数である。ソースコードに書き込んだり、インストールされたライブラリに含まれる関数です。

このリストは、アプリケーションのデバッグによく利用されます。
