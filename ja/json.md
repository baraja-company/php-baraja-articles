PHPでのJson - 処理、生成、および書式設定
=========================

> id: '07729429-7201-4b3a-b991-4a66f9251161'
> slug:
> 	cs: json
> 	ja: phpdenojson-chu-li-sheng-cheng-oyobi-shu-shi-she-ding
> 
> perex:
> 	- JSON je datový typ pro předávání složitých datových struktur přes textový řetězec typicky jako API pro javascript.
> 	- JSONは、複雑なデータ構造をテキスト文字列で渡すためのデータ型であり、一般的にはjavascriptのAPIとして使用される。
> 
> publicationDate: '2019-11-26 11:32:43'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: c80746ea92c3dce77e890827e28d48b8

Jsonデータ形式は、クライアントとサーバー間で構造化されたデータの転送を容易にするために作成されました。

配列やオブジェクトをjsonに変換する
--------------------------------

配列やオブジェクトをJsonに変換するために、PHPには `json_encode` という関数があります。

```php
$user = [
	'名前' => 'ヤン',
	'苗字' => 'バラセック',
	'役割' => [
		'管理者',
		'モデレーター',
	],
];

echo json_encode($user);
```

生み出すだろう。

```txt
{"name":"Jan","surname":"Barasek","role":["admin","moderator"]}
```

json_encode()` 関数は他のデータ型も変換することができます。例えば、`integer` (整数)、`string` (文字列) などを直接挿入することができる。

Json pretty print - 人間が読みやすいように
--------------------------------------------

デフォルトの設定では、jsonは1つの長い文字列として生成されます。これは、なるべく場所を取らないようにするためです。

もし、より良い可読性を求めるのであれば、2番目のパラメータとして `JSON_PRETTY_PRINT` 定数を渡せばよいでしょう。

```php
echo json_encode($users, JSON_PRETTY_PRINT);
```

生み出すだろう。

```php
{
	"名前": "ヤン",
	"苗字": "バラセック",
	"役割": [
		"管理者",
		"モデレーター"
	]
}
```

jsonの書式設定 - カスタム書式ルール
------------------------------------------------

PHPによる通常のjsonの書式設定には追加設定がなく、この出力のみで解決する必要があります。例えば、スペースではなくタブをインデントしたい場合（あるいは何らかの方法で生成規則を変更したい場合）、自分でプログラムしなければならない。

この関数はすでに基本的な書式設定を行うことができるので、あとはそれを修正するだけです。

```php
function prettyJsonPrint(string $json): string
{
    $result = '';
    $level = 0;
    $in_quotes = false;
    $in_escape = false;
    $ends_line_level = NULL;
    $json_length = strlen($json);

    for ($i = 0; $i < $json_length; $i++) {
        $char = $json[$i];
        $new_line_level = NULL;
        $post = '';
        if ($ends_line_level !== NULL) {
            $new_line_level = $ends_line_level;
            $ends_line_level = NULL;
        }
        if ($in_escape) {
            $in_escape = false;
        } else if ($char === '"') {
            $in_quotes = !$in_quotes;
        } else if (!$in_quotes) {
            switch ($char) {
                case '}':
                case ']':
                    $level--;
                    $ends_line_level = NULL;
                    $new_line_level = $level;
                    break;

                case '{':
                case '[':
                    $level++;
                case ',':
                    $ends_line_level = $level;
                    break;

                case ':':
                    $post = '';
                    break;

                case "":
                case "\t":
                case "\n":
                case "\r":
                    $char = '';
                    $ends_line_level = $new_line_level;
                    $new_line_level = NULL;
                    break;
            }
        } else if ($char === '\\') {
            $in_escape = true;
        }
        if ($new_line_level !== NULL) {
            $result .= "\n" . str_repeat("\t", $new_line_level);
        }
        $result .= $char . $post;
    }

    return $result;
}
```

提供元：https://stackoverflow.com/questions/6054033/pretty-printing-json-with-php/9776726#9776726

例えば、入力。

```php
{"キー1":[1,2,3],"キー2":"価値"}
```

としてフォーマットされています。

```php
{
	"キー1": [
		1,
		2,
		3
	],
	"キー2": "価値"
}
```

というのは、各要素のインデントが統一されているため、より読みやすくなっています。

json記法をHTMLとして色付けする
------------------------------

キーやデータそのもの、個々の書式要素（特に閉じ括弧）に色を付けると便利な場合があります。

例えば、入力。

```php
{"アクセス": {"トークン": {"issued_at": "2008-08-16T14:10:31.309353", "ぜついん": "2008-08-17T14:10:31Z", "アイド": "MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"}, "サービスカタログ": [], "ユーザー": {"ユーザー名": "エイジェイ", "ロールリンク": [], "アイド": "16452ca89", "やくまわり": [], "名前": "エイジェイ"}}}
```

としてフォーマットされています。

{<br
&nbsp;&nbsp;<span style="color:#C22;">"access"</span>:&nbsp;{<br>」となります。
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"token"</span>:&nbsp;{<br>」となります。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"issued_at"</span>:&nbsp;<span style="color:#35D;">"2008-08-16T14:10:31.309353"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"expires"</span>:&nbsp;<span style="color:#35D;">"2008-08-17T14:10:31Z"</span>,&nbsp;<br> <br
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"id"</span>:&nbsp;<span style="color:#35D;">"MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"</span><br> <span>は。
&nbsp;&nbsp;&nbsp;&nbsp;},<br> <br>。
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"serviceCatalog"</span>:&nbsp;[<br>].
&nbsp;&nbsp;&nbsp;],&nbsp;<br> <br>。
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"user"</span>:&nbsp;{<br>」のようになります。
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"username"</span>:&nbsp;<span style="color:#35D;">"ajay"</span>,&nbsp;<br> <br> <span style="color:#35D;">"username"</span
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"roles_links"</span>:&nbsp;[<br>].
&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br> <br>。
&nbsp;&nbsp;&nbsp;&nbsp;<span style="id">:&nbsp;<span style="color:#35D;">"16452ca89"</span>,&nbsp;<br> <span style="color:#C22;"><span style="span">、</span>。
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">「役割」</span>:&nbsp;[<br>][<br>][<br>][<br>]。
&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br> <br>。
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">「名前」</span>:&nbsp;<span style="color:#35D;">"ajay"</span><br> <span style="color:#35D;">「名前」</span
&nbsp;&nbsp;&nbsp;&nbsp;}<br
&nbsp;&nbsp;}<br>。
}

これは、以下のような機能で実現できます。

```php
function jsonColorFormater(string $json, string $indentation = "\t"): string
{
    $crl = 0;
    $ss = false;
    $buffer = '';
    for ($c = 0; $c < strlen($json); $c++) {
        if ($json[$c] == '}' || $json[$c] == ']') {
            $crl--;
            $buffer .= "\n" . str_repeat($indentation, $crl);
        }
        if ($json[$c] == '"' && (@$json[$c - 1] == ',' || @$json[$c - 2] == ',')) {
            $buffer .= "\n" . str_repeat($indentation, $crl);
        }
        if ($json[$c] == '"' && !$ss) {
            $buffer .= '<span style="color:'.((@$json[$c - 1] == ':' || @$json[$c - 2] == ':') ? '#35D' : '#C22').';">';
        }
        $buffer .= $json[$c];
        if ($json[$c] == '"' && $ss) $buffer .= '</span>。';
        if ($json[$c] == '"') $ss = !$ss;
        if ($json[$c] == '{' || $json[$c] == '[') {
            $crl++;
            $buffer .= "\n". str_repeat($indentation, $crl);
        }
    }

    // HTMLソースを返す
    return '<pre>' . $buffer . '</pre> <pre>';
}

