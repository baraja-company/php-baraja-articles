PHPのローカル変数
==========

> id: '9c3fbf4e-8612-4599-8127-d66b9ec5e980'
> slug:
> 	cs: lokalni-promenna
> 	ja: phpnorokaru-bian-shu
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: cffee22c4dbcc2ec331b73235349c409

ローカル変数は、**<a href="/prikazy-a-function">function</a>** または **method** (in object-oriented programming) の本体内部でのみ有効である。

通常のスクリプトの文脈で作業している場合は、すべてが予想通りに行われます。

```php
$x = 5;

echo $x; // 5を表示します。
```

しかし、カスタム関数を定義すると、動作が少し変わってきます。

```php
$x = 5;

function mojeFunkce(): int
{
    $x = 3;

    echo $x; // 3を表示します。
}

echo $x;     // 5を表示します。
```

なぜなら、$x 変数は現在の関数またはメソッドのコンテキストにのみ存在するからです。この行動は意図的なものです。

周囲のコードから関数に値を渡したい場合は、必要なパラメータを付けて呼び出す必要があります。

```php
echo mojeFunkce(5);	// 6を表示します。

function mojeFunkce(int $x): int
{
    return $x + 1;
}
```

関数に値を入力するには、パラメータを使用します。もし、パラメータ以外の追加の変数を関数に渡したい場合は、<a href="/global-variable">グローバル変数</a>を使用することができます。

> ローカル変数の使用は、より大きなアプリケーションをプログラミングする際に、大きな違いをもたらします。文脈によって変数の有効性を区別しなければ、ある変数がそれを当てにしていない場所で上書きされないことを保証することは不可能です（これがグローバル変数が悪である理由です）。

```php
$x = 5;
$y = 3;

function soucet(int $x, int $y): int
{
    return $x + $y;
}

echo $x;             // 5を表示します。
echo soucet($x, $y); // 8を表示します。
```
