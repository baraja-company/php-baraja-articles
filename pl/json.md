Json w PHP - przetwarzanie, generowanie i formatowanie
======================================================

> id: '07729429-7201-4b3a-b991-4a66f9251161'
> slug:
> 	cs: json
> 	pl: json-w-php---przetwarzanie-generowanie-i-formatowanie
> 
> perex:
> 	- JSON je datový typ pro předávání složitých datových struktur přes textový řetězec typicky jako API pro javascript.
> 	- 'JSON to typ danych służący do przekazywania złożonych struktur danych za pomocą łańcucha tekstowego, zazwyczaj jako interfejs API dla języka javascript.'
> 
> publicationDate: '2019-11-26 11:32:43'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: c80746ea92c3dce77e890827e28d48b8

Format danych Json został stworzony w celu ułatwienia przesyłania ustrukturyzowanych danych między klientem a serwerem.

Konwersja tablicy lub obiektu do formatu json
--------------------------------

Aby przekonwertować tablicę lub obiekt na Json, w PHP istnieje funkcja `json_encode`:

```php
$user = [
	'nazwa' => 'Jan',
	'nazwisko' => 'Barasek',
	'rola' => [
		'Administrator',
		'moderator',
	],
];

echo json_encode($user);
```

Będzie generować:

```txt
{"name":"Jan","surname":"Barasek","role":["admin","moderator"]}
```

Funkcja `json_encode()` może również konwertować inne typy danych. Na przykład, możemy bezpośrednio wstawić `integer` (liczba całkowita), `string` (łańcuch znaków) i tak dalej.

Json ładny druk - lepsza czytelność dla człowieka
--------------------------------------------

W domyślnej konfiguracji json jest generowany jako jeden długi ciąg znaków. Dzieje się tak dlatego, aby zajmowały jak najmniej miejsca.

Jeśli wymagana jest lepsza czytelność dla człowieka, wystarczy przekazać stałą `JSON_PRETTY_PRINT` jako drugi parametr:

```php
echo json_encode($users, JSON_PRETTY_PRINT);
```

Będzie generować:

```php
{
	"nazwa": "Jan",
	"nazwisko": "Barasek",
	"rola": [
		"Administrator",
		"moderator"
	]
}
```

Formatowanie json - niestandardowe reguły formatowania
------------------------------------------------

Normalne formatowanie json za pomocą PHP nie ma żadnych dodatkowych ustawień i musimy zadowolić się tylko tym wyjściem. Na przykład, jeśli chcemy wciskać tabulatory zamiast spacji (lub w jakikolwiek sposób zmodyfikować reguły generowania), musimy sami to zaprogramować.

Funkcja ta potrafi już wykonywać podstawowe formatowanie, wystarczy ją tylko zmodyfikować:

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

Źródło: https://stackoverflow.com/questions/6054033/pretty-printing-json-with-php/9776726#9776726

Na przykład dane wejściowe:

```php
{"klawisz1":[1,2,3],"klucz2":"wartość"}
```

Sformatowane jako:

```php
{
	"klawisz1": [
		1,
		2,
		3
	],
	"klucz2": "wartość"
}
```

Jest to znacznie łatwiejsze do odczytania (ze względu na spójne wcięcie każdego elementu).

Kolorowanie notacji json jako HTML
------------------------------

Czasami przydatne może być podświetlenie kolorem klawiszy, samych danych oraz poszczególnych elementów formatowania (zwłaszcza nawiasów zamykających).

Na przykład dane wejściowe:

```php
{"dostęp": {"token": {"wydany_at": "2008-08-16T14:10:31.309353", "wygasa": "2008-08-17T14:10:31Z", "id": "MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"}, "serviceCatalog": [], "użytkownik": {"nazwa użytkownika": "ajay", "role_linki": [], "id": "16452ca89", "role": [], "nazwa": "ajay"}}}
```

Sformatowane jako:

{<br>
&nbsp;&nbsp;<span style="color:#C22;">"access"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"token"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"issued_at"</span>:&nbsp;<span style="color:#35D;">"2008-08-16T14:10:31.309353"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"expires"</span>:&nbsp;<span style="color:#35D;">"2008-08-17T14:10:31Z"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;"><span style="color:#C22;">"id"</span>:&nbsp;<span style="color:#35D;">"MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"serviceCatalog"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"user"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"nazwa użytkownika"</span>:&nbsp;<span style="color:#35D;">"ajay"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"role_links"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;"><span style="id">:&nbsp;<span style="color:#35D;">"16452ca89"</span>,&nbsp;<br>.
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"role"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"name"</span>:&nbsp;<span style="color:#35D;">"ajay"</span><br>.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>
&nbsp;&nbsp;}<br>
}

Można to zrobić za pomocą następującej funkcji:

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
        if ($json[$c] == '"' && $ss) $buffer .= '</span>.';
        if ($json[$c] == '"') $ss = !$ss;
        if ($json[$c] == '{' || $json[$c] == '[') {
            $crl++;
            $buffer .= "\n". str_repeat($indentation, $crl);
        }
    }

    // Zwraca źródło HTML
    return '<pre>.' . $buffer . '</pre>.';
}

// Po prostu wywołaj to i zwróć sformatowane dane wyjściowe
// echo jsonColorFormater($data, ' ');
```

Cechy zaczerpnięte i radykalnie przerobione z: https://stackoverflow.com/a/20953262/6777550


Konwersja z JSON na tablicę/obiekt PHP
----------------------

Można to zrealizować za pomocą funkcji json_decode(), która tworzy z Json strukturę danych, z którą musimy pracować jako z obiektem:

```php
$json = '{"name": "Jan", "surname": "Barasek", "role":["admin", "moderator"]}';
$decode = json_decode($json);

echo $decode->name; // Zwraca "Jan".

// echo $decode->role;
//
// Nie jest to możliwe, ponieważ rola właściwości
// zawiera tablicę, musimy wykonać iterację.

echo '<ul>.';
foreach ($decode->role as $role) {
    echo '<li>.' . $role . '</li>.'; // Kolejno wymienia role, które kryją się za kulami
}
echo '</ul>.';
```

Przetwarzanie w javascript
------------------------

Na przykład w bibliotece jQuery ciąg znaków json można bardzo łatwo przekształcić w obiekt:

```php
var json = '{"name": "Jan", "surname": "Barasek", "role":["admin", "moderator"]}';
var parser = $.parseJSON(json);

document.write('Nazwa:' + parser.name);
console.log(parser); // Drukuje cały obiekt na konsolę w celu ułatwienia debugowania
```

Ogólnie w języku javascript każdy json jest poprawnym obiektem javascript, z którym można pracować bezpośrednio i korzystać z jego właściwości.
