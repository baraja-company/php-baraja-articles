Json in PHP - Verarbeitung, Erzeugung und Formatierung
======================================================

> id: '07729429-7201-4b3a-b991-4a66f9251161'
> slug:
> 	cs: json
> 	de: json-in-php---verarbeitung-erzeugung-und-formatierung
> 
> perex:
> 	- JSON je datový typ pro předávání složitých datových struktur přes textový řetězec typicky jako API pro javascript.
> 	- 'JSON ist ein Datentyp zur Übermittlung komplexer Datenstrukturen über eine Textzeichenkette, typischerweise als API für Javascript.'
> 
> publicationDate: '2019-11-26 11:32:43'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: c80746ea92c3dce77e890827e28d48b8

Das Json-Datenformat wurde entwickelt, um die Übertragung von strukturierten Daten zwischen Client und Server zu erleichtern.

Konvertierung eines Arrays oder Objekts in json
--------------------------------

Um ein Array oder Objekt in Json zu konvertieren, gibt es in PHP die Funktion `json_encode`:

```php
$user = [
	'Name' => 'Jan',
	'Nachname' => 'Barasek',
	'Rolle' => [
		'Verwaltung',
		'Moderatorin',
	],
];

echo json_encode($user);
```

Das wird etwas bewirken:

```txt
{"name":"Jan","surname":"Barasek","role":["admin","moderator"]}
```

Die Funktion `json_encode()` kann auch andere Datentypen konvertieren. Zum Beispiel können wir direkt `integer` (Ganzzahl), `string` (Zeichenkette) und so weiter einfügen.

Json pretty print - bessere Lesbarkeit für Menschen
--------------------------------------------

In der Standardkonfiguration wird json als eine lange Zeichenkette erzeugt. Der Grund dafür ist, dass sie so wenig Platz wie möglich einnehmen sollen.

Wenn Sie eine bessere Lesbarkeit benötigen, übergeben Sie einfach die Konstante `JSON_PRETTY_PRINT` als zweiten Parameter:

```php
echo json_encode($users, JSON_PRETTY_PRINT);
```

Das wird etwas bewirken:

```php
{
	"Name": "Jan",
	"Nachname": "Barasek",
	"Rolle": [
		"Verwaltung",
		"Moderatorin"
	]
}
```

Formatierung von json - benutzerdefinierte Formatierungsregeln
------------------------------------------------

Die normale Formatierung von json über PHP hat keine zusätzlichen Einstellungen und wir müssen uns mit dieser Ausgabe begnügen. Wenn wir beispielsweise Tabulatoren anstelle von Leerzeichen einrücken wollen (oder die Generierungsregeln in irgendeiner Weise ändern wollen), müssen wir dies selbst programmieren.

Diese Funktion kann bereits grundlegende Formatierungen vornehmen, wir müssen sie nur noch anpassen:

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

Quelle: https://stackoverflow.com/questions/6054033/pretty-printing-json-with-php/9776726#9776726

Zum Beispiel: Eingabe:

```php
{"Taste1":[1,2,3],"Taste2":"Wert"}
```

Formatiert als:

```php
{
	"Taste1": [
		1,
		2,
		3
	],
	"Taste2": "Wert"
}
```

Das ist viel einfacher zu lesen (wegen der einheitlichen Einrückung der einzelnen Elemente).

Json-Notation als HTML einfärben
------------------------------

Manchmal kann es sinnvoll sein, Tasten, die Daten selbst und einzelne Formatierungselemente (insbesondere schließende Klammern) farblich hervorzuheben.

Zum Beispiel: Eingabe:

```php
{"Zugang": {"Token": {"ausgestellt_am": "2008-08-16T14:10:31.309353", "Läuft ab": "2008-08-17T14:10:31Z", "id": "MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"}, "serviceKatalog": [], "Benutzer": {"Nutzername": "ajay", "rollen_links": [], "id": "16452ca89", "Rollen": [], "Name": "ajay"}}}
```

Formatiert als:

{<br>
&nbsp;&nbsp;<span style="color:#C22;">"access"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"token"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"issued_at"</span>:&nbsp;<span style="color:#35D;">"2008-08-16T14:10:31.309353"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"läuft ab"</span>:&nbsp;<span style="color:#35D;">"2008-08-17T14:10:31Z"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;"><span style="color:#C22;">"id"</span>:&nbsp;<span style="color:#35D;">"MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;},<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"serviceCatalog"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"user"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"username"</span>:&nbsp;<span style="color:#35D;">"ajay"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"roles_links"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;"><span style="id">:&nbsp;<span style="color:#35D;">"16452ca89"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"Rollen"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"name"</span>:&nbsp;<span style="color:#35D;">"ajay"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;}<br>
&nbsp;&nbsp;}<br>
}

Dies kann durch die folgende Funktion realisiert werden:

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
        if ($json[$c] == '"' && $ss) $buffer .= '</span>';
        if ($json[$c] == '"') $ss = !$ss;
        if ($json[$c] == '{' || $json[$c] == '[') {
            $crl++;
            $buffer .= "\n". str_repeat($indentation, $crl);
        }
    }

    // Gibt HTML-Quelle zurück
    return '<pre>' . $buffer . '</pre>';
}

// Rufen Sie dies einfach auf und geben Sie eine formatierte Ausgabe zurück
// echo jsonColorFormater($data, ' ');
```

Feature übernommen und radikal umgeschrieben von: https://stackoverflow.com/a/20953262/6777550


Konvertierung von JSON in PHP-Array/Objekt
----------------------

Dies kann mit der Funktion json_decode() realisiert werden, die aus Json eine Datenstruktur macht, mit der wir als Objekt arbeiten müssen:

```php
$json = '{"Name": "Jan", "Nachname": "Barasek", "Rolle":["admin", "moderator"]}';
$decode = json_decode($json);

echo $decode->name; // Gibt 'Jan' zurück

// echo $decode->role;
//
// Dies ist nicht möglich, da die Eigenschaftsrolle
// ein Array enthält, müssen wir iterieren.

echo '<ul>';
foreach ($decode->role as $role) {
    echo '<li>' . $role . '</li>'; // Führt die Rollen hinter den Aufzählungszeichen fortlaufend auf
}
echo '</ul>';
```

Verarbeitung in javascript
------------------------

In der jQuery-Bibliothek kann beispielsweise eine json-Zeichenkette sehr einfach in ein Objekt geparst werden:

```php
var json = '{"Name": "Jan", "Nachname": "Barasek", "Rolle":["admin", "moderator"]}';
var parser = $.parseJSON(json);

document.write('Name:' + parser.name);
console.log(parser); // Druckt das gesamte Objekt zur Fehlersuche auf der Konsole aus
```

In Javascript, im Allgemeinen, ist jede json ein gültiges Javascript-Objekt, das mit direkt gearbeitet werden kann und seine Eigenschaften zugegriffen.
