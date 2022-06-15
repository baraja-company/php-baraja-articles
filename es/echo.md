Echo - salida al código fuente
==============================

> id: d6a1a7bf-03bf-49ea-9947-d4d6753ca6e4
> slug:
> 	cs: echo
> 	es: echo-salida-al-codigo-fuente
> 
> perex:
> 	- Echo - výpis řetězce do stránky a na standardní výstup. Možnosti escapování a HTTP hlaviček.
> 	- Echo - vuelca la cadena a la página y a la salida estándar. Opciones de escape y cabeceras HTTP.
> 
> publicationDate: '2020-02-16 18:40:31'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '7d611691825ec537f58f387696d13e28'

La construcción `echo` se utiliza para volcar una variable o cadena en el código fuente.

| Soporte: Todas las versiones
|----------------|------
| Breve descripción: | Salida de una o varias cadenas
| Tipo: <a href="/comandos-y-funciones">comando, construcción</a> (no una función)

Descripción
-----

```php
echo 'Hola, mundo';
```

Dice "hola mundo".

```php
$var = 'Texto';
echo $var;
```

Imprime el valor de la variable `$var`, es decir, "Texto".

**Echo** no es una función (es un <a href="/comandos-y-funciones">comando</a>), por lo que puede o no utilizar un paréntesis. Por lo tanto, escribir `echo ('hola mundo');` también es correcto.

> **Nota adicional:** PHP trata a **Echo** como un comando (una construcción) y por lo tanto lo trata como una expresión. El paréntesis es opcional en este caso. Si damos la notación: `echo ('algo');`, la sentencia **Echo** no se convierte en una función y no se trata como tal. El paréntesis en este caso significa encerrar el valor exacto de la expresión, de forma similar a como funciona en matemáticas.

Comillas
--------

Las cadenas pueden ir entre comillas y apóstrofes.

Así que esto:

```php
echo "Hola";
```

Es lo mismo que esto:

```php
echo 'Hola';
```

Pero tenga en cuenta que cada cadena debe empezar y terminar con el mismo tipo de carácter de comillas y el carácter de comillas no debe ser utilizado en la cadena.

Por ejemplo, si quiere dar salida a un enlace HTML (o a cualquier código HTML), debe preceder las comillas con una barra. Una barra oblicua significa "exactamente este carácter", por lo que no se entiende como una expresión en el idioma.

```php
echo "<a href="index.php">texto del enlace</a>";
```

Nota técnica: Las comillas tienen un <a href="/quotation-meaning">significado especial</a> en PHP.

Parámetros
---------

- `arg` Parámetro de salida.

Valores de retorno
-----------------

No se devuelve ningún valor.

No se puede utilizar como variable.

Nota
--------

Nota: Como se trata de una **construcción del lenguaje** (construcción = comando) (no una función), no puede cargarse en una variable.

Ejemplo
-------

```php
echo "Hola, mundo";

echo "echo" puede dar salida a varias líneas de texto.
Pero cuidado con la etiqueta HTML <br>, no se imprime. Para eso está la función nl2br()".;

$a = "php";				// definición de la variable

echo "Me gusta" . $a;	// Escribe: Me gusta el php
```

**Echo** también tiene una sintaxis acortada, en la que es posible utilizar sólo el signo de igualdad después de la etiqueta php de apertura.

```php
Ahoj <?=$jmeno;?>!
```

Esto es útil si necesitamos escribir alguna información rápida en la página. Por ejemplo, el año en curso:

```php
Píše Jan Barášek © <?=date('Y');?>
```

> Esta sintaxis acortada sólo funcionará si las etiquetas de apertura acortadas de php están habilitadas, es decir, si la directiva `short_open_tag` está configurada como `on`.

Operación
-------

Todas las operaciones matemáticas comunes se pueden realizar dentro del comando **echo**.

Para un análisis detallado de las matemáticas, véase <a href="/matemáticas">un artículo aparte</a>.

```php
echo 5 + 3 * 2; // imprime 11
```
