Apóstrofes y comillas
=====================

> id: '526ad995-3412-446e-bb56-9627dff8e29e'
> slug:
> 	cs: apostrofy-a-uvozovky
> 	es: apostrofes-y-comillas
> 
> perex:
> 	- Použití uvozovek a apostrofů pro ohraničení řetězců v PHP. Escapování řetězců a řešení kontextů.
> 	- Uso de comillas y apóstrofes para delimitar cadenas en PHP. Escape de cadenas y resolución de contexto.
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c5b0bf8b74238134be5348f886591e2a

Puede utilizar comillas o apóstrofes para delimitar las cadenas. Personalmente, prefiero sólo los **apóstrofes**, a menos que se trate de un carácter especial de salto de línea o de un tabulador.

Hay una serie de razones para ello, repasémoslas de forma constructiva.

> La principal razón para no utilizar las comillas es la seguridad y el manejo inadecuado de los tipos de datos.

Uso de etiquetas HTML dentro de una cadena
--------------------------------

Si necesitamos devolver código HTML en una cadena, y envolvemos la cadena entre comillas, tenemos que escapar de forma bastante incómoda:

```php
return "<a href=\"{$link}\">{$text}</a>";
```

O hacer lo mismo de forma más legible, manteniendo las comillas sin escapar:

```php
return '<a href="' . $link . '">' . $text . '</a>';
```

Mayor distancia visual de la cadena respecto a la variable
---------------------------------------------

No hay nada peor que meter variables dentro de una cadena directamente encima de otra, donde no se puede distinguir rápidamente lo que es una variable y lo que es una cadena:

```php
$url = "{$baseImageUrl}/{$dirName}/{$basename}.{$ext}";
```

Esta notación no tiene ningún efecto negativo en la funcionalidad, sólo que las variables individuales y los caracteres fijos de la cadena se apilan demasiado juntos, lo que degrada la legibilidad.

Intentémoslo de nuevo y hagámoslo más legible:

```php
$url = $baseImageUrl . '/' . $dirName . '/' . $basename . '.' . $ext;
```

La principal ventaja que veo es la limpieza en torno a las variables, donde no estorban los caracteres innecesarios en el nombre.

Además, será más fácil de envolver y no habrá caracteres de envoltura en la cadena ;)

```php
$url = $baseImageUrl
       . '/' . $dirName
       . '/' . $basename . '.' . $ext;
```

No es posible llamar a una función entre comillas
---------------------------------------

Pero una variable puede, por lo que "algo" se añade fuera de la cadena (especialmente las funciones), pero las variables se escriben dentro. Y a veces el programador incluso añade la variable después de la cadena. En resumen, la confusión sobre la confusión.

¿Por qué no podemos hacer las cosas de manera uniforme?

Asignación inadecuada de otro tipo de datos a una cadena
---------------------------------------------------

Considere la siguiente llamada de función:

```php
echo getFullName("{$user->name}");

function getFullName(string $name): string
{
	// ... implementación ...
}
```

Es posible insertar variables entre comillas, lo que hará que se reescriban como una cadena. Sin embargo, si la variable se encuentra en la propia cadena y es de un tipo de datos diferente al de la cadena, la información sobre el tipo de datos original puede estar corrupta. Por ejemplo, si la construcción `$user->name` devolviera `false` o `null`, no podríamos decir que se trata de un error.

Una aplicación debería reconocer siempre una condición de error y fallar adecuadamente, en lugar de ignorarla. Un informe de errores adecuado conduce a una mejor depuración en el futuro.

Ocasionalmente, puede encontrar anulaciones, especialmente porque una función requiere un tipo de datos particular:

```php
trim("{$returnText}");
```

En ese caso, me inclino más por escribirlo:

```php
trim ((string) $returnText);
```

que no es tan "mágico" y es obvio, incluso para los usuarios menos expertos, lo que ocurrió con la variable.

Eliminación del valor `null` de la base de datos
----------------------------------

Supongamos que estamos recuperando el nombre de un hotel de una base de datos:

