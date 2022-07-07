PHP中的Json - 处理、生成和格式化
=====================

> id: '07729429-7201-4b3a-b991-4a66f9251161'
> slug:
> 	cs: json
> 	zh: php-zhong-dejson-chu-li-sheng-cheng-he-ge-shi-hua
> 
> perex:
> 	- JSON je datový typ pro předávání složitých datových struktur přes textový řetězec typicky jako API pro javascript.
> 	- JSON是一种数据类型，用于在文本字符串上传递复杂的数据结构，通常作为javascript的API。
> 
> publicationDate: '2019-11-26 11:32:43'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: c80746ea92c3dce77e890827e28d48b8

创建Json数据格式是为了方便在客户端和服务器之间传输结构化数据。

将一个数组或对象转换为json
--------------------------------

为了将数组或对象转换为Json，在PHP中有一个函数`json_encode`。

```php
$user = [
	'名称' => '扬',
	'姓氏' => '巴拉塞克',
	'作用' => [
		'管理员',
		'主持人',
	],
];

echo json_encode($user);
```

它将产生。

```txt
{"name":"Jan","surname":"Barasek","role":["admin","moderator"]}
```

`json_encode()`函数也可以转换其他数据类型。例如，我们可以直接插入`integer`（整数），`string`（字符串），等等。

Json漂亮的打印 - 更好的人类可读性
--------------------------------------------

在默认配置中，json被生成为一个长字符串。这是因为要尽可能少地占用空间的原因。

如果你需要更好的人类可读性，只需传递`JSON_PRETTY_PRINT`常量作为第二个参数。

```php
echo json_encode($users, JSON_PRETTY_PRINT);
```

它将产生。

```php
{
	"名称": "扬",
	"姓氏": "巴拉塞克",
	"作用": [
		"管理员",
		"主持人"
	]
}
```

格式化json - 自定义格式化规则
------------------------------------------------

通过PHP对json的正常格式化，没有额外的设置，我们只能满足于这种输出。例如，如果我们想缩进制表符而不是空格（或以任何方式修改生成规则），我们必须自己编程。

这个函数已经可以做基本的格式化，我们只需要修改它。

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

来源 : https://stackoverflow.com/questions/6054033/pretty-printing-json-with-php/9776726#9776726

例如，输入。

```php
{"钥匙1":[1,2,3],"钥匙2":"价值"}
```

格式化为。

```php
{
	"钥匙1": [
		1,
		2,
		3
	],
	"钥匙2": "价值"
}
```

这就更容易阅读了（因为每个元素的缩进一致）。

将json符号着色为HTML
------------------------------

有时，用颜色突出显示键、数据本身和个别格式化元素（特别是闭合括号）是很有用的。

例如，输入。

```php
{"访问": {"象征性的": {"发布时间": "2008-08-16T14:10:31.309353", "过期": "2008-08-17T14:10:31Z", "身份证": "MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"}, "服务目录": [], "用户": {"帐号": "ajay", "角色链接": [], "身份证": "16452ca89", "角色": [], "名称": "ajay"}}}
```

格式化为。

{<br
&nbsp;&nbsp;<span style="color:#C22;">"访问"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"token"</span>：&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"issued_at"</span>:&nbsp;<span style="color:#35D;">"2008-08-16T14:10:31.309353"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"expires"</span>:&nbsp;<span style="color:#35D;">"2008-08-17T14:10:31Z"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"id"</span>：&nbsp;<span style="color:#35D;">"MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;},<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"serviceCatalog"</span>:&nbsp;[br>
&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"user"</span>：&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"用户名"</span>：&nbsp;<span style="color:#35D;">"ajay"</span>，&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"role_links"</span>:&nbsp;[<br>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]，&nbsp;<br>
<span style="color:#C22;"><span style="id">:&nbsp;<span style="color:#35D;">"16452ca89"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"角色"</span>：&nbsp;[br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]，&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"name"</span>：&nbsp;<span style="color:#35D;">"ajay"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;}<br>。
&nbsp;&nbsp;}<br>
}

这可以通过以下函数来实现。

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
        if ($json[$c] == '"' && $ss) $buffer .= '</SPAN>';
        if ($json[$c] == '"') $ss = !$ss;
        if ($json[$c] == '{' || $json[$c] == '[') {
            $crl++;
            $buffer .= "\n". str_repeat($indentation, $crl);
        }
    }

    // 返回HTML源代码
    return '<pre>' . $buffer . '</pre>';
}

//只需调用这个并返回格式化的输出
// echo jsonColorFormater($data, ' ');
```

特征取自并彻底改写自: https://stackoverflow.com/a/20953262/6777550


从JSON转换为PHP数组/对象
----------------------

这可以用json_decode()函数来实现，它将Json做成一个数据结构，我们必须将其作为一个对象来处理。

```php
$json = '{"名"："Jan", "姓"："Barasek", "角色"：["管理员", "版主"]}。';
$decode = json_decode($json);

echo $decode->name; // 返回 "Jan"。

// echo $decode->role;
//
// 这是不可能的，因为属性角色
//包含一个数组，我们需要进行遍历。

echo '';
foreach ($decode->role as $role) {
    echo '<li>' . $role . '</li>'; // 连续列出子弹背后的角色
}
echo '</ul>';
```

在javascript中处理
------------------------

例如，在jQuery库中，一个json字符串可以非常容易地被解析成一个对象。

```php
var json = '{"名"："Jan", "姓"："Barasek", "角色"：["管理员", "版主"]}。';
var parser = $.parseJSON(json);

document.write('名字。' + parser.name);
console.log(parser); // 将整个对象打印到控制台以进行调试
```

在javascript中，一般来说，任何json都是一个有效的javascript对象，可以直接操作，也可以访问其属性。
