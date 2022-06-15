Addcslashes
===========

> id: '48278937-b1c5-479b-bac2-9b1ec552df4c'
> slug:
> 	cs: addcslashes
> 	es: addcslashes
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1f53fe14dd5fbf79958c645d15c4d204'

Soporta PHP4, PHP5

`addcslashes` - Cadena de barras de estilo C

Descripción
--------------------------

```php
string addcslashes (string $str, string $charlist)
```

Devuelve una cadena con barras invertidas antes de los caracteres especificados en el parámetro charlist.

Parámetros
--------------------------

**str** Cadena de texto

**Característica**

caracteres a eliminar. Si charlist contiene los caracteres `\n`, `\r`, y otros, se convierten a estilo C. Otros caracteres ASCI no alfanuméricos con longitudes inferiores a 32 y superiores a 126 cambian.

Cuando defina una secuencia de caracteres en un argumento charlist, asegúrese de saber qué caracteres pone como principio y final del rango.

```php
echo addcslashes('foo[ ]', 'A..z');

// Valores: \f\o\o[ \o]
// Elimina todas las letras minúsculas y mayúsculas
```

Valores de retorno
--------------------------

Devuelve la cadena modificada.

Ejemplo
--------------------------

```php
$escaped = addcslashes($not_escaped, "\0..\37!@\177..\377");
```

charlist `0..\37!@\177..\377`, elimina todos los caracteres con código ASCII entre 0 y 31.
