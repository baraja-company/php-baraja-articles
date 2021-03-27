Json v PHP - zpracování, generování a formátování
================================

> id: 07729429-7201-4b3a-b991-4a66f9251161
> slugCS: json
> perex: JSON je datový typ pro předávání složitých datových struktur přes textový řetězec typicky jako API pro javascript.
> publicationDate: 2019-11-26 11:32:43
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9

Pro snadný přenos strukturovaných dat mezi klientem a serverem vznikl datový formát Json.

Převod pole nebo objektu na json
--------------------------------

Pro převod pole nebo objektu na Json existuje v PHP funkce `json_encode`:

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

Vygeneruje:

```
{"name":"Jan","surname":"Barasek","role":["admin","moderator"]}
```

Funkce `json_encode()` zvládá převést i další datové typy. Můžeme tak například přímo vložit `integer` (celé číslo), `string` (řetězec) a podobně.

Json pretty print - lepší čitelnost člověkem
--------------------------------------------

Ve výchozí konfiguraci se json generuje jako jeden dlouhý řetězec. To z toho důvodu, aby zabíral co nejméně místa.

Pokud vyžadujete lepší čitelnost člověkem, stačí předat druhým parametrem konstantu `JSON_PRETTY_PRINT`:

```php
echo json_encode($users, JSON_PRETTY_PRINT);
```

Vygeneruje:

```php
{
	"name": "Jan",
	"surname": "Barasek",
	"role": [
		"admin",
		"moderator"
	]
}
```

Formátování jsonu - vlastní formátovací pravidla
------------------------------------------------

Běžné formátování Jsonu přes PHP nemá žádné další nastavení a musíme se spokojit pouze s tímto výstupem. Pokud chceme například místo mezer odsazovat tabulátory (nebo jakkoli pozměnit pravidla generování), musíme si to naprogramovat sami.

Tato funkce umí již základní formátování a stačí ji jen pozměnit:

```php
function prettyJsonPrint($json) {
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

Zdroj: https://stackoverflow.com/a/9776726/6777550

Například vstup:

```php
{"key1":[1,2,3],"key2":"value"}
```

Naformátuje jako:

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

Což je mnohem lépe čitelné (kvůli důslednému odsazení každého prvku).

Obarvení json zápisu jako HTML
------------------------------

Někdy se může hodit barevné zvýraznění klíčů, samotných dat a také jednotlivých formátovacích prvků (zejména uzavírací závorky).

Například vstup:

```php
{"access": {"token": {"issued_at": "2008-08-16T14:10:31.309353", "expires": "2008-08-17T14:10:31Z", "id": "MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"}, "serviceCatalog": [], "user": {"username": "ajay", "roles_links": [], "id": "16452ca89", "roles": [], "name": "ajay"}}}
```


Naformátuje jako:

{<br>
&nbsp;&nbsp;<span style="color:#C22;">"access"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"token"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"issued_at"</span>:&nbsp;<span style="color:#35D;">"2008-08-16T14:10:31.309353"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"expires"</span>:&nbsp;<span style="color:#35D;">"2008-08-17T14:10:31Z"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"id"</span>:&nbsp;<span style="color:#35D;">"MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;},&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"serviceCatalog"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"user"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"username"</span>:&nbsp;<span style="color:#35D;">"ajay"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"roles_links"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"id"</span>:&nbsp;<span style="color:#35D;">"16452ca89"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"roles"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"name"</span>:&nbsp;<span style="color:#35D;">"ajay"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;}<br>
&nbsp;&nbsp;}<br>
}

To lze implementovat následující funkcí:

```php
function jsonColorFormater($json, $indentation = "\t") {
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
        if ($json[$c] == '"' && $ss) $buffer .= '</span>';
        if ($json[$c] == '"') $ss = !$ss;
        if ($json[$c] == '{' || $json[$c] == '[') {
            $crl++;
            $buffer .= "\n". str_repeat($indentation, $crl);
        }
    }

    // Vrátí HTML zdroj
    return '<pre>' . $buffer . '</pre>';
}

// Takto stačí zavolat a vrátí formátovaný výstup
// echo jsonColorFormater($data, '  ');
```


Funkce převzata a radikálně přepsána z: https://stackoverflow.com/a/20953262/6777550


Převod z JSON do PHP pole / objektu
----------------------

Lze realizovat funkcí json_decode(), která z Jsonu vyrobí datovou strukturu, s kterou musíme pracovat, jako s objektem:

```php
$json = '{"name":"Jan","surname":"Barasek","role":["admin","moderator"]}';
$decode = json_decode($json);

echo $decode->name; // Vrátí 'Jan'

// echo $decode->role;
//
// Toto nejde, protože property role
// obsahuje pole, musíme iterovat.

echo '<ul>';
foreach ($decode->role as $role) {
    echo '<li>' . $role . '</li>'; // Postupně vypíše role za odrážky
}
echo '</ul>';
```

Zpracování v javascriptu
------------------------

Například v knihovně jQuery lze řetězec s json naparsovat do objektu velice snadno:

```php
var json = '{"name":"Jan","surname":"Barasek","role":["admin","moderator"]}';
var parser = $.parseJSON(json);

document.write('Jméno: ' + parser.name);
console.log(parser); // Vypíše do konzole celý objekt pro možnost ladění
```

V javascriptu obecně platí, že jakýkoli json je validní javascriptový objekt, s kterým lze pracovat přímo a přistupovat k jeho properties.
