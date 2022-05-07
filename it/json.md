Json in PHP - elaborazione, generazione e formattazione
=======================================================

> id: '07729429-7201-4b3a-b991-4a66f9251161'
> slug:
> 	cs: json
> 	it: json-in-php---elaborazione-generazione-e-formattazione
> 
> perex:
> 	- JSON je datový typ pro předávání složitých datových struktur přes textový řetězec typicky jako API pro javascript.
> 	- 'JSON è un tipo di dati per passare strutture di dati complessi su una stringa di testo, tipicamente come API per javascript.'
> 
> publicationDate: '2019-11-26 11:32:43'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: c80746ea92c3dce77e890827e28d48b8

Il formato di dati Json è stato creato per facilitare il trasferimento di dati strutturati tra client e server.

Convertire un array o un oggetto in json
--------------------------------

Per convertire un array o un oggetto in Json, c'è una funzione `json_encode` in PHP:

```php
$user = [
	'nome' => 'Jan',
	'cognome' => 'Barasek',
	'ruolo' => [
		'Admin',
		'moderatore',
	],
];

echo json_encode($user);
```

Genererà:

```txt
{"name":"Jan","surname":"Barasek","role":["admin","moderator"]}
```

La funzione `json_encode()` può anche convertire altri tipi di dati. Per esempio, possiamo inserire direttamente `integer` (intero), `string` (stringa) e così via.

Json pretty print - migliore leggibilità umana
--------------------------------------------

Nella configurazione predefinita, json è generato come una lunga stringa. Questo per il motivo di occupare meno spazio possibile.

Se hai bisogno di una migliore leggibilità, passa semplicemente la costante `JSON_PRETTY_PRINT` come secondo parametro:

```php
echo json_encode($users, JSON_PRETTY_PRINT);
```

Genererà:

```php
{
	"nome": "Jan",
	"cognome": "Barasek",
	"ruolo": [
		"Admin",
		"moderatore"
	]
}
```

Formattazione json - regole di formattazione personalizzate
------------------------------------------------

La normale formattazione di json via PHP non ha impostazioni aggiuntive e dobbiamo accontentarci solo di questo output. Per esempio, se vogliamo far rientrare le tabulazioni invece degli spazi (o modificare le regole di generazione in qualsiasi modo), dobbiamo programmarlo noi stessi.

Questa funzione può già fare la formattazione di base e dobbiamo solo modificarla:

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

Fonte: https://stackoverflow.com/questions/6054033/pretty-printing-json-with-php/9776726#9776726

Per esempio, input:

```php
{"chiave1":[1,2,3],"chiave2":"valore"}
```

Formattato come:

```php
{
	"chiave1": [
		1,
		2,
		3
	],
	"chiave2": "valore"
}
```

Che è molto più facile da leggere (a causa dell'indentazione coerente di ogni elemento).

Colorare la notazione json come HTML
------------------------------

A volte può essere utile colorare le chiavi, i dati stessi e i singoli elementi di formattazione (specialmente le parentesi di chiusura).

Per esempio, input:

```php
{"accesso": {"token": {"emesso_a": "2008-08-16T14:10:31.309353", "scade": "2008-08-17T14:10:31Z", "id": "MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"}, "serviceCatalog": [], "utente": {"nome utente": "ajay", "collegamenti ai ruoli": [], "id": "16452ca89", "ruoli": [], "nome": "ajay"}}}
```

Formattato come:

{<br>
&nbsp;&nbsp;<span style="color:#C22;">"access"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"token"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"issued_at"</span>:&nbsp;<span style="color:#35D;">"2008-08-16T14:10:31.309353"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"expires"</span>:&nbsp;<span style="color:#35D;">"2008-08-17T14:10:31Z"</span>,&nbsp;<br>
<span style="color:#C22;"><span style="color:#C22;">"id"</span>:&nbsp;<span style="color:#35D;">"MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"</span><br>
<br> <br>Si prega di non utilizzare il sistema di sicurezza.
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"serviceCatalog"</span>:&nbsp;[<br>
[*],&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"user"</span>:&nbsp;{<br>
<span style="color:#C22;">"username"</span>:&nbsp;<span style="color:#35D;">"ajay"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"roles_links"</span>:&nbsp;[<br>
[*],&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
<span style="color:#C22;"><span style="id">:&nbsp;<span style="color:#35D;">"16452ca89"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"roles"</span>:&nbsp;[<br>
[*],&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
<span style="color:#C22;">"name"</span>:&nbsp;<span style="color:#35D;">"ajay"</span><br>
}<br>
&nbsp;&nbsp;}<br>
}

Questo può essere implementato dalla seguente funzione:

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

    // Restituisce il sorgente HTML
    return '<pre>' . $buffer . '</pre>';
}

// Basta chiamare questo e restituire l'output formattato
// echo jsonColorFormater($data, ' ');
```

Caratteristica presa e riscritta radicalmente da: https://stackoverflow.com/a/20953262/6777550


Convertire da JSON a array/oggetto PHP
----------------------

Questo può essere implementato con la funzione json_decode(), che crea una struttura di dati da Json con cui dobbiamo lavorare come un oggetto:

```php
$json = '{"nome": "Jan", "cognome": "Barasek", "ruolo":["admin", "moderatore"]}';
$decode = json_decode($json);

echo $decode->name; // Restituisce 'Jan'

// echo $decode->role;
//
// Questo non è possibile perché il ruolo della proprietà
// contiene un array, dobbiamo iterare.

echo '<ul>';
foreach ($decode->role as $role) {
    echo '<li>' . $role . '</li>'; // Elenca consecutivamente i ruoli dietro i proiettili
}
echo '</ul>';
```

Elaborazione in javascript
------------------------

Per esempio, nella libreria jQuery, una stringa json può essere analizzata in un oggetto molto facilmente:

```php
var json = '{"nome": "Jan", "cognome": "Barasek", "ruolo":["admin", "moderatore"]}';
var parser = $.parseJSON(json);

document.write('Nome:' + parser.name);
console.log(parser); // Stampa l'intero oggetto sulla console per il debug
```

In javascript, in generale, qualsiasi json è un oggetto javascript valido con cui si può lavorare direttamente e accedere alle sue proprietà.
