End()
=====

> id: '879c7c9a-a497-4080-865d-f15e6c9e8578'
> slug:
> 	cs: end
> 	ja: end
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '2d097dd1fae4e4f125d879c92bc2cfbe'

- PHP 4、PHP 5に対応
- 簡単な説明：配列の内部ポインタを最後の要素に設定する。
- 必要条件

商品説明
--------------------------

end()` 関数は、最後の要素への内部配列ポインタをセットし、その値を返します。

類似機能
--------------------------

- `current()`
- `each()`
- `prev()`
- <a href="/reset">リセット()</a>を行う。
- `next()`

例
--------------------------

```php
echo end([
    'アップル',
    'バナナ',
    'クランベリー',
]); // クランベリーを書き出す
```



```php
$a = [];
$a[1] = 1;
$a[0] = 0;

echo end($a); // 0を表示する
```

パラメータ
--------------------------

| タイプ｜説明｜#
| --- | ------- | ----- |
| 1｜`array`｜作業する配列の名前です。

戻り値
--------------------------

最後の要素の値を返します．空の配列の場合は `false` を返します．

旧バージョンとの相違点
--------------------------

なし
