Cómo escribir su primer script PHP
==================================

> id: '2d6986dc-f24b-4ae5-b24c-d5bb9bf94512'
> slug:
> 	cs: prvni-script
> 	es: como-escribir-su-primer-script-php
> 
> perex:
> 	- Jak napsat první PHP script? Úvod do PHP pro začátečníky.
> 	- ¿Cómo escribir su primer script PHP? Introducción a PHP para principiantes.
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: a5fab0e5edf99323140e497a6fe52322

Antes de escribir nuestro primer script PHP, primero necesitamos explicar teóricamente cómo cargar una página usando PHP.

1. En primer lugar, el usuario llama a una URL específica en su navegador web, por ejemplo `https://baraja.cz`.
2. A continuación, el navegador web construye una `request`, que es una petición especial al servidor web que se envía a Internet. Contiene información sobre la página solicitada, la identificación y configuración básica del navegador, información sobre las cookies, etc.
3. Esta `solicitud` viaja a través de Internet hasta un servidor web (casi siempre `Apache`), que lee la solicitud y comienza a compilar una respuesta.
4. Como la URL llamada es un script PHP y la petición es para un archivo llamado `index.php`, `Apache` lee el archivo `index.php` del directorio raíz en el disco y lo pasa al `interprete de PHP`, que es un programa que puede procesar el código PHP en sí mismo y construir un código `HTML` basado en él, que es enviado de vuelta al usuario.
5. Una vez compilado el código HTML, la respuesta se devuelve al usuario (lo que se denomina `respuesta`) y el navegador web representa la página de forma normal, como si fuera HTML puro.

> Tenga en cuenta que el navegador web no aprende nada sobre el contenido del script PHP, sino que sólo procesa el HTML generado, por lo que sus scripts y el contenido del servidor permanecen seguros.

Vamos a crear el primer script
----------------------------

> Escribir tu primer script supone que tienes un servidor web funcionando en tu ordenador. Para Windows, <a href="https://www.apachefriends.org/index.html">XAMPP</a> es lo mejor (descarga la versión 7.0 de PHP o posterior), y <a href="https://www.apachefriends.org/index.html">XAMPP</a> funciona exactamente igual en Mac que en Windows. Para Linux, recomiendo <a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">Servidor LAMP</a> (este sitio también funciona en un servidor Lamp).

El nombre del archivo de script PHP debe terminar con la extensión `.php` para que el servidor web sepa que queremos procesarlo según las reglas de PHP. Así que vamos a crear un archivo de ejemplo `index.php` que contendrá el código de la página principal de nuestro sitio web.

Abrir el archivo
--------------------------

Abra este archivo en un editor de texto adecuado para escribir el código fuente.

En Windows, por ejemplo, <a href="https://www.sublimetext.com">Sublime Text</a> es un buen lugar para empezar, ya que colorea agradablemente la sintaxis (reglas del lenguaje) y hace que el código sea más fácil de leer. Más adelante, recomiendo comprar <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, que se utiliza mucho en las empresas y ofrece la posibilidad de programar en múltiples personas.

Escribir la estructura básica de una página HTML
--------------------------

Probablemente ya conozca la estructura básica de una página HTML:

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>

    </body>
</html>
```

Todo el código HTML se manejará de forma normal y será una gran ayuda para el diseño del sitio. PHP utiliza en gran medida los principios de HTML y CSS.

Separación del script PHP del código HTML
--------------------------

PHP es principalmente un lenguaje de plantillas que genera contenido personalizado en lugares apropiados del código. Para poder distinguir claramente lo que es HTML y lo que es PHP, necesitamos utilizar una etiqueta separadora.

Actualmente, es mejor utilizar la notación con `<?php` y `?>`.

```php
// aquí está el código PHP
?>
```

> Utilizamos el terminador `?>` si queremos utilizar algún otro código HTML. Si no hay más código HTML al final del script PHP, es preferible no incluir la etiqueta `?>`, para que no haya espacios en blanco y saltos de línea innecesarios al final de la página que pueda insertar el editor de texto.

En el pasado, se ha utilizado mucho la etiqueta `<?` en lugar de `<?php`, pero no siempre es compatible.

Las etiquetas Wrapper pueden colocarse en cualquier lugar del código HTML, como en el cuerpo de la página:

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>
    <?php
        // tady bude PHP kód
    ?>
    </body>
</html>
```

