PPEにおける遺伝性と視認性
==============

> id: '71896a55-dcfd-4bb6-9f53-64f22b1514eb'
> slug:
> 	cs: dedicnost-a-viditelnost
> 	ja: ppeniokeru-yi-chuan-xingto-shi-ren-xing
> 
> publicationDate: '2020-02-16 22:17:05'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '4584f3dcfdcfdc506d34a37c36fe6dd1'

オブジェクト指向プログラミングの基本的な特性の1つは、**継承**と<a href="/encapsulation">カプセル化</a>です。これらの機能により、実装の可読性を保ちつつ、複雑なアプリケーションロジックを容易に構築することができるようになります。

相続の原則
-------------------

継承は、あるクラスの実装が他のクラスをベースにしていることを表現しています。OOPの用語では、**descendant**（継承するクラス）と**ancestor**（継承するクラス）という言い方をします。

一般に、継承は子孫が祖先の持つすべての機能を取得し、祖先が持っていた機能をそのまま採用するか、独自の方法で変更するか、完全にオーバーライドして独自の実装を使用することで機能します。

この手法の利用範囲は非常に広く、多くの<a href="/design-patterns">デザインパターン</a>で継承が利用されている。

継承の実際の使用例 - アプリケーションでのプレゼンター
--------------------

継承は、いわゆる**プレゼンター**の設計に適しており、これは**MVC**デザインパターンにおけるリンクロジックを表す特殊なクラスである。

例えば、`ホームページ`、`連絡先`、`ログイン`の3つのページがあるとします。

各ページの実装では、多くのロジックが繰り返されます（たとえば、リクエストの受け付け、URLの作成、テンプレートのレンダリング、結果のHTMLの送信など）。そのため、このロジックで先祖を1つ実装し、子孫で使うだけというのが便利です。

まず、祖先を定義することから始めます（クラス名は重要ではありません、私はNetteフレームワークからの慣例を使用しています）。

```php
abstract class BasePresenter
{
   public function link(string $route, array $params = []): string
   {
      // URLを構築するためのメソッドの実装
   }

   public function renderTemplate(string $path, array $params = []): string
   {
      // テンプレート描画ロジック
   }
}
```

クラスを定義するときに、新しい `abstract` キーワードを使いました。これは、`BasePresenter` クラスが抽象的であることを意味します。つまり、インスタンスを作ることはできず、他のクラスがそれを継承して実装するように使うだけでいいのです。抽象化には他にも有用な利点がありますが、それについては後述します。クラスは抽象的でなくても継承可能です - それは可能な設定のひとつに過ぎません。

ここで、2つ目のクラス、例えば `HomepagePresenter` を実装することができます。

```php
final class HomepagePresenter extends BasePresenter
{
   public function run(): void
   {
       // レンダリングロジック
       $this->renderTemplate('主ページ', [
          'コンタクトリンク' => $this->link('お問い合わせ先：デフォルト'),
       ]);
   }
}
```

これで、`HomepagePresenter`クラスが動作するようになりました。このクラスは `final` であることに注意してください。つまり、もはや継承することができないので、メソッドは指定したとおりに使われることが保証されます。

このクラスを実装する際に、`HomepagePresenter`だけが扱える `run()` メソッドを新たに作成しました。このメソッドの内部では、クラスにはない `renderTemplate()` メソッドと `link()` を呼び出しています。しかし、これは問題ではありません。なぜなら、 `extends` キーワードはメソッドがどこから継承されるかを教えてくれるので、それらが使用されるからです。

継承のおかげで、一度書いたメソッドを複数箇所で使用できるため、コードの再利用性を実現することができました。

特定のメソッドの実装をオーバーライドする
------------

継承の際に、特定のメソッドの動作をオーバーライドしておくと便利な場合が非常に多い。例えば、`ContactPresenter` で先ほどの `link()` メソッドの挙動を変更したい場合は、以下のようになります。

```php
final class ContactPresenter extends BasePresenter
{
   public function run(): void
   {
      // レンダリングロジック
      echo $this->link('ホームページ：デフォルト', []);
   }

   public function link(string $route, array $params = []): string
   {
      return 'https://baraja.cz';
   }
}
```

実装をオーバーライドするには、子メソッドに再度メソッドを定義し、メソッドボディを上書きするだけです。インターフェースを満たし、同じ入力引数を実装することが重要である。

継承の可視性
--------------------------

継承時に一部のメソッドを非表示にして、内部でのみ使用したい場合があります。あるいは、継承時にのみ使用できるようにし、パブリックインターフェースとしては使用できないようにします。

したがって、一般的には、いくつかの簡単な視認性のルールがあります。メソッドを `public`, `protected` または `private` で表し、その可視性のルールは以下の通りである。

- 誰でも、どこからでも `public` メソッドを呼び出すことができます。つまり、祖先と子孫の両方のインスタンスを作成するときに呼び出すことができます。
- 祖先や子孫だけが `protected` メソッドを呼び出すことができますが、インスタンス生成時に public インターフェースから呼び出すことはできません。これらは、継承を実現するための内部メソッドです（便利な応用例として、先ほどの例の `link()` メソッドがあります）。
- 継承や公開インターフェースの設定に関係なく、現在のクラスだけが `private` メソッドを呼び出すことができます。

実行時に可視性を変更する
----------------------------

非常に特殊なケースでは、実行時にメソッドの可視性を変更してから呼び出すと便利な場合があります。これは例えば、様々なDoctrineのライブラリで使用されています。

可視性を変更するには、次に PHP 自身が実装しているネイティブの **ReflectionClass** クラスを使用します。
