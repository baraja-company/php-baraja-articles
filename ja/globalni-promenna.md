PHPのグローバル変数
===========

> id: '1cdfc51a-31f4-4a5d-81f6-cfd92f86a9d4'
> slug:
> 	cs: globalni-promenna
> 	ja: phpnogurobaru-bian-shu
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '31e78a9abf19252ef62315230a3731b5'

グローバル変数は、アプリケーションのどの部分でもいつでも利用可能であり、渡す必要はありません。

> 警告:** 適切に設計されたアプリケーションは、グローバル変数を使用すべきではありません。なぜなら、グローバル変数はカプセル化の原則に違反し、不用意に扱うと検出しにくいエラーを引き起こす可能性があるからです。

使用例

```php
$a = 1;
$b = 2;

function suma(): void
{
	global $a, $b;
	$b = $a + $b;
}

suma();

echo $b; // 変数 $b はグローバルなので、数字 3 を表示します。
```

変数 `$a` と `$b` を自然な文脈の外で得ていることに注意してください。この動作は「マジック」と呼ばれ、他の関数が現在使用中の変数を上書きすると、アプリケーションが予期せぬ状態になるためです。

正しく、アプリケーションは変数を**カプセル化**し、毎回渡す必要があります。

```php
$a = 1;
$b = 2;

function suma(int $a, int $b): int
{
	return $a + $b;
}

echo suma($a, $b); // 3を表示します。
```

このおかげで、異なる入力パラメータで動的に関数を呼び出すことができ、その出力は環境に依存せず、入力のみに依存することになります。

URLから入力パラメータを取得する
---------------------------------

おそらくグローバル変数の唯一の妥当な使用法は、ユーザー入力の解析で、その場合は <a href="/superglobal-variable">superglobal variables</a> について話しているのでしょう。

この場合、変数は書き込み専用ではなく、読み取り専用であるべきで、しかもアプリケーション全体で同じであるため、すっきりした設計になっています。

```php
function getNameFromUrl(): string
{
    return isset($_GET['名前'])
    	? htmlspecialchars($_GET['名前'])
    	: '';
}

echo getNameFromUrl();
```
