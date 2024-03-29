PHP入門
=====

> id: '66b7fd22-6195-4999-ad8d-b799afdc1876'
> slug:
> 	cs: uvod
> 	ja: php-ru-men
> 
> perex:
> 	- 'Úvodní tutoriál do jazyka PHP, možnosti jazyka, informace o tvorbě webu v PHP.'
> 	- PHP言語の入門チュートリアル、言語オプション、PHPでのWeb開発に関する情報。
> 
> publicationDate: '2019-09-29 19:25:06'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: db6ba8fe6d5ca28aab62d7302c1d7527

一般教養を目的とした、PHPを学ぶためのオンラインコースです。本サイトで無料配布しています。このテキストは、書面による確認なしに、有料のコースで使用することはできません。本サイトで使用されているサンプルは、お客様のアプリケーションにそのままお使いいただけます。[利用規約](https://baraja.cz/vseobecne-obchodni-podminky)。

HTMLからのアップグレード
--------------

"アップグレード"？むしろメディアのスピンに聞こえるし、実際そうなのだが。

アップグレードはありません。PHPは、純粋なHTML文書のすべての機能と特徴を保持し、新しい書き方と配置のオプションを追加するだけです。素晴らしいでしょ？

静的なHTMLページを作成していると、コードがタグで構成されていることは既にご存知でしょう。タグは、尖った大括弧で囲まれたキーワードとして定義されます（例えば、`<b>Hello!</b>`は、太字のテキスト **Hello!** を表現します）。

PHPは、HTMLページに `<?php` や `?>` タグとして挿入され、その中に他のアプリケーションロジックが記述されます。重要なのは、PHPは独自の構文(コードを書く際のルール)を持っており、HTMLとは異なり、エラーに寛容ではありません。

具体的な方法と各タグの意味については、後述します。手始めに、PHPがサーバー上でどのように動作し、コードがどのように処理されるのか、その一般原則を理解することが重要です。

ユーザーとサーバー間の通信の流れ
---------------------------------------

通常のHTMLページの場合、おおよそこのような仕組みになっています。

> ユーザーがリクエスト（特定のHTMLページを要求する）を送ると、サーバーはディスクを見て、保存されているものをそのまま送り返す。特に何も起こらないし、それ以上期待しない方がいい。ページが静止しているだけで、サーバーとのやりとりはできない。

しかし、PHPを追加すると、不思議なことが起こり始めるのです。

> ユーザーが再びページを要求する。サーバーはディスク上のファイルを開くが、そのファイルには純粋なHTMLだけでなく、PHPスクリプトを示す特別なタグも含まれていることがわかる。そこで、まずそれらを評価し、PHPが生成したものを送信する。

PHPコードの評価は、デフォルトではページがロードされるたびに行われます。将来的には、コードをキャッシュする（高速処理のためにコンパイルしたものを保存する）方法を学ぶことになるでしょう。

PHPのスクリプト処理とC/C++の違いについて
------------------------------------------

学校で`C`言語や`C++`言語を習ったことがある人もいるかもしれません。PHP は `C` 言語の構文を直接ベースにしており、カーネル内部では `C` 言語が使われているので、いくつかの共通点と逆に相違点を知っておくとよいでしょう。

一般的なコンパイルプログラム（パソコンやスマートフォンで直接ローカルに動作するもの）の基本原理は、アプリケーションロジックを動作メモリにロードし、OSと直接通信して、ユーザーからの入力を受け取り、プログラムの出力を表示することである。重要なのは、プログラムが起動から終了までずっと孤立して動いていることです。

PHPはページレンダリング要求ごとに起動し、毎回すべてのコードとデータを再ロードし、終了します。そのため、PHPスクリプトの寿命は文字通りユピーで、通常、数十ミリ秒しか存在しません。

この方法の利点は、より高いレベルの分離性です。何か問題が発生しても、次のページロードですべてが再ロードされます。一方、この方法では、データベースへの接続やディスクからのファイルの読み込みなどを繰り返し行う必要があるため、性能面での要求が高くなります。

今後、ほとんどの新しいサーバー（PHP 7.1以降）が基本設定で設定している `OP Cache` 拡張を使用すれば、PHPスクリプトを動作中のメモリにロードしておくことができることを学習することになります。

海外のPHPスクリプトをダウンロードする
--------------------------

学生との議論で比較的多いのは、サーバーから外国のPHPスクリプトをダウンロードして、そのソースコードを見る方法です。この疑問の前には、ページのHTMLコードをWebブラウザで簡単に表示できることが考慮されます。

答えは、**PHPスクリプトはダウンロードできない**からです。これは、PHPコードがまずWebサーバーで評価され、生成されたHTMLコード（またはその他の出力）がユーザーのブラウザに送信されるためです。そのため、ダウンロードできるのはPHPスクリプトの出力のみで、スクリプトそのものはダウンロードできません。

HTMLへの生成はどのように行われるのですか？
---------------------------------

文字通り、HTMLページをサーフィンすることで機能するわけではありません。PHPスクリプトは、必ずPHPファイルでなければなりません。

これまでは、拡張子が**.html**で終わるファイルをたくさん集めた巨大なフォルダを作るのが一般的でした。これでだいぶファイル数が減りますね。極端な話、1つのファイルになることもあります。
