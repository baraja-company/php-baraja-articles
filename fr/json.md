Json en PHP - traitement, génération et formatage
=================================================

> id: '07729429-7201-4b3a-b991-4a66f9251161'
> slug:
> 	cs: json
> 	fr: json-en-php---traitement-generation-et-formatage
> 
> perex: JSON je datový typ pro předávání složitých datových struktur přes textový řetězec typicky jako API pro javascript.
> publicationDate: '2019-11-26 11:32:43'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: c80746ea92c3dce77e890827e28d48b8

Le format de données Json a été créé pour faciliter le transfert de données structurées entre le client et le serveur.

Convertir un tableau ou un objet en json
--------------------------------

Pour convertir un tableau ou un objet en Json, il existe une fonction `json_encode` en PHP :

```php
$user = [
	'nom' => 'Jan',
	'nom de famille' => 'Barasek',
	'rôle' => [
		'Admin',
		'modérateur',
	],
];

echo json_encode($user);
```

Ça va générer :

```txt
{"name":"Jan","surname":"Barasek","role":["admin","moderator"]}
```

La fonction `json_encode()` peut également convertir d'autres types de données. Par exemple, nous pouvons insérer directement `integer` (entier), `string` (chaîne de caractères) et ainsi de suite.

Json pretty print - meilleure lisibilité humaine
--------------------------------------------

Dans la configuration par défaut, json est généré comme une longue chaîne. Ceci afin de prendre le moins de place possible.

Si vous souhaitez une meilleure lisibilité, passez simplement la constante `JSON_PRETTY_PRINT` comme deuxième paramètre :

```php
echo json_encode($users, JSON_PRETTY_PRINT);
```

Ça va générer :

```php
{
	"nom": "Jan",
	"nom de famille": "Barasek",
	"rôle": [
		"Admin",
		"modérateur"
	]
}
```

Formatage de json - règles de formatage personnalisées
------------------------------------------------

Le formatage normal de json via PHP n'a pas de paramètres supplémentaires et nous devons nous contenter de cette sortie. Par exemple, si nous voulons indenter les tabulations au lieu des espaces (ou modifier les règles de génération de quelque manière que ce soit), nous devons le programmer nous-mêmes.

Cette fonction peut déjà effectuer un formatage de base et nous devons simplement la modifier :

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

Source : https://stackoverflow.com/questions/6054033/pretty-printing-json-with-php/9776726#9776726

Par exemple, l'entrée :

```php
{"Clé1":[1,2,3],"clé2":"valeur"}
```

Formaté comme :

```php
{
	"Clé1": [
		1,
		2,
		3
	],
	"clé2": "valeur"
}
```

Ce qui est beaucoup plus facile à lire (en raison de l'indentation cohérente de chaque élément).

Coloration de la notation json en HTML
------------------------------

Il peut parfois être utile de mettre en couleur les clés, les données elles-mêmes et les éléments de formatage individuels (en particulier les parenthèses fermantes).

Par exemple, l'entrée :

```php
{"accès": {"jeton": {"issued_at": "2008-08-16T14:10:31.309353", "expirant à": "2008-08-17T14:10:31Z", "id": "MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"}, "serviceCatalogue": [], "utilisateur": {"nom d'utilisateur": "ajay", "rôles_liens": [], "id": "16452ca89", "rôles": [], "nom": "ajay"}}}
```

Formaté comme :

{<br>
&nbsp;&nbsp;<span style="color:#C22 ;">"access"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22 ;">"token"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"issued_at"</span>:&nbsp;<span style="color:#35D;">"2008-08-16T14:10:31.309353"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22 ;">"expire"</span>:&nbsp;<span style="color:#35D ;">"2008-08-17T14:10:31Z"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22 ;"><span style="color:#C22 ;">"id"</span>:&nbsp;<span style="color:#35D ;">"MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"</span><br>.
&nbsp;&nbsp;&nbsp;&nbsp;},<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22 ;">"serviceCatalog"</span>:&nbsp ;[<br>
[&nbsp;&nbsp;&nbsp;&nbsp ;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22 ;">"utilisateur"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22 ;">"username"</span>:&nbsp;<span style="color:#35D ;">"ajay"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22 ;">"roles_links"</span>:&nbsp ;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp ;],&nbsp;<br>
<nbsp;&nbsp;&nbsp;&nbsp;<nbsp;<span style="color:#C22 ;"><span style="id">:&nbsp;<span style="color:#35D ;">"16452ca89"</span>,&nbsp;<br>.
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22 ;">"rôles"</span>:&nbsp ;[<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp ;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22 ;">"name"</span>:&nbsp;<span style="color:#35D ;">"ajay"</span><br>.
&nbsp;&nbsp;&nbsp;&nbsp;}<br>
&nbsp;&nbsp;}<br>
}

Ceci peut être mis en œuvre par la fonction suivante :

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
            $buffer .= '<span style="color :'.((@$json[$c - 1] == ':' || @$json[$c - 2] == ':') ? '#35D' : '#C22').';">';
        }
        $buffer .= $json[$c];
        if ($json[$c] == '"' && $ss) $buffer .= '</span>';
        if ($json[$c] == '"') $ss = !$ss;
        if ($json[$c] == '{' || $json[$c] == '[') {
            $crl++;
            $buffer .= "\n". str_repeat($indentation, $crl);
        }
    }

    // Retourne la source HTML
    return '<pre>' . $buffer . '</pre>';
}

// Appelez simplement cette fonction et renvoyez la sortie formatée
// echo jsonColorFormater($data, ' ') ;
```

Fonctionnalité reprise et radicalement réécrite de : https://stackoverflow.com/a/20953262/6777550


Conversion de JSON en tableau/objet PHP
----------------------

Cela peut être mis en œuvre avec la fonction json_decode(), qui crée une structure de données à partir de Json avec laquelle nous devons travailler comme un objet :

```php
$json = '{"nom" : "Jan", "nom de famille" : "Barasek", "rôle" :["admin", "modérateur"]}';
$decode = json_decode($json);

echo $decode->name; // Retourne 'Jan'.

// echo $decode->role ;
//
// Ce n'est pas possible car le rôle de propriété
// contient un tableau, nous devons l'itérer.

echo '<ul>';
foreach ($decode->role as $role) {
    echo '<li>' . $role . '</li>'; // Liste consécutivement les rôles derrière les puces.
}
echo '</ul>';
```

Traitement en javascript
------------------------

Par exemple, dans la bibliothèque jQuery, une chaîne json peut être analysée en objet très facilement :

```php
var json = '{"nom" : "Jan", "nom de famille" : "Barasek", "rôle" :["admin", "modérateur"]}';
var parser = $.parseJSON(json);

document.write('Nom :' + parser.name);
console.log(parser); // Imprime l'objet entier sur la console pour le débogage.
```

En javascript, en général, tout json est un objet javascript valide avec lequel on peut travailler directement et accéder à ses propriétés.
