Variables locales en PHP
========================

> id: '9c3fbf4e-8612-4599-8127-d66b9ec5e980'
> slug:
> 	cs: lokalni-promenna
> 	es: variables-locales-en-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: cffee22c4dbcc2ec331b73235349c409

Las variables locales sólo son válidas dentro del cuerpo de **<a href="/prikazy-a-function">función</a>** o **método** (en programación orientada a objetos).

Si estamos trabajando en el contexto de un script regular, todo sucede como se espera:

```php
$x = 5;

echo $x; // imprime 5
```

Pero cuando definimos una función personalizada, el comportamiento cambia ligeramente:

```php
$x = 5;

function mojeFunkce(): int
{
    $x = 3;

    echo $x; // imprime 3
}

echo $x;     // imprime 5
```

La razón es que la variable $x sólo existe en el contexto de la función o método actual. Este comportamiento es intencionado.

Si queremos pasar un valor del código circundante a una función, tenemos que llamarla con los parámetros necesarios:

```php
echo mojeFunkce(5);	// imprime 6

function mojeFunkce(int $x): int
{
    return $x + 1;
}
```

Los valores se introducen en las funciones mediante parámetros. Si quieres pasar variables adicionales a la función más allá de los parámetros, puedes usar <a href="/variable-global">variables-globales</a>, pero no es una buena idea.

> El uso de variables locales supone una gran diferencia a la hora de programar una aplicación mayor. Si no distinguiéramos la validez de las variables a través de los contextos, sería imposible garantizar que una variable no fuera anulada en un lugar donde no contamos con ella (por eso las variables globales son malas).

```php
$x = 5;
$y = 3;

function soucet(int $x, int $y): int
{
    return $x + $y;
}

echo $x;             // imprime 5
echo soucet($x, $y); // imprime 8
```
