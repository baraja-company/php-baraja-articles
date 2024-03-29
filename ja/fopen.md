PHP関数fopen()
============

> id: '733ba757-1d5f-4717-a088-5ddabe730838'
> slug:
> 	cs: fopen
> 	ja: php-guan-shufopen
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '6b791877e57d88840d603dea69967b69'

fopen()`関数は、ディスク上のファイルへの低レベルのアクセスを表します。

プログラマーは、ファイルを開く、データを読む、新しいデータを書き込む、ファイルを閉じるなど、すべてを自分でやらなければならない。

ファイルの読み書きを素早く行うだけなら、もっとシンプルな選択肢があります。

- <a href="/file-get-contents">file_get_contents()</a> - ファイルから読み込む。
- <a href="/file-put-contents">file_put_contents()</a> - ファイルへの書き込み

基本的な使い方
----------------

```php
$text = '保存されるテキストはすべて...';

$file = fopen('ファイル.html', 'a+'); // ファイルとモードを開く

fwrite($file, $text); // ファイルに保存する
fclose($file);        // ファイルを閉じる
```

> ファイルを読み込み用にオープンして、それがクローズされないと、他のプロセスはそれにアクセスできません

ファイル操作モードの種類
----------------------------

私たちは、アクセス権に関する情報を伝えるさまざまなモードでファイルを操作することができます。

例えば、ファイルを読み取り専用で開きたい場合は、`r` モードで十分です。

書き込みのためにファイルを開くと、そのファイルはディスク上で `open` としてマークされ、再び閉じるまで他のプロセス（スクリプト）は書き込むことができなくなります。これにより、書き込み中にファイルが破損することはありません。

| モード｜意味
|-------|--------|
| もしファイルが存在しなければ、作成されます。
| `a+` | データを追加したり、データを読み込むためにファイルを開きます。
| r`｜ 読み取り専用で開く｜←今ココ
| `r+`｜読み書きのために開く｜｜｜。
| w`｜書き込み用にオープン、元のデータは削除され新しいデータに置き換わる、存在しない場合は新規作成される |。
| w+`｜ 書き込みと読み込みのために開く、元のデータは削除され新しいデータに置き換わる、存在しない場合は作成される |。
