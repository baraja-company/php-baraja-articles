読み込んだファイルの一覧を取得する
=================

> id: '72716cbc-90a6-4870-848b-125e8430707f'
> slug:
> 	cs: ziskani-seznamu-vsech-nactenych-souboru
> 	ja: dumi-rundafairuno-yi-lanwo-qu-desuru
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '7e896aa0e501d0fe33dc6595c1bfb43e'

より複雑なアプリケーションをデバッグするとき、すべてのファイルが読み込まれたかどうか、何かが欠けていないかどうかがわからないことがあります。

Composer や他の種類の <a href="/autoloading-trid">class autoloading</a> を使用している場合、おそらくこの問題を知らないでしょう。しかし、他の開発者の古いアプリケーションをデバッグする際には、比較的よく発生することがあります。

ロードされたすべてのファイルを取得するには、 `get_included_files()` 関数を使用します。この関数は、絶対パス文字列の配列としてファイルを返します。

開発現場では、膨大な数のファイルを読み込むことがよくあります（例えば、この比較的シンプルなブログでも、160近いファイルを使っています）。しかし、ほとんどの場合、ファイルの内容はバルクリードに最適化されたOPCacheから取得されるため、大容量であることは問題にはなりません。
