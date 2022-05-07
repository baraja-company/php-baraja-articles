Json v PHP - spracovanie, generovanie a formátovanie
====================================================

> id: '07729429-7201-4b3a-b991-4a66f9251161'
> slug:
> 	cs: json
> 	sk: json-v-php---spracovanie-generovanie-a-formatovanie
> 
> perex: JSON je datový typ pro předávání složitých datových struktur přes textový řetězec typicky jako API pro javascript.
> publicationDate: '2019-11-26 11:32:43'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: c80746ea92c3dce77e890827e28d48b8

Dátový formát Json bol vytvorený na uľahčenie prenosu štruktúrovaných údajov medzi klientom a serverom.

Konverzia poľa alebo objektu na json
--------------------------------

Na konverziu poľa alebo objektu do Json je v PHP k dispozícii funkcia `json_encode`:

```php
$user = [
	'name' => "John,
	'priezvisko' => "Barasek,
	'role' => [
		'admin',
		'moderátor',
	],
];

echo json_encode($user);
```

Vytvorí:

{"name":"Jan","surname":"Barasek","role":["admin","moderator"]}

Funkcia `json_encode()` dokáže konvertovať aj iné dátové typy. Môžeme napríklad priamo vložiť `integer` (celé číslo), `string` (reťazec) atď.

Json pretty print - lepšia čitateľnosť pre človeka
--------------------------------------------

V predvolenej konfigurácii sa json generuje ako jeden dlhý reťazec. Je to z dôvodu, aby zaberali čo najmenej miesta.

Ak požadujete lepšiu čitateľnosť, stačí ako druhý parameter odovzdať konštantu `JSON_PRETTY_PRINT`:

```php
echo json_encode($users, JSON_PRETTY_PRINT);
```

Vytvorí:

```php
{
	"meno": "John.",
	"priezvisko": "Barasek",
	"role": [
		"admin",
		"moderátor"
	]
}
```

Formátovanie json - vlastné pravidlá formátovania
------------------------------------------------

Bežné formátovanie json prostredníctvom PHP nemá žiadne ďalšie nastavenia a musíme sa uspokojiť len s týmto výstupom. Ak napríklad chceme namiesto medzier odsadiť tabulátory (alebo akokoľvek upraviť pravidlá generovania), musíme si to naprogramovať sami.

Táto funkcia už dokáže vykonávať základné formátovanie a my ju musíme len upraviť:

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

Zdroj: https://stackoverflow.com/questions/6054033/pretty-printing-json-with-php/9776726#9776726

Zadajte napríklad:

```php
{"key1":[1,2,3],"key2":"hodnota"}
```

Formátované ako:

```php
{
	"key1": [
		1,
		2,
		3
	],
	"key2": "hodnota"
}
```

Čo je oveľa ľahšie čitateľné (vďaka dôslednému odsadeniu jednotlivých prvkov).

Zafarbenie zápisu json ako HTML
------------------------------

Niekedy môže byť užitočné farebne zvýrazniť kľúče, samotné údaje a jednotlivé formátovacie prvky (najmä uzatváracie zátvorky).

Zadajte napríklad:

```php
{"prístup": {"token": {"issued_at": "2008-08-16T14:10:31.309353", "vyprší": "2008-08-17T14:10:31Z", "id": "MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"}, "serviceCatalog": [], "user": {"užívateľské meno": "ajay", "roles_links": [], "id": "16452ca89", "roly": [], "meno": "ajay"}}}
```

Formátované ako:

{<br>
&nbsp;&nbsp;<span style="color:#C22;">"prístup"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"token"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"issued_at"</span>:&nbsp;<span style="color:#35D;">"2008-08-16T14:10:31.309353"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"expires"</span>:&nbsp;<span style="color:#35D;">"2008-08-17T14:10:31Z"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;"><span style="color:#C22;">"id"</span>:&nbsp;<span style="color:#35D;">"MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;},<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"serviceCatalog"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;], &nbsp;<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"user"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"užívateľské meno"</span>:&nbsp;<span style="color:#35D;">"ajay"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"roles_links"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;], &nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;"><span style="id">:&nbsp;<span style="color:#35D;">"16452ca89"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"roly"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;], &nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"meno"</span>:&nbsp;<span style="color:#35D;">"ajay"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;}<br>
&nbsp;&nbsp;}<br>
}

To možno realizovať pomocou nasledujúcej funkcie:

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
            $buffer .= '<span style='color:'.((@$json[$c - 1] == ':' || @$json[$c - 2] == ':') ? '#35D' : '#C22').';">';
        }
        $buffer .= $json[$c];
        if ($json[$c] == '"' && $ss) $buffer .= '</span>';
        if ($json[$c] == '"') $ss = !$ss;
        if ($json[$c] == '{' || $json[$c] == '[') {
            $crl++;
            $buffer .= "\n". str_repeat($indentation, $crl);
        }
    }

    // Vracia zdroj HTML
    return '<pre>' . $buffer . '</pre>';
}

// Stačí zavolať túto funkciu a vrátiť formátovaný výstup
// echo jsonColorFormater($data, ' ');
```

Funkcia prevzatá a radikálne prepracovaná z: https://stackoverflow.com/a/20953262/6777550


Prevod z JSON na pole/objekt PHP
----------------------

To možno realizovať pomocou funkcie json_decode(), ktorá z Json vytvorí dátovú štruktúru, s ktorou musíme pracovať ako s objektom:

```php
$json = '{"name": "Jan", "surname": "Barasek", "role":["admin", "moderator"]}';
$decode = json_decode($json);

echo $decode->name; // Vracia 'Jan'

// echo $decode->role;
//
// Toto nie je možné, pretože rola vlastnosti
// obsahuje pole, ktoré musíme iterovať.

echo '<ul>';
foreach ($decode->role as $role) {
    echo '<li>' . $role . '</li>'; // Postupne uvádza roly za odrážkami
}
echo '</ul>';
```

Spracovanie v jazyku javascript
------------------------

Napríklad v knižnici jQuery možno reťazec json veľmi jednoducho analyzovať na objekt:

```php
var json = '{"name": "Jan", "surname": "Barasek", "role":["admin", "moderator"]}';
var parser = $.parseJSON(json);

document.write('Názov: ' + parser.name);
console.log(parser); // Vypíše celý objekt na konzolu na ladenie
```

V javascripte je vo všeobecnosti každý json platným javascriptovým objektom, s ktorým možno priamo pracovať a pristupovať k jeho vlastnostiam.
