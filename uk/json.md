Json в PHP - обробка, генерація та форматування
===============================================

> id: '07729429-7201-4b3a-b991-4a66f9251161'
> slug:
> 	cs: json
> 	uk: json-v-php-obrobka-generaciya-ta-formatuvannya
> 
> perex:
> 	- JSON je datový typ pro předávání složitých datových struktur přes textový řetězec typicky jako API pro javascript.
> 	- 'JSON - це тип даних для передачі складних структур даних через текстовий рядок, як правило, як API для javascript.'
> 
> publicationDate: '2019-11-26 11:32:43'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: c80746ea92c3dce77e890827e28d48b8

Формат даних Json був створений для полегшення передачі структурованих даних між клієнтом і сервером.

Перетворення масиву або об'єкту в json
--------------------------------

Для перетворення масиву або об'єкта в Json в PHP існує функція `json_encode`:

```php
$user = [
	'ім'я' => 'Ян',
	'прізвище' => 'Барасек',
	'роль' => [
		'Адміністратор',
		'модератор',
	],
];

echo json_encode($user);
```

Він згенерує:

```txt
{"name":"Jan","surname":"Barasek","role":["admin","moderator"]}
```

Функція `json_encode()` може також конвертувати інші типи даних. Наприклад, ми можемо безпосередньо вставити `integer` (ціле число), `string` (рядок) і так далі.

Json гарний шрифт - краща читабельність для людини
--------------------------------------------

За замовчуванням json генерується як один довгий рядок. Це зроблено для того, щоб займати якомога менше місця.

Якщо вам потрібна краща читабельність для людини, просто передайте константу `JSON_PRETTY_PRINT` як другий параметр:

```php
echo json_encode($users, JSON_PRETTY_PRINT);
```

Він згенерує:

```php
{
	"ім'я": "Ян",
	"прізвище": "Барасек",
	"роль": [
		"Адміністратор",
		"модератор"
	]
}
```

Форматування json - користувацькі правила форматування
------------------------------------------------

Звичайне форматування json через PHP не має додаткових налаштувань і доводиться задовольнятися тільки таким виводом. Наприклад, якщо ми хочемо зробити відступ табуляції замість пробілів (або якимось чином змінити правила генерації), нам доведеться програмувати це самостійно.

Ця функція вже вміє робити базове форматування і нам просто потрібно її модифікувати:

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

Джерело: https://stackoverflow.com/questions/6054033/pretty-printing-json-with-php/9776726#9776726

Наприклад, вхідні:

```php
{"key1":[1,2,3],"ключ2":"значення"}
```

Відформатовано як:

```php
{
	"key1": [
		1,
		2,
		3
	],
	"ключ2": "значення"
}
```

Який набагато легше читається (завдяки послідовному відступу кожного елементу).

Розфарбовування json нотації як HTML
------------------------------

Іноді буває корисно виділити кольором клавіші, самі дані та окремі елементи форматування (особливо дужки, що закриваються).

Наприклад, вхідні:

```php
{"доступ": {"маркер": {"видано_на": "2008-08-16T14:10:31.309353", "закінчується": "2008-08-17T14:10:31Z", "ідентифікатор": "MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"}, "СервісКаталог": [], "користувач": {"ім'я користувача": "сойка", "roles_links": [], "ідентифікатор": "16452ca89", "ролі": [], "ім'я": "сойка"}}}
```

Відформатовано як:

{<br>
&nbsp;&nbsp;<span style="color:#C22;">"доступ"</span>:&nbsp;{{br>}
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"токен"</span>:&nbsp;{<br> <br> </span
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"issued_at"</span>:&nbsp;<span style="color:#35D;">"2008-08-16T14:10:31.309353"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"закінчується"</span>:&nbsp;<span style="color:#35D;">"2008-08-17T14:10:31Z"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;"><span style="color:#C22;">"id"</span>:&nbsp;<span style="color:#35D;">"MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"</span><br></span
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br>.
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"Каталог послуг"</span>:&nbsp;[<br>[<br
&nbsp;&nbsp;&nbsp;],&nbsp;<br>.
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"користувач"</span>:&nbsp;{{br>}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"username"</span>:&nbsp;<span style="color:#35D;">"ajay"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"roles_links"</span>:&nbsp;[<br>[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br></b>.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;"><span style="id">:&nbsp;<span style="color:#35D;">"16452ca89"</span>,&nbsp;<br>.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"ролі"</span>:&nbsp;[<br>[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br></b>.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"name"</span>:&nbsp;<span style="color:#35D;">"ajay"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>
&nbsp;&nbsp;}<br></b>.
}

Це може бути реалізовано за допомогою наступної функції:

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
        if ($json[$c] == '"' && $ss) $buffer .= '</span> </span>.';
        if ($json[$c] == '"') $ss = !$ss;
        if ($json[$c] == '{' || $json[$c] == '[') {
            $crl++;
            $buffer .= "\n". str_repeat($indentation, $crl);
        }
    }

    // Повертає HTML-джерело
    return '<pre>' . $buffer . '</pre> </pre';
}

// Просто викликати цю функцію і повернути відформатований вивід
// echo jsonColorFormater($data, ' ');
```

Матеріал взятий і докорінно переписаний з: https://stackoverflow.com/a/20953262/6777550


Конвертувати з JSON в PHP масив/об'єкт
----------------------

Це можна реалізувати за допомогою функції json_decode(), яка робить з Json структуру даних, з якою ми маємо працювати як з об'єктом:

```php
$json = '{"author": "Jan", "прізвище": "Barasek", "role":["admin", "moderator"]}.';
$decode = json_decode($json);

echo $decode->name; // Повертає 'Jan'

// echo $decode->role;
//
// Це неможливо, тому що роль власності
// містить масив, потрібно виконати ітерацію.

echo '<ul>';
foreach ($decode->role as $role) {
    echo '<li>' . $role . '</li> </li> </ul>.'; // Послідовно перераховує ролі, що стоять за кулями
}
echo '</ul> </li> <ul>.';
```

Обробка на javascript
------------------------

Наприклад, в бібліотеці jQuery json-рядок дуже легко розбирається в об'єкт:

```php
var json = '{"author": "Jan", "прізвище": "Barasek", "role":["admin", "moderator"]}.';
var parser = $.parseJSON(json);

document.write('Ім'я:' + parser.name);
console.log(parser); // Виводить весь об'єкт в консоль для налагодження
```

У javascript, в загальному випадку, будь-який json є дійсним об'єктом javascript, з яким можна працювати безпосередньо та отримувати доступ до його властивостей.
