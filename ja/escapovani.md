PHP での文字列のエスケープ
===============

> id: '40f9361e-e286-4b5e-a0c0-1f6cda8106af'
> slug:
> 	cs: escapovani
> 	ja: php-deno-wen-zi-lienoesukepu
> 
> publicationDate: '2019-11-26 11:56:52'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '026284252541bc9c918a87298b9e261c'

エスケープとは、異なる文脈で異なる意味を持つ文字を書くために使われる。

例えば、引用符で囲まれた文字列の中に別の引用符を挿入したい。どうすればいいのか？

2つのオプションがあります。

```php
echo "リーバイス・ジーンズ"; // 引用符の種類の組み合わせ

echo 'レヴィのジーンズ'; // バックスラッシュのエスケープ
```

また、HTMLテンプレートに変数を書き込む場合、文字列の内容が異なる文脈で、何か特別な意味を持つ可能性があるため、エスケープは重要である。

したがって、例えば（変数に入れた）HTMLコードをリストアップする場合、リストアップの処理をしないとHTMLコードが実行されてしまいます。

例えば、こんな感じです。

```php
$message = 'こんにちは<b>トミー！</b>。';

echo $message; // 間違っている

echo htmlspecialchars($message); // そうですね）
```

エスケープの問題は非常に複雑なので、David Grudel氏の記事<a href="https://phpfashion.com/escapovani-definitivni-prirucka">Escaping - The Definitive Guide</a> を読むことをお勧めします。
