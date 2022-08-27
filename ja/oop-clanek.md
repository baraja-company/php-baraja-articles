PHPによるオブジェクト指向プログラミング
=====================

> id: '3e56e179-877c-43d5-9f2d-6f260142a8ea'
> slug:
> 	cs: oop-clanek
> 	ja: phpniyoruobujekuto-zhi-xiangpuroguramingu
> 
> perex:
> 	- Programování v objektech pro začátečníky i pokročilé. Návrhové vzory a jak programovat správně. Praktické návody a zkušenosti.
> 	- 初心者から上級者まで、オブジェクトでプログラミングをする。デザインパターンと正しいプログラミングの方法実践的なチュートリアルと体験談。
> 
> publicationDate: '2020-02-16 22:22:50'
> mainCategoryId: b7ee485b-e3be-4f71-a536-dbe81fe0131e
> sourceContentHash: c7cebd6f50fc729183b5b9e070a5bf00

このページは、PHPにおけるOOPの完全ガイドとして機能します。基本的なものから高度なものまで、すべてのプログラミング手法を学び、何十ものサンプルを見て、さらに優れたコードと再利用可能なアプリケーションを書くことができます。

はじめに
--------------------

- <a href="/philosophy-oop">オブジェクト指向の基本的な考え方</a>、オブジェクト指向の考え方
- <a href="/oop-concepts">用語の索引と説明</a>。
- 動機 - なぜオブジェクト指向のプログラミングをするのか？どのようなメリットがあるのでしょうか？
- <a href="/proc-use-frameworks">フレームワークとライブラリを使用する理由と方法</a>。

シリーズの一部
------------

- <a href="/uvod-do-oop">OOPの基本</a>、クラスの定義とインスタンスの作成。
- <a href="/methods-and-passing-input">コンストラクタ、メソッド、および入力を渡す</a>。
- <a href="/encapsulation">カプセル化の原理</a>。

今後の記事
-------------------

- <a href="/dedication-and-visibility">奉納と可視化</a>。
- <a href="/comparison-vs-identity-oop">比較 vs. アイデンティティ</a>。
- <a href="/exceptions">データ検証、例外処理、エラーキャッチ</a>。
- スタティックパッシングとインスタンスパッシング
- <a href="/service-configuration">サービス構成と定数</a>。
- <a href="/object-types">オブジェクトの種類</a>：クラス、オブジェクト、サービス、エンティティ、値-オブジェクト
- <a href="/interface">インターフェース</a>、継承や抽象クラスで使用する。
- <a href="/magicke-methods-oop">特殊なマジックメソッド</a>、 `__toString` と PHP のマジック。
- 高度なオブジェクト処理、`instanceof`演算子
- 名前空間とライブラリ開発の原則
- <a href="/fluent-interfaces">フルエントインターフェイス</a>、ネットフォームズ例

OOPのデザインパターンとトリック
----------------------------

オブジェクトでプログラミングする場合、多くの巧妙なヒントや推奨事項があり、それらに従えば、アプリケーション全体の可読性、再利用性、保守性を非常に効果的に向上させることができます。**この記事では、私が開発者の方々とコンサルティングを行う際に、最もよくあるシナリオを紹介しています。

- <a href="/design-patterns">デザインパターンとは何か、何のためにあるのか</a>。
- <a href="/autoloading-trid">ディスクから名前を指定してクラスをオートロードする</a>。
- 依存性注入、トピックの紹介とインスタンスの取得
- 単一責任の原則
- ファクトリー、シングルトン、スタティック
- データを型実体でカプセル化する(Doctrine)
