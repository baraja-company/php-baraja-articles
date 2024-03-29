オブジェクト指向プログラミングの基本的な考え方
=======================

> id: c38214d9-8c92-4484-9c31-239d716f2545
> slug:
> 	cs: filosofie-oop
> 	ja: obujekuto-zhi-xiangpuroguraminguno-ji-ben-dena-kaoe-fang
> 
> publicationDate: '2020-01-02 22:21:56'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3841046b40a039413bacd045f275c68

オブジェクト指向プログラミングは、プログラミングの方法についてのパラダイム（考え方）である。OOPは、実際のプログラミングで何度も何度も解決されてきた一般的な問題や困難を、かなり根本的に単純化してくれることがすぐにわかると思います。

基本的な考え方
-----------------

オブジェクト指向プログラミングの基本的な考え方は、大きなアプリケーション（複雑なタスク）を多くの小さなパーツに分割し、エレガントかつ独立して解決できるようにすることです。

例えば、航空券の予約システムをプログラミングする場合、何千ものケースを解決する非常に複雑なプロジェクトとなります。もし、すべての複雑なロジックを多くの層とパーツに分解することができれば、複雑なシステム全体を容易に理解し、個々のサブタスクを独立してプログラムすることができます。

OOPの実用的な利点とコードをオブジェクトに分割すること
------------------------------------------------

学術的な観点とは別に、OOPを使用する実用的な理由も多くあります。

- アプリケーションは<a href="/encapsulation">カプセル化</a>を作成します。つまり、データは常に少数のローカルコンテキストで処理されるので、アプリケーション全体を一度に考える必要はないのです。
- アプリケーションを何十万もの小さなパーツに分割して、別々の人が並行して開発し、共通のインターフェイスをアレンジすればいいのです。
- アプリケーションを様々なレイヤー（抽象的にはモジュール全体、局所的には特定のアルゴリズムを解く特定の機能）の視点から見ることができるのです。
- アプリケーションをデバッグする際（デバッグ）には、アプリケーションに手を加えたり、一部を省略したり、置き換えたりすることが可能です。
- デザインパターン**をうまく適用することで、コードのエラーや複雑さを未然に防ぐことができるのです。

個人的には、2人以上のチームでプログラミングが変わるとは思えません。

> **注意:**
>
> オブジェクトを使用すると、コンピュータにメモリとCPUのオーバーヘッドが少し増えるので、アプリケーションのパフォーマンスが少し低下します。しかし、実際の環境では、これは問題ではありません。なぜなら、アプリケーションをオブジェクトに再プログラミングすることで、パフォーマンスは多少（通常は数パーセント）低下しますが、プログラマーの時間は（通常は数十から数百パーセント）節約できるからです。 人間の時間は常にコンピューターの時間よりはるかに高価（そして非常に限られている）です。
>
> また、OOPはアプリケーション全体に大きな簡略化をもたらし、大規模なアプリケーションを合理的な時間で完成させることができることも忘れてはならない。多くの複雑なアプリケーションは、オブジェクトなしではほとんどプログラムできないだろう。

現実世界との関係
-------------------------

ソフトウェア設計におけるOOPの基本的な目標は、実世界の特性、動作、原理を可能な限りシミュレートすることである。OOPにおけるオブジェクトは実体を表す。このような考え方によって、よく理解できる巨大な複雑システムを構築し、コンピュータがなくても解決できるような実世界の問題を内部で解決し、その原理を実際の人間に説明することができるようになるのです。

例えば、コンテンツ管理アプリケーションを実装する場合、多くの実在するエンティティ（記事、著者、カテゴリ）にすべての内部ロジックをレイアウトし、（データベースで従来行われてきたように）人為的に生成したレコードIDではなく、実在する関係に基づいてセッションを構築することが理にかなっています。

具体的な実装例です。

```php
class Article
{
    private Author $author;

    /** @var カテゴリ[]*/
    private array $categories;

    private string $title;
}

class Author
{
    private string $name;
}

class Category
{
    private string $name;
}
```

見てわかるように、`Article`クラスは著者を持つレコードのIDのような技術的なパラメータを含むだけでなく、著者エンティティへの本当のバインディングであり、それはまたそれ自身のプロパティを持ちます。

具体的な実装や構文の説明については、<a href="/vod-do-oop">入門チュートリアル</a>をご覧ください。
