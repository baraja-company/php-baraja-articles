Json i PHP - bearbetning, generering och formatering
====================================================

> id: '07729429-7201-4b3a-b991-4a66f9251161'
> slug:
> 	cs: json
> 	sv: json-i-php---bearbetning-generering-och-formatering
> 
> perex:
> 	- JSON je datový typ pro předávání složitých datových struktur přes textový řetězec typicky jako API pro javascript.
> 	- 'JSON är en datatyp för överföring av komplexa datastrukturer över en textsträng, vanligtvis som ett API för javascript.'
> 
> publicationDate: '2019-11-26 11:32:43'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: c80746ea92c3dce77e890827e28d48b8

Json-dataformatet skapades för att underlätta överföringen av strukturerade data mellan klient och server.

Konvertera en matris eller ett objekt till json
--------------------------------

För att konvertera en array eller ett objekt till Json finns funktionen `json_encode` i PHP:

```php
$user = [
	'namn' => 'Jan',
	'efternamn' => 'Barasek',
	'roll' => [
		'Admin',
		'moderator',
	],
];

echo json_encode($user);
```

Det kommer att generera:

```txt
{"name":"Jan","surname":"Barasek","role":["admin","moderator"]}
```

Funktionen `json_encode()` kan också konvertera andra datatyper. Vi kan till exempel direkt infoga `integer` (heltal), `string` (sträng) och så vidare.

Json pretty print - bättre läsbarhet för människor
--------------------------------------------

I standardkonfigurationen genereras json som en lång sträng. Detta för att ta upp så lite utrymme som möjligt.

Om du vill ha bättre läsbarhet kan du bara skicka konstanten `JSON_PRETTY_PRINT` som den andra parametern:

```php
echo json_encode($users, JSON_PRETTY_PRINT);
```

Det kommer att generera:

```php
{
	"namn": "Jan",
	"efternamn": "Barasek",
	"roll": [
		"Admin",
		"moderator"
	]
}
```

Formatering av json - anpassade formateringsregler
------------------------------------------------

Den normala formateringen av json via PHP har inga ytterligare inställningar och vi måste nöja oss med denna utskrift. Om vi till exempel vill indela tabulaturer i stället för mellanslag (eller ändra genereringsreglerna på något sätt) måste vi programmera det själva.

Den här funktionen kan redan göra grundläggande formatering och vi behöver bara ändra den:

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

Källa : https://stackoverflow.com/questions/6054033/pretty-printing-json-with-php/9776726#9776726

Till exempel:

```php
{"nyckel1":[1,2,3],"key2":"värde"}
```

Formaterad som:

```php
{
	"nyckel1": [
		1,
		2,
		3
	],
	"key2": "värde"
}
```

Det är mycket lättare att läsa (på grund av den konsekventa indragningen av varje element).

Färgläggning av json-notation som HTML
------------------------------

Ibland kan det vara användbart att färgmarkera tangenter, data och enskilda formateringselement (särskilt avslutande parenteser).

Till exempel:

```php
{"tillgång till": {"symbol": {"issued_at": "2008-08-16T14:10:31.309353", "upphör att gälla": "2008-08-17T14:10:31Z", "id": "MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"}, "serviceCatalog": [], "användare": {"användarnamn": "ajay", "roller_länkar": [], "id": "16452ca89", "roller": [], "namn": "ajay"}}}
```

Formaterad som:

{<br>
&nbsp;&nbsp;<span style="color:#C22;">"access"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"token"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"issued_at"</span>:&nbsp;<span style="color:#35D;">"2008-08-16T14:10:31.309353"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"expires"</span>:&nbsp;<span style="color:#35D;">"2008-08-17T14:10:31Z"</span>,&nbsp;<br>
<span style="color:#C22;"><span style="color:#C22;">"id"</span>:&nbsp;<span style="color:#35D;">"MIICQgYJJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"serviceCatalog"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"user"</span>:&nbsp;{<br>
<span style="color:#C22;">"användarnamn"</span>:&nbsp;<span style="color:#35D;">"ajay"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"roles_links"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
<span style="color:#C22;"><span style="id">:&nbsp;<span style="color:#35D;">"16452ca89"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"roller"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
<span style="color:#C22;">"name"</span>:&nbsp;<span style="color:#35D;">"ajay"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>
&nbsp;&nbsp;}<br>
}

Detta kan genomföras med hjälp av följande funktion:

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

    // Returnerar HTML-källan
    return '<pre>' . $buffer . '</pre>';
}

// Kalla bara detta och återge formaterad utdata.
// echo jsonColorFormater($data, ' ');
```

Funktion hämtad och radikalt omskriven från: https://stackoverflow.com/a/20953262/6777550


Konvertera från JSON till PHP array/objekt
----------------------

Detta kan genomföras med funktionen json_decode(), som skapar en datastruktur av Json som vi måste arbeta med som ett objekt:

```php
$json = '{"namn": "Jan", "efternamn": "Barasek", "roll":["admin", "moderator"]}';
$decode = json_decode($json);

echo $decode->name; // Återger "Jan

// echo $decode->role;
//
// Detta är inte möjligt eftersom egenskapsrollen
// innehåller en matris, vi måste iterera.

echo '<ul>';
foreach ($decode->role as $role) {
    echo '<li>' . $role . '</li>'; // Listan över de roller som ligger bakom punkterna är löpande.
}
echo '</ul>';
```

Bearbetning i javascript
------------------------

I jQuery-biblioteket kan till exempel en json-sträng mycket enkelt analyseras till ett objekt:

```php
var json = '{"namn": "Jan", "efternamn": "Barasek", "roll":["admin", "moderator"]}';
var parser = $.parseJSON(json);

document.write('Namn:' + parser.name);
console.log(parser); // Skriver ut hela objektet på konsolen för felsökning.
```

I javascript är json generellt sett ett giltigt javascriptobjekt som kan bearbetas direkt och vars egenskaper kan nås.
