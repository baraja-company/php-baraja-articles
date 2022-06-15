Significado especial de las comillas
====================================

> id: '2649942d-94d6-48c3-9c78-5a303165bf72'
> slug:
> 	cs: uvozovky-vyznam
> 	es: significado-especial-de-las-comillas
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c1b63b98a3e9d9c642247c363747616a

En PHP, a menudo se pueden sustituir las comillas por apóstrofes y conseguir el mismo efecto. A veces incluso utilizamos una combinación de comillas y apóstrofes a propósito para conseguir un resultado diferente sin tener que utilizar el escape.

**TLDR: El uso de las comillas puede ser peligroso en algunos contextos, así que usa apóstrofes en todas partes. Las comillas sólo son adecuadas para casos especiales.**

| Carácter | Nombre |
|------|-----------
| `"` | Comillas |
| `'` | Apóstrofe |

Ejemplo uno - mismo resultado
-----------------------------

```php
echo "¡Hola, mundo!"; // esto es lo mismo
echo '¡Hola, mundo!'; // así
```

En este caso, el uso de comillas o apóstrofes no importa. Así que, a primera vista, puede parecer que no hay diferencia entre las comillas y los apóstrofes.

Combinación de comillas y apóstrofes
------------------------------

Considere el siguiente código:

```php
echo 'Mi color favorito es el "rojo".';
```

Si utilizara retornos de carro para delimitar la cadena que se está escribiendo, sería ambiguo dónde empieza la cadena y dónde termina, así que utilicé apóstrofes para delimitar la cadena, lo que resuelve el problema.

Inserción de comillas en una cadena delimitada por comillas
---------------------------------------------------

Ocasionalmente, puede haber situaciones en las que necesite enumerar tanto las "comillas" como los "apóstrofos" dentro de la misma cadena. No sólo se puede utilizar la concatenación de dos cadenas, sino también el llamado **escapado** de caracteres.

```php
echo "Esta cadena contiene comillas.";
```

La barra invertida en este caso tiene el significado de "exactamente este carácter" y por lo tanto la comilla no será vista como el final de una cadena (o cualquier otra cosa) y por lo tanto siempre será una comilla. Otros caracteres extraños pueden marcarse de esta manera cuando no se pueda garantizar su durabilidad y se pueda malinterpretar el significado correcto.

Variable dentro de una cadena
-----------------------

Las comillas pueden ser muy complicadas porque permiten insertar variables directamente en una cadena:

```php
$color = 'Rojo';

echo "Moje oblíbená barva je {$color}, a tvoje?";
```

Mucho más seguros son los apóstrofos, que no lo permiten y hay que doblar la cadena:

```php
$color = 'Rojo';

echo 'Mi color favorito es' . $color . '¿y el tuyo?';
```

Buenos hábitos
--------------------------

En general, recomiendo utilizar apóstrofes (si es posible) para delimitar cualquier cosa, ya que son mucho menos comunes en las cadenas que las comillas.

Además, PHP es un lenguaje web, es decir, se utiliza para generar documentos HTML, donde las comillas son muy comunes precisamente porque también se utilizan para generar partes del código HTML. Personalmente, recomiendo acostumbrarse a utilizar estrictamente los apóstrofes en todas partes, porque así no hay que recordar lo que se encierra.

Comportamiento de las comillas
--------------------------

¡Cuidado! No tires las comillas por completo. Tienen algunas ventajas especiales que pueden ser útiles para el trabajo avanzado de PHP - sin embargo, los principiantes los consideran como errores y no los entienden.

```php
$x = 10; // establecer una variable
echo "Hodnota proměnné je: $x, přesně."; // y un listado
```

Dentro de las comillas, también se pueden enumerar los valores de las variables, o el signo de dólar hace que todo lo que va después sea una variable. Por lo tanto, si no quieres que salga el valor de una variable, sino el signo de dólar, tienes que escapar.

```php
$price = 25; // precio en dólares
echo "Cena produktu: $price\$"; // imprime "Precio del producto: 25 dólares"
```

La ventaja de las comillas es cuestionable en este caso y puede ser mejor utilizar apóstrofes y simplemente enlazar las cadenas.

```php
$price = 25;
echo 'Precio del producto:' . $price . '$'; // imprime lo mismo que el ejemplo anterior
```

