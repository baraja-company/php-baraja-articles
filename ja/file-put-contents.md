ファイル_put_contents
=================

> id: '7ec781c6-e3db-484d-8a49-0b93ab8252b1'
> slug:
> 	cs: file-put-contents
> 	ja: fairu-put-contents
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '78211a3696ebba56d559045f8a0ab9e4'

ファイルへの自動書き込みには、**file_put_contents**関数が適している。また、<a href="/fopen">fopen()</a>を使うこともできますが、これは初心者にはお勧めしません。

サンプル
--------------------------

```php
$file = 'ファイルテキスト';
$content = 'ファイルに保存する内容。';

file_put_contents($file, $content);
```

file_put_contents は2つのパラメータを持つ。

- filename` に書き込む。
- これから書き込む`ファイルの内容`です。

> 注意: `file_put_contents()` は、ファイルを最新の内容で上書きします。

上書きに注意
--------------------------

file_put_contentsで保存する場合、データの上書きに注意してください。この関数は、現在のコンテンツをすべて削除し、新しいコンテンツに置き換えます。ですから、テキストを付加するだけなら、自作のスクリプトを使って、先頭に付加することも、末尾に付加することも可能です。

```php
$file = 'ファイルテキスト';
$content = '新しいコンテンツです。';

$oldContent = file_get_contents($file);
file_put_contents($file, $content . $oldContent);
```

つまり、まずファイルを開いて、新しい内容を書き込んで、その後に元の内容を書き込む...。

もし、新しいコンテンツの前に古いコンテンツを追加したい場合は、スクリプトを少し修正するだけでよい。

```php
$file = 'ファイルテキスト';
$content = Nový obsah.';

$oldContent = file_get_contents($soubor);
file_put_contents($file, $oldContent . $content);
```
