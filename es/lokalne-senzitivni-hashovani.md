Hashing sensible a nivel local
==============================

> id: '76e09294-08ea-4dde-a431-2116595d9f04'
> slug:
> 	cs: lokalne-senzitivni-hashovani
> 	es: hashing-sensible-a-nivel-local
> 
> publicationDate: '2022-07-11 10:45:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: c23def9586aa05d73c4f1cf1fd1dda43

El principio de la mayoría de las funciones de hash de huellas dactilares de documentos es que siempre devuelven el mismo resultado para cada entrada. Esto se llama comportamiento determinista. Al mismo tiempo, un pequeño cambio en la entrada causará un gran cambio en la salida (devolviendo efectivamente un hash completamente diferente). Esto es especialmente útil cuando queremos comprobar si el documento de entrada ha cambiado, y si lo ha hecho, obtenemos algo completamente diferente. Un ejemplo de esta función es MD5 o SHA1.

Si queremos inferir el contenido de la entrada original a partir del hash, no hay una forma fácil de hacerlo y no tenemos más remedio que hacer fuerza bruta con cada opción hasta conseguir un resultado. Esto se debe a que, debido al gran cambio en la salida, no podemos determinar por iteraciones si nos estamos acercando al objetivo o no.

Algunas funciones de hashing como BCrypt para la misma entrada devolverán una salida diferente cada vez para hacerlas robustas al hashing de contraseñas. De hecho, la salida contiene directamente la sal, lo que hace imposible el llamado ataque de diccionario. Esta función es útil sólo para el hash de la contraseña, pero es muy poco adecuada para la revisión de documentos.

Comprobación de la similitud de los documentos y búsqueda de duplicación de contenidos
-----------------------------------------------------------

Imaginemos que estamos resolviendo un algoritmo de un buscador web que quiere comprobar cuánto ha cambiado una página web. También quiere comprobar rápidamente si el texto de una página es similar al de otra, o incluso si está completamente duplicado.

Una forma de comprobarlo es comparar cada página entre sí, lo que requiere muchos recursos del sistema. Otra opción es calcular un hash SHA1, pero este hash es el contenido de todo el documento, y si la página cambia por un solo carácter, obtenemos un hash diferente - por lo que no es adecuado para estos fines.

Por lo tanto, una posible solución es calcular el hash de diferentes áreas del documento de manera diferente y luego buscar los cambios en la salida de la función hash.

El principio del hashing sensible a la localización
----------------------------------

Hemos descargado el código HTML de una página web para la que queremos calcular un hash. Dividimos el documento en diferentes regiones (celdas) mediante un algoritmo que debe ser configurado correctamente.

Hacemos un hash de cada región por separado utilizando un algoritmo común y concatenamos el resultado en una sola cadena.

El resultado puede ser, por ejemplo, `3791744029724361934` (este hash de contenido fue proporcionado por la herramienta Ahrefs).

Por ejemplo, si sabemos que el contenido de la cabecera representa los 5 primeros caracteres y el pie de página los 5 últimos, sabemos que el centro de la cadena representa el contenido de la página. Si el contenido del pie de página cambia en todas las páginas (por ejemplo, el webmaster añade un nuevo enlace, cambia la fecha de actualización, etc.), cuando se descargue la nueva versión de la página, sólo cambiarán ligeramente algunos caracteres del hash en la zona de la derecha, y sabremos que sólo ha cambiado el pie de página, pero el contenido no ha cambiado. Así que podemos ignorar dicho cambio y no tenemos que revisar todo el sitio.

¿Cómo optimiza Google el rastreo de la web?
----------------------------------------

Los bots de Internet tienen que ahorrar en lo que puedan. El rastreo de la web es muy caro y las actualizaciones cuestan tiempo de computación.

Por ejemplo, al observar el comportamiento del algoritmo del robot de Google, observé que sólo responde a los grandes cambios de contenido. Si la página cambia poco, se rastrea de forma normal. Pero cuando, por ejemplo, el pie de página y la cabecera de un sitio cambian significativamente, lo evalúa como un rediseño y recorre la mayor parte del sitio a la vez para conseguir el nuevo aspecto lo antes posible.

También detecta los duplicados en los sitios de manera similar. Cuando un grupo de páginas son muy similares o tienen el mismo contenido, obtienen un hash muy similar, que el robot puede utilizar para verificar rápidamente que los documentos son similares y puede seleccionar el canónico e ignorar el resto.

Implementación de la función hashing
-----------------------------

No existe una implementación de la función hash sensible a la localización en PHP. Al mismo tiempo, no conozco ningún paquete disponible gratuitamente que implemente bien esta función.
