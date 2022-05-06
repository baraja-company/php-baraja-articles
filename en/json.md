Json in PHP - processing, generation and formatting
===================================================

> id: '07729429-7201-4b3a-b991-4a66f9251161'
> slug:
> 	cs: json
> 	en: json-in-php---processing-generation-and-formatting
> 
> perex: JSON je datový typ pro předávání složitých datových struktur přes textový řetězec typicky jako API pro javascript.
> publicationDate: '2019-11-26 11:32:43'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: c80746ea92c3dce77e890827e28d48b8

The Json data format was created to facilitate the transfer of structured data between client and server.

Converting an array or object to json
--------------------------------

To convert an array or object to Json, there is a function `json_encode` in PHP:

```php
$user = [
	'name' => 'Jan',
	'surname' => 'Barasek',
	'role' => [
		'admin',
		'moderator',
	],
];

echo json_encode($user);
```

Generates:

```
{"name": "Jan", "surname": "Barasek", "role":["admin", "moderator"]}
```

The `json_encode()` function can also convert other data types. For example, we can directly insert `integer` (integer), `string` (string) and so on.

Json pretty print - better human readability
--------------------------------------------

In the default configuration, json is generated as one long string. This is for the reason to take up as little space as possible.

If you require better human readability, just pass the `JSON_PRETTY_PRINT` constant as the second parameter:

```php
echo json_encode($users, JSON_PRETTY_PRINT);
```

Generates:

```php
{
	"name": "jan",
	"surname": "Barasek",
	"role": [
		"admin",
		"moderator".
	]
}
```

Formatting json - custom formatting rules
------------------------------------------------

The normal formatting of json via PHP has no additional settings and we have to be satisfied with this output only. For example, if we want to indent tabs instead of spaces (or modify the generation rules in any way), we have to program it ourselves.

This function can already do basic formatting and we just need to modify it:

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
                    $post = ' ';
                    break;

                case " ":
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

Source : https://stackoverflow.com/questions/6054033/pretty-printing-json-with-php/9776726#9776726

For example, input:

```php
{"key1":[1,2,3],"key2":"value"}
```

Formatted as:

```php
{
	"key1": [
		1,
		2,
		3
	],
	"key2": "value"
}
```

Which is much easier to read (because of the consistent indentation of each element).

Coloring json notation as HTML
------------------------------

Sometimes it can be useful to color highlight keys, the data itself, and individual formatting elements (especially closing brackets).

For example, input:

```php
{"access": {"token": {"issued_at": "2008-08-16T14:10:31.309353", "expires": "2008-08-17T14:10:31Z", "id": "MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"}, "serviceCatalog": [], "user": {"username": "ajay", "roles_links": [], "id": "16452ca89", "roles": [], "name": "ajay"}}}
```


Formatted as:

{<br>
&nbsp;&nbsp;<span style="color:#C22;">"access"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"token"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"issued_at"</span>:&nbsp;<span style="color:#35D;">"2008-08-16T14:10:31.309353"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"expires"</span>:&nbsp;<span style="color:#35D;">"2008-08-17T14:10:31Z"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;"><span style="color:#C22;">"id"</span>:&nbsp;<span style="color:#35D;">"MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;},<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"serviceCatalog"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"user"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"username"</span>:&nbsp;<span style="color:#35D;">"ajay"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"roles_links"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;"><span style="id">:&nbsp;<span style="color:#35D;">"16452ca89"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"roles"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"name"</span>:&nbsp;<span style="color:#35D;">"ajay"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;}<br>
&nbsp;&nbsp;}<br>
}

This can be implemented by the following function:

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
        if ($json[$c] == '"' && (@$json[$c - 1] == ',' || @$json[$c - 2] == ',') {
            $buffer .= "\n" . str_repeat($indentation, $crl);
        }
        if ($json[$c] == '"' && !$ss) {
            $buffer .= '<span style="color:'.((@$json[$c - 1] == ':' || @$json[$c - 2] == ':') ? '#35D' : '#C22').';">';
        }
        $buffer .= $json[$c];
        if ($json[$c] == '"' && $ss) $buffer .= '</span>';
        if ($json[$c] == '"') $ss = !
        if ($json[$c] == '{' || $json[$c] == '[') {
            $crl++;
            $buffer .= "\n". str_repeat($indentation, $crl);
        }
    }

    // Returns the HTML source
    return '<pre>' . $buffer . '</pre>';
}

// Just call this and it will return formatted output
// echo jsonColorFormater($data, ' ');
```


Function taken and radically rewritten from: https://stackoverflow.com/a/20953262/6777550


Convert from JSON to PHP array/object
----------------------

This can be implemented with the json_decode() function, which makes a data structure out of Json that we need to work with as an object:

```php
$json = '{"name": "Jan", "surname": "Barasek", "role":["admin", "moderator"]}';
$decode = json_decode($json);

echo $decode->name; // Returns 'Jan'

// echo $decode->role;
//
// This doesn't work because property role
// contains an array, we have to iterate.

echo '<ul>';
foreach ($decode->role as $role) {
    echo '<li>' . $role . '</li>'; // Sequentially lists the roles after the bullets
}
echo '</ul>';
```

Processing in javascript
------------------------

For example, in the jQuery library, a json string can be parsed into an object very easily:

```php
var json = '{"name": "Jan", "surname": "Barasek", "role":["admin", "moderator"]}';
var parser = $.parseJSON(json);

document.write('Name: ' + parser.name);
console.log(parser); // Prints the entire object to the console for debugging
```

In javascript, in general, any json is a valid javascript object that can be worked with directly and its properties accessed.