```php
return "{$row->hotel->name}";
```

¿Pero qué pasa si el nombre no existe y es "nulo"? Al utilizar las comillas, hemos creado un error potencialmente difícil de detectar, porque se reescribirá en una cadena vacía. Sin embargo, nos gustaría devolver un verdadero "nulo". Es diferente si el registro no existe, o existe pero está vacío.

Así que la forma correcta de hacerlo sería no especificar nada:

```php
return $row->hotel->name;
```

¿La función requiere la devolución de un tipo de datos específico y es estricta al respecto? Si no podemos satisfacer la especificación, debemos lanzar una excepción.

```php
function getHotelName(int $hotelId): string
{
   // aplicación

   if ($row->hotel->name === null) {
      throw new \Exception('El nombre del hotel no existe.');
   }

   return $row->hotel->name;
}
```

Incongruencias en la codificación de caracteres y tipos de datos
--------------------------------------------

No es raro encontrarse con construcciones del tipo:

```php
echo "{$returnCode}" . self::CRLF;
```

Que se escribe mejor como:

```php
echo $returnCode . "\n";
```

El uso de comillas alrededor de la variable es innecesario en este caso y sólo hace que sea más difícil de leer porque son caracteres extra innecesarios. Al mismo tiempo, el ajuste de línea del tipo `CRLF` no es moderno y es mejor utilizar sólo `\n`, que es nativo de la codificación `UTF-8`.

De forma adecuada, podríamos seguir validando el tipo de datos para el código. Por ejemplo, que sea un número y que posiblemente devuelva un reemplazo. Esto se puede hacer con el operador <a href="/ternary-operator">operador alternativo</a>:

```php
echo (is_int($returnCode) ? $returnCode : '?') . "\n";
```

> Nota: El uso de la función `is_int()` indica un mal diseño de la aplicación, ya que nunca debería ocurrir que no conozcamos el tipo de dato de una variable.

Envolver la cadena de salida con apóstrofes
---------------------------------------

A menudo es necesario encerrar la cadena de salida de una variable entre comillas o apóstrofes en el texto de la excepción. Sin embargo, esto crea innecesariamente un "pago" para ambos tipos de delimitadores de cadena, y si la cadena comienza y termina con el mismo carácter, no es posible determinar rápidamente y sin ambigüedades dónde comenzó y terminó la cadena en el caso de cadenas múltiples si aparece un apóstrofe dentro:

```php
throw new \Exception(__METHOD__ . ": Unsupported data type '{$fileType}'");
```

Yo personalmente lo resuelvo encerrando en un tipo de carácter diferente (a mí me funcionan bien los corchetes), para que se vea con elegancia el principio y el final de la cadena:

```php
throw new \Exception(__METHOD__ . ': Tipo de datos no admitidos [' . $fileType . ']');
```

O:

```php
throw new \Exception("Status '{$status}' is invalid");
```

Se puede cambiar por:

```php
throw new \Exception('Estado [' . $status . '] no es válido');
```

Análisis por líneas
--------------------

Las comillas son útiles, por ejemplo, para analizar una nueva línea:

```php
foreach(explode("\n", $text) as $line) {
	// aplicación
}
```

Fueron creados especialmente para este fin.

Sin embargo, si necesitas procesar un documento más complejo (como el código fuente de un lenguaje de programación o una página HTML), te recomiendo utilizar **Tokenizer** junto con <a href="/regex">expresiones regulares</a>.

No combine dos métodos de encierro
-----------------------------------

Si por alguna razón vas a utilizar las comillas de todas formas, al menos mantenlas de forma coherente en todo momento.

Este es un caso disuasorio:

```php
throw new \Exception("La imagen en la URL no existe. Tamaño de la respuesta:" . strlen($result) . ')');
```

Donde el principio de la cadena está delimitado por comillas y el final por apóstrofes.

Este defecto se produce especialmente cuando se utilizan herramientas de formateo automático (como **Code Sniffer**) que intentan unificar las convenciones, pero no siempre lo consiguen.
