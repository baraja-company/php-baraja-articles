Json i PHP - behandling, generering og formatering
==================================================

> id: '07729429-7201-4b3a-b991-4a66f9251161'
> slug:
> 	cs: json
> 	da: json-i-php-behandling-generering-og-formatering
> 
> perex:
> 	- JSON je datový typ pro předávání složitých datových struktur přes textový řetězec typicky jako API pro javascript.
> 	- 'JSON er en datatype til overførsel af komplekse datastrukturer over en tekststreng, typisk som en API for javascript.'
> 
> publicationDate: '2019-11-26 11:32:43'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: c80746ea92c3dce77e890827e28d48b8

Json-dataformatet blev skabt for at lette overførslen af strukturerede data mellem klient og server.

Konvertering af et array eller objekt til json
--------------------------------

For at konvertere et array eller objekt til Json findes der en funktion `json_encode` i PHP:

```php
$user = [
	'navn' => 'Jan',
	'efternavn' => 'Barasek',
	'rolle' => [
		'Admin',
		'moderator',
	],
];

echo json_encode($user);
```

Det vil generere:

```txt
{"name":"Jan","surname":"Barasek","role":["admin","moderator"]}
```

Funktionen `json_encode()` kan også konvertere andre datatyper. Vi kan f.eks. direkte indsætte `integer` (heltal), `string` (streng) osv.

Json pretty print - bedre læsbarhed for mennesker
--------------------------------------------

I standardkonfigurationen genereres json som én lang streng. Dette er for at optage så lidt plads som muligt.

Hvis du har brug for bedre læsbarhed, skal du blot sende konstanten `JSON_PRETTY_PRINT` som den anden parameter:

```php
echo json_encode($users, JSON_PRETTY_PRINT);
```

Det vil generere:

```php
{
	"navn": "Jan",
	"efternavn": "Barasek",
	"rolle": [
		"Admin",
		"moderator"
	]
}
```

Formatering af json - brugerdefinerede formateringsregler
------------------------------------------------

Den normale formatering af json via PHP har ingen yderligere indstillinger, og vi er nødt til at nøjes med dette output. Hvis vi f.eks. ønsker at indrykke tabulatorer i stedet for mellemrum (eller ændre genereringsreglerne på nogen måde), skal vi selv programmere det.

Denne funktion kan allerede lave grundlæggende formatering, og vi skal blot ændre den:

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

Kilde : https://stackoverflow.com/questions/6054033/pretty-printing-json-with-php/9776726#9776726

For eksempel, input:

```php
{"nøgle1":[1,2,3],"key2":"værdi"}
```

Formateret som:

```php
{
	"nøgle1": [
		1,
		2,
		3
	],
	"key2": "værdi"
}
```

Hvilket er meget lettere at læse (på grund af den konsekvente indrykning af hvert element).

Farvelægning af json-notation som HTML
------------------------------

Nogle gange kan det være nyttigt at fremhæve taster, selve dataene og individuelle formateringselementer (især lukkende parenteser) med farve.

For eksempel, input:

```php
{"adgang": {"symbolsk": {"issued_at": "2008-08-16T14:10:31.309353", "udløber": "2008-08-17T14:10:31Z", "id": "MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiOiAi"}, "serviceCatalog": [], "bruger": {"brugernavn": "ajay", "roller_links": [], "id": "16452ca89", "roller": [], "navn": "ajay"}}}
```

Formateret som:

{<br>
&nbsp;&nbsp;<span style="color:#C22;">"access"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"token"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"issued_at"</span>:&nbsp;<span style="color:#35D;">"2008-08-16T14:10:31.309353"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"udløber"</span>:&nbsp;<span style="color:#35D;">"2008-08-17T14:10:31Z"</span>,&nbsp;<br>
<span style="color:#C22;"><span style="color:#C22;">"id"</span>:&nbsp;<span style="color:#35D;">"MIICQgYJKoZoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"serviceCatalog"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"user"</span>:&nbsp;{<br>
<span style="color:#C22;">"brugernavn"</span>:&nbsp;<span style="color:#35D;">"ajay"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"roles_links"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
<span style="color:#C22;"><span style="id">:&nbsp;<span style="color:#35D;">"16452ca89"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"roller"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
<span style="color:#C22;">"name"</span>:&nbsp;<span style="color:#35D;">"ajay"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>
&nbsp;&nbsp;}<br>
}

Dette kan gennemføres ved hjælp af følgende funktion:

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

    // Returnerer HTML-kilde
    return '<pre>' . $buffer . '</pre>';
}

// Kald bare dette og returnerer formateret output
// echo jsonColorFormater($data, ' ' ');
```

Funktion taget og radikalt omskrevet fra: https://stackoverflow.com/a/20953262/6777550


Konverter fra JSON til PHP array/objekt
----------------------

Dette kan implementeres med funktionen json_decode(), som laver en datastruktur ud af Json, som vi skal arbejde med som et objekt:

```php
$json = '{"name": "Jan", "efternavn": "Barasek", "role":["admin", "moderator"]}';
$decode = json_decode($json);

echo $decode->name; // Returnerer "Jan

// echo $decode->role;
//
// Dette er ikke muligt, fordi egenskabsrollen
// indeholder et array, vi skal iterere.

echo '<ul>';
foreach ($decode->role as $role) {
    echo '<li>' . $role . '</li>'; // Lister fortløbende de roller, der ligger bag kuglerne
}
echo '</ul>';
```

Behandling i javascript
------------------------

I jQuery-biblioteket kan en json-streng f.eks. meget let analyseres til et objekt:

```php
var json = '{"name": "Jan", "efternavn": "Barasek", "role":["admin", "moderator"]}';
var parser = $.parseJSON(json);

document.write('Navn:' + parser.name);
console.log(parser); // Udskriver hele objektet til konsollen til fejlfinding
```

I javascript er json generelt et gyldigt javascript-objekt, som der kan arbejdes direkte med, og som man kan få adgang til dets egenskaber.