// これを呼び出すだけで、フォーマットされた出力が返される
// echo jsonColorFormater($data, ' ');
```

https://stackoverflow.com/a/20953262/6777550 から引用し、根本的に書き直した機能です。


JSONからPHPの配列/オブジェクトに変換する
----------------------

これは、json_decode()関数で実装でき、オブジェクトとして扱う必要のあるJsonからデータ構造を作ることができます。

```php
$json = '{name": "Jan", "surname": "Barasek", "role": ["admin", "moderator"]}.';
$decode = json_decode($json);

echo $decode->name; // 'Jan'を返します。

// echo $decode->role;
//
// プロパティロールのため、これは不可能です
// 配列を含むので、反復処理する必要があります。

echo '<ul>';
foreach ($decode->role as $role) {
    echo '<li>。' . $role . '</li>。'; // 箇条書きの後ろに役割を連続的に並べる
}
echo '</ul> </p';
```

javascriptでの処理
------------------------

例えば、jQueryライブラリでは、json文字列は非常に簡単にオブジェクトにパースすることができます。

```php
var json = '{name": "Jan", "surname": "Barasek", "role": ["admin", "moderator"]}.';
var parser = $.parseJSON(json);

document.write('名前' + parser.name);
console.log(parser); // デバッグのため、オブジェクト全体をコンソールに出力する
```

javascriptでは、一般的に、どんなjsonも有効なjavascriptオブジェクトであり、直接操作したり、そのプロパティにアクセスしたりすることができます。