> **Nota:** El precio en dólares está correctamente escrito en el formato `$25`, lo que causaría aún más confusión, porque la notación `$$price` en realidad llama a algo llamado `variable variable` (en el nombre de la variable llamamos al nombre de la variable a llamar - sólo una confusión).

**TIP:** Te puede interesar <a href="/promenna-variable">variable variable</a>.

Designación de la cadena
--------------------------

En general, todo lo que está entre comillas o apóstrofes se trata como una cadena. Así:

```php
$x = 5;   // esto es otra cosa,
$x = "5"; // que esto.
```

En el primer caso el **número** 5 se almacena en la variable **$x**, en el segundo caso la **cadena** "5" se almacena en la misma variable. Afortunadamente, esto no importa en PHP, y se puede trabajar con cualquiera de las dos variantes (casi) de la misma manera, porque PHP puede **transformar** automáticamente las variables según su contenido. Sin embargo, en general se recomienda no escribir los números entre comillas, especialmente para las operaciones de cálculo, en las que pueden producirse errores de redondeo.

A veces puedo querer almacenar la salida de una función en una variable:

```php
$pi = 3.14159;
$zaokrouhleni = round($pi); // esto es correcto
$zaokrouhleni = "round($pi)"; // esto está "mal" (la cuestión es qué salida espero).
```

El error en el segundo caso está justo en las comillas, cuando la salida de la función **round()** no se almacena en la variable **$round**, sino en la cadena que llama a esta función.
> **TIP:** El valor `$pi` no tiene que ser introducido directamente en el script de esta manera y podemos utilizar la función `pi()`, que es más precisa para cálculos más complejos.

Significado especial de la fuga
--------------------------

> **Atención:** Los siguientes ejemplos sólo funcionan entre comillas, si los encierra entre apóstrofes se comportarán como caracteres normales sin significado especial (excepto `\'` que escapa del apóstrofe).
El escape es para cuando quiero dar salida a algún carácter especial dentro de las comillas o a un apóstrofe que podría ser interpretado como una expresión del lenguaje y, por tanto, procesado, aunque el programador no lo pretendiera. Ya hemos mostrado un ejemplo; esta sección describe las posibles excepciones de comportamiento.

Esto se debe a que, a veces, la huida en sí misma tiene un significado especial. Ejemplo:

```php
echo "Texto largo, dividido en dos líneas.";
```

El ejemplo anterior se imprime:

```php
Dlouhý text, rozdělený na
dva řádky.
```

Así que si queremos escribir una barra oblicua, también debemos escaparla (escapar el carácter `n` no es necesario en este caso, porque se entendería como una envoltura de nuevo, o en este caso no debemos escapar en absoluto):

```php
echo "Texto largo, dividido en dos líneas.";
```

Hay más caracteres especiales como este, por ejemplo `t` hace una pestaña. La lista completa está en la documentación oficial.

Uso real de los caracteres especiales en el escape
-----------------------------------------------

Para empezar, me gustaría señalar que casi cualquier cosa se puede hacer en PHP de múltiples maneras, y los ejemplos dados aquí son sólo para ilustrar cómo se puede abordar el problema.

Por ejemplo, si queremos analizar el texto línea por línea, podemos utilizar la función <a href="/explode">explode</a>.

```php
$parser = explode("\n", $retezec); // analiza el texto línea por línea
```

En este caso, el uso del carácter especial `\n` tiene sentido, porque podemos decir muy eficientemente que queremos analizar por saltos de línea.

```php
$parser = explode('
', $retezec);
```

> **Atención:** En los sistemas Unix y Windows, los caracteres utilizados para marcar el final de las líneas se confunden. Por ejemplo, Windows utiliza `CRLF` (un par de caracteres `r\n`), mientras que Linux sólo utiliza `LF` (un solo carácter `\n`). Cuando se analice por líneas, debe tenerse en cuenta esto. Por lo general, el problema se resuelve normalizando los caracteres a `LF` solamente.

Resumen
-------

Si puedes, utiliza **apóstrofes** en todas partes.

Es bueno conocer el uso de las comillas y utilizarlas sólo cuando sea necesario (o en general sea bueno). Si el texto que se imprime puede contener comillas, enciérrelo entre apóstrofes (que se comportan de forma más predecible). Personalmente, utilizo las comillas para expresar varios caracteres especiales que son innecesariamente complejos de introducir en apóstrofos y que requerirían un complejo escape.
