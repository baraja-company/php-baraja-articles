Imprimir
========

> id: '23d672c5-b347-43b8-bd08-8ccb7ecca33f'
> slug:
> 	cs: print
> 	es: imprimir
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7f7c3c32d9f3c1efd794ca38b7db384

`print` - Salida de cadena

Descripción
--------------------------

```php
print '¡Hola, mundo!';
```

`print()` no es en realidad una función real (es una construcción del lenguaje), por lo que no es necesario utilizar paréntesis.

Parámetros
--------------------------

- La gente de la ciudad de Nueva York no puede hacer nada por el bien de los demás.

Parámetro de salida

Valores de retorno
--------------------------

Siempre devuelve el número 1.

Nota
--------------------------

Nota: Como se trata de una **construcción del lenguaje** (no de una función), no puede cargarse en una variable.

Ejemplo
--------------------------

```php
print "Hola, mundo";

print "print" puede dar salida a varias líneas de texto.Pero cuidado con la etiqueta HTML
porque no se imprime. Eso es lo que dice el <a href="/nl2br">Nl2br</a>.";

// Ejemplo de conexión con la variable
$a = 'php';

print 'Me gusta' . $a; // Me gusta php
```

**print** es exactamente la misma función que **echo**. Si buscas más información, consulta el artículo <a href="/echo">echo</a>.
