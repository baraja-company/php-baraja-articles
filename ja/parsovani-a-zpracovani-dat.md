PHPによるパースとデータ処理
===============

> id: '7dfc991d-a7ee-4e2b-8159-16cef1d27483'
> slug:
> 	cs: parsovani-a-zpracovani-dat
> 	ja: phpniyorupasutodeta-chu-li
> 
> publicationDate: '2017-10-15 09:54:02'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '33947488a0005f2977e6283c532a08d7'

> この記事では、PHPでデータを処理する方法について説明しますが、まだ終わりではありません。

そこで一時的に、可能性を簡単にスケッチしてみました。

- **Crawl character by character** は非常に古い方法で、コードが乱雑になりますが、他のすべての方法は内部でそれを行います。
- <a href="/explode">エクスプロード</a>で、文字列をデリミタで分割する。
- <a href="/regex">**正規表現**</a>は、単純な文字列を扱うのに最も適した方法です。
- トークン化器**は、複雑な文字列を正規表現に従って断片(トークン)に分割します。例えば、PHPはこのように処理されます。
