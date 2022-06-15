Incluir la seguridad en PHP y en el archivo adjunto
===================================================

> id: '820f8de6-ff1e-406c-8fe5-95080642656f'
> slug:
> 	cs: bezpecnost-include
> 	es: incluir-la-seguridad-en-php-y-en-el-archivo-adjunto
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1207d637c20fcb5e8f609be2eb449135'

A menudo, podemos querer adjuntar un archivo a una página que tenemos almacenada en el disco en otro lugar. Si introducimos su nombre exacto directamente en la función attach, no hay que preocuparse.

Adjuntar un archivo de forma segura
--------------------------

```php
include 'menú.html';
```

La escritura anterior es perfectamente segura porque siempre montamos el mismo archivo. En este caso no puede producirse un error de seguridad. El único problema que puede ocurrir es la ausencia del archivo **menu.html**, lo que provocará un mensaje de advertencia (que probablemente no aparecerá de todos modos), pero esta situación es rara porque normalmente adjuntamos un archivo cuya existencia es prácticamente segura.

Adjuntar un archivo según el patrón
--------------------------

¿Pero qué pasa si, por ejemplo, queremos adjuntar artículos a un sitio de contenido simple como éste? En este sitio, tengo una carpeta física donde se almacenan los artículos en formato HTML y los adjunto directamente al código fuente.

Sin embargo, ¡no basta con conectarse! Un principiante puede llamar a los artículos individuales así:

```php
include 'artículos/' . $_GET['Artículo'] . '.html';
```

> Pero esto es extremadamente peligroso. Un atacante puede pasar un enlace a otro directorio utilizando `../` o algo similar como nombre del artículo, y a veces es posible deshacerse de la terminación pasando un byte nulo al final. Al menos debería utilizar la función `basename()`, pero es mejor permitir sólo los valores de la lista blanca.

¿Por qué no cargar archivos no relevantes?
--------------------------

A menudo no nos importa cargar un archivo incorrecto (inesperado) - es culpa del usuario por solicitar una página que en realidad no quiere, pero puede haber situaciones en las que sí importa. En particular:

- El usuario carga un archivo al que el público no puede acceder y sólo el servidor puede hacerlo.
- La carga de algún otro script PHP puede desencadenar una acción inesperada o un mensaje de error que puede dar una pista sobre el funcionamiento del sitio y ayudar a realizar más ataques.
- La carga de otro archivo no sólo puede hacer que se añada al documento, sino también que se ejecute.

Lista blanca y validación de entradas
--------------------------

Si no tenemos la capacidad de validar la entrada de alguna manera segura (por ejemplo, desde la lista blanca), no deberíamos confiar en la honestidad del usuario y defender los scripts al menos a nivel de PHP de todos modos.

Lo primero importante es poner todos los archivos cargados en la misma carpeta (directorio) y desactivar algunos caracteres peligrosos, especialmente la barra y el punto. Esto hará imposible el acceso a otras carpetas que contengan archivos potencialmente vulnerables. La desactivación de los caracteres peligrosos también puede hacerse simplemente borrándolos de la cadena de entrada.

```php
$load = '../índice'; // esta entrada podría ser potencialmente peligrosa
$load = strtr($load, './', ''); // borra todos los puntos y barras de la cadena
include $load .'.html';
```

¿Archivos en ejecución?
--------------------------

Es importante tener en cuenta que la construcción **include** ejecuta los archivos cuando se conectan de la misma manera que si fueran código PHP, por lo que es una buena idea tener en cuenta esta posibilidad.

Sin embargo, a menudo adjuntaremos archivos que no es necesario ejecutar después, y sólo nos interesa el texto almacenado (contenido) en forma de cadena. Por lo tanto, podemos cargar el archivo en una variable y trabajar con él como una cadena, lo cual es bastante seguro.

```php
$load = '../índice'; // esta entrada podría ser potencialmente peligrosa
$load = strtr($load, './', ''); // borra todos los puntos y barras de la cadena
$file = file_get_contents($load . '.html'); // cargar el contenido en la variable
echo $file; // volcar el contenido del archivo
```

Esta solución parece interesante y segura a primera vista, y lo es. Incluso si el usuario logra llamar a un archivo PHP, éste nunca se ejecutará. Sin embargo, puede mostrarlo (es decir, su código fuente) y hay que tener cuidado con eso.

Reconocer el archivo deseado desde el script
--------------------------

No hay una guía definitiva para esto, cada uno tiene que hacerlo por sí mismo según las necesidades del guión. Por ejemplo, reconozco un artículo de otros archivos por el hecho de que contienen un encabezado de tamaño H1. Así que si alguien carga un archivo donde no hay encabezado, no muestro nada y la página termina con un mensaje de error. Siempre es importante encontrar alguna característica única que sólo tengan los archivos que quieres y que no tengan los demás, y a partir de ahí puedes seguir.

Conclusión
--------------------------

Aunque la validación y la carga de archivos es relativamente sencilla, un gran número de principiantes sigue cometiendo errores, y seguirá haciéndolo. Lo más importante es acertar con el significado de lo que cargamos y saber distinguir el contenido que queremos del resto. Y lo más importante, trabaja con el contenido como una cadena y nunca lo cargues directamente en la página.
