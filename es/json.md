Json en PHP - procesamiento, generación y formato
=================================================

> id: '07729429-7201-4b3a-b991-4a66f9251161'
> slug:
> 	cs: json
> 	es: json-en-php-procesamiento-generacion-y-formato
> 
> perex:
> 	- JSON je datový typ pro předávání složitých datových struktur přes textový řetězec typicky jako API pro javascript.
> 	- 'JSON es un tipo de datos para pasar estructuras de datos complejas sobre una cadena de texto, normalmente como una API para javascript.'
> 
> publicationDate: '2019-11-26 11:32:43'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: c80746ea92c3dce77e890827e28d48b8

El formato de datos Json se creó para facilitar la transferencia de datos estructurados entre el cliente y el servidor.

Convertir un array o un objeto en json
--------------------------------

Para convertir un array u objeto en Json, existe la función `json_encode` en PHP:

```php
$user = [
	'nombre' => 'Jan',
	'apellido' => 'Barasek',
	'papel' => [
		'Administración',
		'moderador',
	],
];

echo json_encode($user);
```

Generará:

```txt
{"name":"Jan","surname":"Barasek","role":["admin","moderator"]}
```

La función `json_encode()` también puede convertir otros tipos de datos. Por ejemplo, podemos insertar directamente `integer` (entero), `string` (cadena), etc.

Json pretty print - mejor legibilidad humana
--------------------------------------------

En la configuración por defecto, el json se genera como una cadena larga. Esto es por la razón de ocupar el menor espacio posible.

Si necesita una mejor legibilidad humana, simplemente pase la constante `JSON_PRETTY_PRINT` como segundo parámetro:

```php
echo json_encode($users, JSON_PRETTY_PRINT);
```

Generará:

```php
{
	"nombre": "Jan",
	"apellido": "Barasek",
	"papel": [
		"Administración",
		"moderador"
	]
}
```

Formato json - reglas de formato personalizadas
------------------------------------------------

El formato normal de json a través de PHP no tiene ninguna configuración adicional y tenemos que conformarnos con esta salida solamente. Por ejemplo, si queremos sangrar tabulaciones en lugar de espacios (o modificar las reglas de generación de cualquier manera), tenemos que programarlo nosotros mismos.

Esta función ya puede hacer un formato básico y sólo tenemos que modificarla:

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

Fuente : https://stackoverflow.com/questions/6054033/pretty-printing-json-with-php/9776726#9776726

Por ejemplo, la entrada:

```php
{"clave1":[1,2,3],"clave2":"valor"}
```

Formateado como:

```php
{
	"clave1": [
		1,
		2,
		3
	],
	"clave2": "valor"
}
```

Que es mucho más fácil de leer (debido a la sangría consistente de cada elemento).

Colorear la notación json como HTML
------------------------------

A veces puede ser útil resaltar en color las claves, los datos en sí mismos y los elementos individuales de formato (especialmente los corchetes de cierre).

Por ejemplo, la entrada:

```php
{"acceder a": {"ficha": {"emitido_a": "2008-08-16T14:10:31.309353", "expira": "2008-08-17T14:10:31Z", "id": "MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXQiOiAi"}, "servicioCatálogo": [], "usuario": {"nombre de usuario": "ajay", "roles_enlaces": [], "id": "16452ca89", "roles": [], "nombre": "ajay"}}}
```

Formateado como:

{<br>
<span style="color:#C22;">"acceso"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"token"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"issued_at"</span>:&nbsp;<span style="color:#35D;">"2008-08-16T14:10:31.309353"</span>,&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"expires"</span>:&nbsp;<span style="color:#35D;">"2008-08-17T14:10:31Z"</span>,&nbsp;<br>
<span style="color:#C22;"><span style="color:#C22;">"id"</span>:&nbsp;<span style="color:#35D;">"MIICQgYJKoZIhvcIegeyJpc3N1ZWRfYXiOiAi"</span><br>
<br>
<span style="color:#C22;">"serviceCatalog"</span>:&nbsp;[<br>
&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"usuario"</span>:&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"nombre de usuario"</span>:&nbsp;<span style="color:#35D;">"ajay"</span>,&nbsp;<br>
<span style="color:#C22;">"roles_links"</span>:&nbsp;[<br>
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;"><span style="id">:&nbsp;<span style="color:#35D;">"16452ca89"</span>,&nbsp;<br>
<span style="color:#C22;">"roles"</span>:&nbsp;[<br>
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#C22;">"nombre"</span>:&nbsp;<span style="color:#35D;">"ajay"</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;}<br>
&nbsp;&nbsp;}<br>
}

Esto se puede implementar mediante la siguiente función:

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

    // Devuelve la fuente HTML
    return '<pre>' . $buffer . '</pre>';
}

// Sólo llama a esto y devuelve la salida formateada
// echo jsonColorFormater($data, ' ');
```

Característica tomada y reescrita radicalmente de: https://stackoverflow.com/a/20953262/6777550


Convertir de JSON a array/objeto PHP
----------------------

Esto se puede implementar con la función json_decode(), que hace una estructura de datos de Json con la que tenemos que trabajar como un objeto:

```php
$json = '{"nombre": "Jan", "apellido": "Barasek", "rol":["administrador", "moderador"]}';
$decode = json_decode($json);

echo $decode->name; // Devuelve 'Jan'

// echo $decode->role;
//
// Esto no es posible porque el rol de la propiedad
// contiene un array, necesitamos iterar.

echo '<ul>';
foreach ($decode->role as $role) {
    echo '<li>' . $role . '</li> <li>Por lo tanto, no es necesario que el usuario se sienta cómodo.'; // Enumera consecutivamente los roles detrás de las viñetas
}
echo '';
```

Procesamiento en javascript
------------------------

Por ejemplo, en la biblioteca jQuery, una cadena json puede ser analizada en un objeto muy fácilmente:

```php
var json = '{"nombre": "Jan", "apellido": "Barasek", "rol":["administrador", "moderador"]}';
var parser = $.parseJSON(json);

document.write('Nombre:' + parser.name);
console.log(parser); // Imprime el objeto completo en la consola para su depuración
```

En javascript, en general, cualquier json es un objeto javascript válido con el que se puede trabajar directamente y acceder a sus propiedades.
