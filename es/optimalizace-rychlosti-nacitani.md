Cómo optimizar eficazmente la velocidad de carga de las páginas
===============================================================

> id: '72f1d564-cb70-431c-a3dd-a3fdcaf14292'
> slug:
> 	- null
> 	es: como-optimizar-eficazmente-la-velocidad-de-carga-de-las-paginas
> 
> cs: optimalizace-rychlosti-nacitani
> publicationDate: '2021-10-15 15:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '013c8f29c213d3f3f1df3fbe4be8d572'

Cuando viajo, a menudo me encuentro con malas conexiones a Internet, lo que, como diseñador web, me hace pensar en los principios de diseño para resolver la velocidad de la web incluso con una mala conexión.

La práctica me ha dado algunos trucos útiles:

Las páginas de destino importantes suelen ser sencillas y vale la pena utilizar estilos CSS personalizados
-----------------------------------------------------------------------------------

Por ejemplo, la página de acceso al CMS suele ser muy sencilla. ¿Realmente un simple formulario necesita descargar todo Bootstrap y otros frameworks CSS/JS? Vale la pena optimizar las páginas importantes y escribir código nativo.

Una vez que la página está completamente renderizada, tenemos unos segundos en los que el usuario rellena sus datos, y podemos descargar y almacenar en caché los estilos restantes en el navegador en segundo plano. Cuando el usuario se conecte, probablemente ya tendrá descargado Bootstrap (y si está en Edge, no lo tendrá...).

En lugar de iconos, a menudo podemos utilizar emoji
-----------------------------------

¡De verdad! El emoji tiene muchas ventajas prácticas:

- Sólo ocupa 4 bytes, así que supera a cualquier imagen
- Nunca falla la descarga, por lo que el usuario siempre ve al menos un icono en lugar de un espacio en blanco
- Se muestra en el estilo visual del usuario
- Actualmente ya cuenta con una amplia compatibilidad de dispositivos
- Se ahorra la petición al servidor y no tenemos que ocuparnos de dónde obtener la imagen (esto se puede optimizar a través de CDN, pero para qué, verdad)
- Un montón de iconos con los que probablemente no quieras tratar. Por ejemplo, ¿dónde conseguir los iconos de las banderas de todos los países que quieres mostrar en la administración? Utilice el emoji: https://github.com/bara.../country/blob/main/flag-emoji.json

Utilizar estilos críticos directamente en el HTML al cargar el sitio
------------------------------------------------------

A veces pongo los estilos CSS importantes que definen el diseño de la página y la maquetación básica directamente en el HTML. Pongo un límite de 1 KB, en el que intento meter lo máximo posible. La desventaja de este enfoque es que los estilos tienen que ser transferidos en cada solicitud y no pueden ser almacenados en caché, por otro lado se descargan incomparablemente más rápido que una imagen.

No soy un experto en velocidad de carga, [Martin Michalek](https://www.programia.cz/rozhovor-martin-michalek-rychlost-webu/) te lo diría mejor.

¡Usa ajax!
--------------

Siempre uso ajax para recuperar contenido poco importante o lento. Aumenta un poco los requisitos tecnológicos de la aplicación, pero me lo puedo permitir.

Un ejemplo de un buen lugar para usar ajax es casi todo en la administración. Muy a menudo hay muchos datos que recuperar, pero el usuario no está interesado en todo. Cuando el usuario sólo tiene descargado un cliente javascript delgado y todos los datos se obtienen a través de Vue y json, sólo se descarga la cantidad mínima de datos y las respuestas son instantáneas.

Cómo hacer esto en Vue: https://vue.baraja.cz/api-a-ajax-ve-vue-js

En el backend, utilizo la biblioteca de Nette: https://github.com/baraja-core/structured-api

Utilice una CDN adecuada
---------------------

Para la distribución de contenido estático, recomiendo utilizar una CDN para todo tipo de sitios. Si no sabes cómo configurar una CDN, al menos utiliza Cloudflare en modo proxy. El contenido estático se almacenará automáticamente en la caché basándose en las cabeceras HTTP. No todos los proveedores de alojamiento tienen Pops bien configurados, y para las rutas largas esto le ahorrará cientos de milisegundos en cada petición.

Formato de imagen adecuado
---------------------

Uno de mis juniors puso hace poco una imagen PNG en la página principal del sitio, que contenía una foto y ocupaba 3 MB. Dijo que estaba bien porque la página se cargaba rápidamente en su conexión (lo hace en la óptica de su casa, ¿no?), además dijo que hoy en día transferimos muchos datos, como el vídeo.

Yo estoy chapado a la antigua en esto y optimizo lo que puedo.

Fotos a JPG, o mejor a WEBP. Pero guardar una imagen en JPG no significa nada, hay que pasarla por un servicio de optimización: https://tinyjpg.com

Si tienes imágenes en Git, esta herramienta puede comprimirlas automáticamente y enviar un pull request: https://imgbot.net. Cuando se añade una nueva imagen, vuelve a enviar el PR.

Si necesitas comprimir miles de imágenes (como toda una galería de productos en un sitio web), uso una aplicación de escritorio para Mac para hacerlo: https://imageoptim.com/mac

Comprimir las imágenes adecuadamente suele ser lo que más datos ahorra. A veces incluso el 50%.
