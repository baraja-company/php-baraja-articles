Cómo funciona el Captcha (imagen descriptiva)
=============================================

> id: '9cf191e4-a49b-407b-b16e-21de7224ac43'
> slug:
> 	cs: captcha
> 	es: como-funciona-el-captcha-imagen-descriptiva
> 
> perex:
> 	- 'Captcha je druh obrázku, který ověří, že uživatel není robot a chrání webovou aplikaci. Možnosti implementace v PHP.'
> 	- Un captcha es un tipo de imagen que verifica que el usuario no es un robot y protege la aplicación web. Opciones de implementación de PHP.
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '80357b2e7f0047bb488922f2ab7b4336'

El captcha es actualmente una de las formas más comunes de proteger los formatos libres. Se creó originalmente no para proteger la seguridad de los datos, sino para proteger contra el spam y reconocer que es un humano.

Sin embargo, es generado por una máquina, por lo que no siempre es perfecto, pero la tasa de éxito de una prueba de captcha ronda el 99% y sólo el 1% de las imágenes pueden ser descifradas por un robot de spam bien hecho.

Cómo funciona
--------------------------

Hay más opciones, por ejemplo yo uso esta solución:

- El servidor recibe la información de que el usuario ha solicitado una página de formulario y comienza a generarla.
- En el futuro campo de prueba del captcha, pone una imagen con una dirección secreta que no dice nada (como un número aleatorio, una cadena de caracteres, ... lo que sea).
- La imagen se genera y se almacena en esa dirección. Al mismo tiempo, la transcripción correcta se escribe en algún lugar (quizás en una sesión o base de datos).
- Cuando el usuario envía el formulario, se comprueba si la transcripción coincide con lo que ha introducido. Si lo hace, el formulario se procesa. Si no es así, se solicita una nueva descripción de otra imagen.
- Después de los intentos, tanto exitosos como fallidos, la imagen temporal del captcha es eliminada (para no volver a ser utilizada). Si el formulario no se envía en un plazo determinado, también se eliminará (caduca).

Transcripción correcta
--------------------------

Si el test de captcha puede ser resuelto, el usuario es probablemente humano. Pero es bueno pensar en los usuarios que no pueden resolver la tarea, especialmente los usuarios ciegos. Es una buena solución para combinar varias pruebas posibles (sobre todo la precarga de voz). Sin embargo, el reconocimiento de la voz por parte de una máquina es actualmente mucho más eficaz que la lectura de una imagen. Por lo tanto, esta solución no siempre es ideal.

Imagen captcha simple personalizada
--------------------------

A menudo, basta con generar una imagen en blanco de determinadas dimensiones e introducir en ella algunos caracteres de forma legible sin necesidad de editarla. ¡En serio! La mayoría de los bots de spam son estúpidos y no pueden atacar los formularios genéricos con este tipo de protección, aunque sea un texto perfectamente legible que un mejor OCR puede transcribir perfectamente.

La imagen resultante puede tener este aspecto:

```txt
<img src="captcha.php" alt="ukázková captcha">
```

Prueba a actualizar la página varias veces y verás que el código cambia aleatoriamente cada vez. Para fines de demostración, sólo se genera sin guardar, por lo que se eliminará inmediatamente después de cargarlo.

Resolví el código fuente utilizando la biblioteca PHPGD, que está disponible en prácticamente todas las instalaciones y alojamientos de PHP:

```php
Header("Tipo de contenido: image/png");
$obr = ImageCreate(100, 35);
$pozadi = ImageColorAllocate ($obr, 219, 28, 49); //definición del color de fondo
$bila = ImageColorAllocate ($obr, 255, 255, 255); //Definición del color blanco para el texto
$styl = array ($pozadi);
ImageSetStyle ($obr, $styl);
  $nahodne_cislo = rand(11111,99999); //sacando un número aleatorio de 5 caracteres
  imagestring($obr, 5, 25, 10, $nahodne_cislo, $bila); //función para dibujar texto (en este caso un número)
ImagePNG($obr); //generar la imagen en la memoria y renderizarla
ImageDestroy($obr); //borrar la imagen de la memoria (ya no será necesaria, porque se genera una vez)
```

La representación de la imagen es entonces sólo una cuestión de HTML:

```html
<img src="captcha.php">
```

Tenga en cuenta que esta secuencia de comandos no es funcional por sí sola sin más modificaciones. Sólo sirve como demostración para generar una imagen sencilla.

Resumen
--------------------------

Las pruebas de captcha son bastante fiables, pero son molestas y consumen mucho tiempo. A veces no son legibles, por lo que es bueno dar al usuario la opción de cargar una imagen diferente usando javascript para no tener que recargar toda la página y tener que rellenar todo de nuevo.

Recuerda: Lo que es una tarea trivial para un humano puede no ser nunca una solución alcanzable para una máquina. Por eso, incluso este primitivo captcha con texto perfectamente legible puede proteger contra más de la mitad del spam.
