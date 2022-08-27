ドキュメンタリーのコメント、チェコ語か英語か？
=======================

> id: e5f1bf13-ab9e-4a70-a95c-77fb50aa4878
> slug:
> 	cs: dokumentacni-komentare
> 	ja: dokyumentarinokomento-cheko-yuka-ying-yuka
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: a33af5a14338e9895fe87087362bd476

ドキュメントを書くのは時間がかかるし、自分の後に誰も読まないことが多いので、ソースコードに直接コメントを書くのは良い習慣です。しかし、不必要に多くのテキストを使用すると、コードが乱雑になり、同時にモニターに収まらなくなり、また可読性が低下する可能性があります。

したがって、どんなコードも**自己説明的**であるべきです。つまり、コードを読んだときに、それが何をするのかがすぐにわかり、たった一つのことをするだけであるべきなのです。

例えば、ある関数が `getUserProfileById($id)` という名前であれば、その関数が何を行い、どんな戻り値を期待できるかは一目瞭然でしょう。そのため、入出力パラメータのみを記述し、（もっと複雑なことが起こっている場合は）原理を短いテキストで説明するコメントがよく使われます。複数の人を対象としたコードの場合、作者や作成日を記載するのが礼儀です。

```php
/**
 * 渡された値が有効な IPv6 アドレスである場合に TRUE を返す。
 * 代替の正規表現
 * /^(((?=(?>.*?(::))(?!.+\3)))\3?|([\dA-F]{1,4}(\3|:(?!$)|$)|\2))
   (?4){5}((?4){2}|((2[0-4]|1\d|[1-9])?\d|25[0-5])(\.(?7)){3})\z/i
 * 著者のヤン・バラセク氏
 */
function isIpV6(string $value): bool
{
   if (str_contains($value, ':')) {
      return (bool) filter_var($value, FILTER_VALIDATE_IP, FILTER_FLAG_IPV6);
   }

   return false;
}
```

コメントからすぐにわかるように、この関数は入力としてIPv6アドレスを想定しており、検証の結果に応じてブール値の `TRUE` または `FALSE` を返します。

ブートストラップアノテーションは、標準化されたキーを使って値を示し、それを多くのエディターがさらに処理して、例えば、関数呼び出し時のパラメータのヒントや、依存関係の自動的な受け渡しなどを行うことができる。

コード内の直接のコメントは、特別な場合にのみ使用されます。もしコードが内部コメントを必要とするならば、それは通常、そのコードを複数の小さなパーツに分割し、それぞれのパーツに対応させるべきことを示すものです。

言語
--------------

クラス、メソッド、関数、変数の名前は必ず英語でつけるのが通例で（多くのプログラマーが読みやすいように）、キーは比較的統一された構文なのでそこでも言語は関係なく、補助テキストはプロジェクトの用途によります。安定した人数のチームの小さなプロジェクトであれば、英語は必ずしも問題ではなく、逆に少なくとも全員が説明を完璧に理解することができます。

アノテーションを使用する動機
-------------------

特殊文字（コールサイン）はエディタ以外のツールでも認識されるので、例えば関数定義の直上にそのテスト（どんな入力でどんな出力を期待するか）を置くと便利です。こうしてテストが書かれた場所を忘れることなく、同時にすべてのプログラマはその関数が何をするのかをすぐに理解できます。

ここでは、テスト定義がどのようなものかを示す小さな例を紹介します（あくまで記法の概念であり、特定のテストツールではありません）。

```php
Dosadí hodnoty a vynutí jejich dump do prohlížeče / konzole:
@test Název I---> $limit => {1-{5-8}}, $city => {Praha|Kladno|Brno|Plzeň} O---> [DUMP]

Vygeneruje náhodný hash o délce 6 znaků a ověří, zda je hlavička odpovědi jakákoli, kromě 201:
@test Název I---> $hash => {a-z0-9|len:6} O---> [HEADER:***|!HEADER:201]

Vygeneruje dvojici čísel a na výstupu ověří jejich součet:
@test Sčítání čísel I---> $a => {1-5}, $b => {7-12} O---> ($return == $a+$b)

Načte náhodný článek s ID mezi 1 až 30, ověří jeho délku nebo neprázdnost:
@test Získání textu článku I---> $id => {1-30} O---> (strlen($return) > 64 || $return != NULL)

Vygeneruje náhodnou IP adresu a ověří, zda je z České republiky:
@test Geolokace I---> $ip => {1-255}.{1-255}.{1-255}.{1-255} O---> ($return['国'] == 'エン')

Pokusí se načíst článek s vysokým ID a při testování mrkne na modifikátory (filtry):
@test Neexistující článek I---> $id => {1000000+} O---> [HEADER:4**:3**|NOCONTENT]

Vrátí počet rohů jednorožce:
@test Jednorožec I---> $maRohy => TRUE O---> 1

Kolik prstů ukazuji?
@test Prsty I---> $ukazuji => {0-10|NAME:prsty} O---> ($return == {*|NAME:prsty})
```

というのは、一般的には、例えば、ルールに従って書くことになる。

```php
@test <název_testu> I---> $<proměnná> => <hodnota> O---> [<modifikátor>:<hodnota>] (<výraz_platnosti>)
```

テストの内部では、正規表現を使った乱数値生成器を使いました。
以下はその例です。

```php
{<hodnota><směr_nerovnosti>}           {500+}  {250-}   {-200-(-50)}
{<začátek>-<konec>}                    {0-5}   {a-z}   {a-f0-9}
{<Hodnota1>|<Hodnota2>|<Hodnota3>}     {Praha|Kladno|Brno}
{<výraz>|<modifikátor>:<hodnota>}      {a-z0-9|len:6}  {*|len:6}
{*|<modifikátor>:<hodnota>}            {*|randomWord:5}
```
