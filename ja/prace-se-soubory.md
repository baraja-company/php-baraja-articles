ファイルを操作する
=========

> id: '12330cfb-5729-419a-b2e4-cb3d1db998cc'
> slug:
> 	cs: prace-se-soubory
> 	ja: fairuwo-cao-zuosuru
> 
> publicationDate: '2020-02-16 16:30:05'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '0f5b9e84a974285dcb63baafc912be5f'

PHPには、ファイルを操作するための関数が多数あります。

- <a href="/fopen">fopen()</a>、ディスク上のファイルに低レベルでアクセスする。
- <a href="/file-get-contents">file_get_contents()</a>, ファイルやURLの中身を取得する。
- <a href="/file-put-contents">file_put_contents()</a> で、ローカルファイルに文字列を保存する。

ディスクの機能
--------------

- unlink($path)`, ファイルを削除する。
- `copy($from, $to)`, ファイルをある場所から別の場所にコピーする (URLからローカルディスクへも可能)
- `rename()`, ディスク上のファイルの名前を変更または移動する。
- chmod()`, ファイルの読み取り、書き込み、実行の権限を変更する。