Construcciones básicas
--------------------------

Entre los bloques de construcción más básicos se encuentran:

- <a href="/echo">Enumerar el contenido en el código fuente</a>
- <a href="/variable">Variables</a>
- <a href="/if">Condiciones</a>

En esta sección, demostraremos un listado simple de contenido a código fuente usando variables.

El principio de la construcción del código fuente
--------------------------------

Todas las construcciones (expresiones del lenguaje), declaraciones y funciones están separadas por puntos y comas para que no haya ambigüedad en cuanto a la validez de la construcción actual desde y hasta.

El punto y coma suele ir seguido de un salto de línea.

Escrito simbólicamente:

```php
příkaz;
další příkaz;
proměnná x = její hodnota;

vypsat proměnnou x;

uložit do souboru;
```

Extraer al código fuente
--------------------------

La construcción <a href="/echo">echo</a> se utiliza para listar el contenido.

Es muy fácil de usar:

```php
echo '¡Hola, mundo!';
```

A continuación, imprime el texto "¡Hola mundo!" en el código HTML. Prueba la muestra.

> Todas las demás demostraciones sólo contendrán el interior del código PHP. El código HTML que lo rodea es libre para que usted lo decida (por ejemplo, utilice el ejemplo que aparece al principio de este artículo).

Variables
--------------------------

Las variables son posiciones de memoria virtuales que almacenan datos y se utilizan para moverlos. El nombre de una variable siempre comienza con un "dólar", seguido del propio "nombre" y luego su "valor".

> He resumido una descripción detallada de cómo funcionan las variables en <a href="/variable">un artículo aparte sobre variables</a>.

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo;
echo '<br>';
echo $jmenoAutora;
```

> El nombre de la variable debe expresar lo que realmente contiene la variable para que el código sea más claro. Observe también la inserción de la etiqueta HTML `<br>` para sangrar el texto. Ya debería estar familiarizado con esta etiqueta de HTML.

Lo que sale en la construcción `echo` se llama cadena (una secuencia de caracteres). Las cadenas individuales pueden concatenarse con un punto (`.`) para reducir la salida a una sola línea:

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo . '<br>' . $jmenoAutora;
```

> Después de conectar las cadenas con un punto, todo se verá como una gran cadena.

Operaciones variables
--------------------------

Entre variables, todas las operaciones básicas <a href="/matemáticas">matemáticas</a> funcionan perfectamente de forma intuitiva como se espera.

Definamos 2 variables y pongamos números en ellas:

```php
$x = 5; // define la variable x con valor 5
$y = 3; // define una variable y con valor 3

echo $x + $y; // añade las variables e imprime 8
```

> Tenga en cuenta que el signo de igualdad (`=`) no se utiliza para realizar una operación matemática, por lo que no puede escribir ecuaciones, por ejemplo. En este sentido, PHP actúa como una calculadora.

Si no queremos utilizar variables, podemos realizar las operaciones directamente. Así que la ubicación de las operaciones no importa y evaluarán en cualquier lugar.

```php
echo 5 + 3; // imprime 8
```

Alternativamente, podemos sumar las variables y almacenar el resultado en otra variable:

```php
$x = 5;
$y = 3;
$z = $x + $y; // la variable $z contiene 8

echo $z; // imprime 8
```

En la siguiente parte, aprenderemos los fundamentos completos de <a href="/principles-of-variables">definición y uso de variables</a>.
