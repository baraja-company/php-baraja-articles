初心者のためのPHPオンラインコース
==================

> id: '10ecc1cc-8d49-4882-8ecf-114ad74902df'
> slug:
> 	cs: zacatek
> 	ja: chu-xin-zhenotamenophponrainkosu
> 
> perex:
> 	- 'Výuka PHP online, kurz pro začátečníky. Naučíte se programovat v PHP přímo od senior vývojáře.'
> 	- オンラインPHPチュートリアル、初心者のためのコース。先輩開発者から直接PHPのプログラミングを学ぶことができます。
> 
> publicationDate: '2020-02-09 14:27:47'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: f4f8a8d74e09eb298963f7ab8ff00959

PHPは、最新のWebアプリケーションのために設計されたサーバーサイドスクリプト言語です。

すなわち、非常に短期間（数週間程度）で、フォーム、ユーザーアカウント、データベースなどを使用したほぼすべての簡単なWebアプリケーションを作成できるところまで、言語の原則のほとんどを理解することができるようになるのです。

PHPのもう一つの利点は、ほとんどすべてのサーバ（ホスティング用）で大規模に普及し、常に開発が行われているため、あなたのアプリケーション/ウェブがどこでも動作することを保証することです。

どうすればいいんだ！？
------------

事前に以下のことを確認してください。

- 脳、考えることが多いんです。
- スクリプトを実行するコンピュータ（またはサーバー）。
- 数学または何らかの技術分野の知識があると便利です。
- 適切な学習教材（本サイトや<a href="https://www.php.net">公式マニュアル</a>など）。
- HTMLとCSSの基本的な知識。
- 少なくとも英語の基礎知識があると便利です（公式マニュアルやウェブフォーラムなど、ほとんどの資料は英語のみです）。
- 他のプログラミング言語の知識があれば有利です（PHPがベースとしているC/C++に非常に似ています）。
- HTMLとCSSの基礎知識がないと、PHPの理解は非常に難しいので、強くお勧めします。
- 基本的なソフトウェアの知識（システムによって異なり、最高のプログラムも無料ではない**）。

基本ソフト
-----------------

Windowsコンピュータ:`
- デバッグモードを提供する最新の**ウェブブラウザ**。個人的には、<a href="https://www.google.com/chrome">Google Chrome</a>を使用しています。
- 手始めに、シンタックスハイライトのあるより良い**テキストエディタ**で十分です。世界最高のものは、おそらく<a href="https://www.sublimetext.com">Sublime Text</a>でしょう（多くのフォーマットであらゆるテキストを扱う高度な作業、複数のカーソルでの作業、正規表現を提供し、一般にプログラミング以上の多目的ツールとなっています）。過去には、私はチェコ語のエディター <a href="https://www.pspad.com/cz/">PSpad</a> を使っていましたが（現在、私は非常に時代遅れで、現代のウェブサイトには不十分だと考えています）、一部の人々は <a href="https://www.slunecnice.cz/sw/notepad/">Notepad++</a> も使用しています。
- 本気で開発するなら、完全な開発環境を使ったほうがいい。仕事では、<a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>を使用しています。これは、これまでにコーディングされたコードを書くための最高のエディタだと私は考えています。
- PHP、MySqlデータベースができ、設定ができる**ウェブサーバー**です。私は現在、Windowsではパッケージ化されている<a href="https://www.apachefriends.org/download.html">Xampp</a>がベストチョイスだと考えています。

Linux (特にWebサーバ):`
- <a href="https://www.google.com/chrome">Google Chrome</a>やFirefoxなど、任意のブラウザを使用することができます。
- Ubuntuでは、<a href="https://www.sublimetext.com">Sublime Text</a>を使用していますが、どちらも始めるには十分です。
- ウェブサーバー**のインストールは、Windowsに比べると難易度が高いです。例えばUbuntuでは、このための<a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">Tasksel</a>というプログラムがあり、ターミナルで制御されます。
- Linux サーバをインストールするのであれば、<a href="https://www.nginx.com/resources/wiki/">Ngnix</a> も検討する価値があります。

マック:`
- Macはプログラムを組むのに最適で、ユーザーに寄り添ってくれます。
- MacBook Proでの開発には、<a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>という最高の開発環境を使い、通常のテキストファイルの編集には<a href="https://www.sublimetext.com">Sublime Text</a>という大きなファイルの扱いが得意なものを使っています。
- サーバーは自分で**Terminal**を使ってインストールしたので、初心者には難しいかもしれませんが、**Mamp**というツールがあり、マウスでいろいろとクリックできるんです。

先輩のおすすめポイント:`ヽ

2020年現在、PHPやアプリケーション全体の運用に関するすべての問題は、Dockerコンテナによって簡単に解決できることが明らかになりつつあります。Dockerの操作方法を学ぶことは、将来的に何百時間もの時間を節約し、新人を既存のプロジェクトに簡単に統合することができます。

シリーズの一部
------------

PHPの完全入門編として、初心者の壁を乗り越え、PHPの基本にすべり込むための記事をいくつか書いています。

- <a href="/introduction">PHP学習への入門</a>。
- <a href="/first-script">最初のスクリプト</a>。
- <a href="/principles of-variable-writing</a> - 変数書き込みの原則
- <a href="/cycles">サイクル</a>について
- <a href="/how-to-findout">コードの探し方</a>。
- <a href="/methods-sending-data">データ送信の方法</a>。
- <a href="/include-file">インクルード（塊からページを組み立てる）</a>。
- <a href="/conditions">条件と分岐</a>。
- <a href="/safe-application">セーフアプリ</a>。

しかし、その後、Web開発はかなり複雑化し、本当に多くの知識が必要になってきます（少なくとも、そのようなものが存在すると疑うこと）。言語全体やWeb制作の考え方はかなり複雑なので、少なくとも<a href="/knowledge">基礎知識の概要</a>は用意し、徐々に追加して記事を書いています。

複雑なアプリケーションの開発には、<a href="/opp">オブジェクト指向プログラミング</a>を使い始めることをお勧めします。

ライセンス
-------

これらの教材は `php.baraja.cz` というウェブサイトを通じて無料で提供していますので、他の有料コースで使用することはできません。テキストには誤りや不正確な表現が含まれている場合があります。このマニュアルは、正式な翻訳ではありません。

テキストに関するすべての権利は私にありますので、複製は禁止されています。本サイトのURL（リンク先）およびサンプルソースは、特に制限なく使用することができます。

お問い合わせ先
-------

ウェブ開発について雑談するのは楽しいし、一般的なアドバイスもするが、**より複雑な仕事は有給の仕事と見なす**。

- 電子メール：jan@barasek.com
- 個人用<a href="https://www.facebook.com/janbarasek">Facebook</a>。

<a href="https://baraja.cz/kontakt">すべての連絡先</a>。
