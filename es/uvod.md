Introducción a PHP
==================

> id: '66b7fd22-6195-4999-ad8d-b799afdc1876'
> slug:
> 	cs: uvod
> 	es: introduccion-a-php
> 
> perex:
> 	- 'Úvodní tutoriál do jazyka PHP, možnosti jazyka, informace o tvorbě webu v PHP.'
> 	- 'Tutorial de introducción al lenguaje PHP, opciones del lenguaje, información sobre el desarrollo web en PHP.'
> 
> publicationDate: '2019-09-29 19:25:06'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: db6ba8fe6d5ca28aab62d7302c1d7527

Este es un curso en línea para aprender PHP, diseñado para la educación general. Está disponible de forma gratuita en este sitio web. Los textos no deben utilizarse en ningún curso de pago sin confirmación escrita. Puede utilizar las muestras utilizadas en todo el sitio en su aplicación sin más restricciones. [Términos y condiciones](https://baraja.cz/vseobecne-obchodni-podminky).

Actualizar desde HTML
--------------

"¿Actualización? Suena más bien como un giro mediático, y de hecho lo es.

No hay actualización. PHP conserva toda la funcionalidad y las características de un documento HTML puro, sólo añade nuevas opciones de escritura y despliegue. Genial, ¿verdad?

Usted ya sabe, por haber desarrollado páginas HTML estáticas, que el código se compone de etiquetas, que se definen como palabras clave encerradas entre corchetes puntiagudos (por ejemplo, `<b>Hola!</b>` expresa el texto en negrita **Hola!**).

PHP se inserta en la página HTML como las etiquetas `<?php` y `?>`, dentro de las cuales se escribe otra lógica de aplicación. Es importante destacar que PHP tiene su propia sintaxis (las reglas con las que se escribe el código) y, a diferencia de HTML, no es tolerante a los errores.

Más adelante se darán ejemplos concretos de cómo hacerlo y qué significa cada etiqueta. Para empezar, es importante entender los principios generales de cómo funciona PHP en el servidor y cómo se procesa el código.

El flujo de comunicación entre el usuario y el servidor
---------------------------------------

Para una página HTML ordinaria, esto es más o menos como funciona:

> El usuario envía una solicitud (pide una página HTML específica), el servidor mira el disco y devuelve exactamente lo que está almacenado. No ocurre nada especial, y no esperes nada más. Las páginas serán simplemente estáticas, sin posibilidad de interacción con el servidor.

Sin embargo, si añadimos PHP, empezarán a ocurrir maravillas:

> El usuario vuelve a solicitar la página. El servidor abre el archivo en el disco, pero ve que no sólo contiene HTML puro, sino también etiquetas especiales que indican un script PHP. Así que primero los evalúa y luego envía lo que generó PHP.

La evaluación del código PHP se realiza por defecto cada vez que se carga la página, en el futuro se aprenderá a cachear el código (almacenarlo compilado para un procesamiento más rápido).

La diferencia entre el procesamiento de scripts PHP y C/C++
------------------------------------------

Es posible que haya aprendido a utilizar el lenguaje `C` o `C++` en la escuela. PHP se basa directamente en la sintaxis del lenguaje `C`, y dentro del núcleo se utiliza el lenguaje `C`, por lo que es bueno conocer algunos puntos en común y a la inversa las diferencias.

El principio básico de los programas compilados comunes (que se ejecutan de forma local directamente en el ordenador o el smartphone) es cargar la lógica de la aplicación en la memoria operativa, que se comunica directamente con el sistema operativo, que recibe la entrada del usuario y luego muestra la salida del programa al usuario. Lo importante es que el programa se ejecute de forma aislada desde su inicio hasta su finalización.

PHP comienza con cada solicitud de renderización de la página, recarga todo el código y los datos cada vez, y luego sale. Por lo tanto, los scripts de PHP tienen una vida literalmente yuppie y suelen existir sólo durante decenas de milisegundos.

La ventaja de este enfoque es un mayor nivel de aislamiento: si algo va mal, todo se recarga con la siguiente carga de la página. Por otro lado, este enfoque tiene mayores requisitos de rendimiento porque tenemos que conectarnos repetidamente a la base de datos, leer archivos del disco, etc., por ejemplo.

En el futuro, aprenderá que puede mantener los scripts de PHP cargados en la memoria operativa utilizando la extensión `OP Cache`, que la mayoría de los nuevos servidores (a partir de PHP 7.1) tienen establecida en la configuración básica.

Descarga de scripts PHP extranjeros
--------------------------

Una pregunta relativamente común que discutimos con los estudiantes es cómo descargar scripts PHP ajenos del servidor y ver su código fuente. Esta cuestión viene precedida de la consideración de que el código HTML de una página puede mostrarse fácilmente en un navegador web.

La respuesta es que **los scripts de PHP no se pueden descargar**. Esto se debe a que el código PHP se evalúa primero en el servidor web, y luego el código HTML generado (u otra salida) se envía al navegador del usuario. Por lo tanto, sólo se puede descargar la salida del script PHP, no el propio script.

¿Cómo funciona la generación a HTML?
---------------------------------

No funciona literalmente así, estarás navegando por páginas HTML. El script PHP debe estar siempre en un archivo PHP.

Hasta ahora, la mayoría de ustedes estaban acostumbrados a crear enormes carpetas llenas de archivos que terminaban en la extensión **.html**. Ahora serán muchos menos archivos. En el caso extremo, puede ser un solo archivo.
