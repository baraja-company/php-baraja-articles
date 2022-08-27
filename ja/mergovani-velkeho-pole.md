PHP での大きな配列のマージ
===============

> id: ccfb3f47-c8a0-4d0f-aa29-18d4c1775725
> slug:
> 	cs: mergovani-velkeho-pole
> 	ja: php-deno-dakina-pei-lienomaji
> 
> publicationDate: '2020-02-06 09:32:08'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3bfc3b53d26a4c9b675e851a643fc71a'

多くの場合、複数の配列をマージする必要がありますが、これは `array_merge` 関数で非常にエレガントに行うことができます。

```php
$userIdsA = [1, 2, 3];
$userIdsB = [5, 6, 7];

// [1, 2, 3, 5, 6, 7] を返す。
$finalIds = array_merge($userIdsA, $userIdsB);
```

array_merge` 関数は、2 つの配列を 1 つの大きな配列にマージする。キーの衝突があった場合、右端の配列の値が優先されます。

ループ内の繰り返しマージ
---------------------------

しかし、ループの中だけで作成された配列（例えば、データベースから作成し、foreachで渡す）を受け取ることが多いので、事前にマージの回数がわからないことがあります。

素朴な解決策としては、次のようなものが考えられます。

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds = array_merge($finalIds, $user->someIds);
}
```

しかし、この方法では、反復処理ごとに配列を結合し、大きな配列全体を反復処理しなければならないため、非常にCPU非効率です。

しかし、データを1回しか処理しないようにマージアルゴリズムを変更することで、簡単に解決することができます。

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds[] = $user->someIds;
}
```

この場合、`$finalIds`フィールドは少し多くのデータを生成しますが、それでも時間短縮の利点に比べれば問題は少ないでしょう。

マージ自体は、使用しているPHPのバージョンによって異なりますが、エレガントなトリックで解決されます。

```php
/* PHP 5.6 以降 */
$finalIds = call_user_func_array('array_merge', $finalIds + [[]]);

/* PHP 5.6+ およびそれ以降 */
$finalIds = array_merge([], ...$finalIds);

/* PHP 7.4+以降で空でないフィールドを使用する場合 */
$finalIds = array_merge(...$finalIds);
```

特に、`array_merge(...$finalIds)`ソリューションは非常に面白そうです。なぜなら、これはPHP 7の新しいコンセプトで、先頭にトリプレット文字を使用して関数に動的な数の引数を渡すことができるのです。そして、マージ処理は可能な限り効率的に行われ、すべてのロジックは内部でPHPによって自動的に処理されます。

短縮記法 `array_merge(...$finalIds)` は、空でない配列に対してのみ使用することができます。空の配列の場合，関数に引数は渡されず， `Function array_merge invoked with 0 parameters, at least 1 required.` というエラーを投げる．
