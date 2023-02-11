¿Cómo seleccionar las tecnologías? ¿Cuándo pasamos a JavaScript?
================================================================

> id: d3ea7ea7-3e2f-455d-a424-cce374bcd1d2
> slug:
> 	cs: senior-9-vyber-technologii
> 	es: como-seleccionar-las-tecnologias-cuando-pasamos-a-javascript
> 
> publicationDate: '2023-02-11 14:40:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '72807451867478e1a7b6d67f4e5c31b3'

Elegir las tecnologías adecuadas es un requisito previo para convertirse en desarrollador senior. Estas decisiones no suelen ser fáciles, porque hay que tener en cuenta el estado técnico actual de la aplicación, hacia dónde se dirige el desarrollo, cuáles son los conocimientos de su equipo actual, qué conocimientos son habituales en el mercado laboral, cuáles son los costes de cada tecnología, qué riesgos aportará a su funcionamiento, cómo de segura y estable es la tecnología y, por último, pero no por ello menos importante, en qué estarán interesados los desarrolladores, digamos, dentro de 5 años, cuando el 80% de su equipo actual haya sido sustituido.

He pasado por 6 grandes empresas que desarrollan en PHP. Sólo 2 de ellos intentan cambiar a otra tecnología a largo plazo, los demás se quedan. Eso tiene muchos problemas. Por ejemplo, actualmente estoy tratando de encontrar un desarrollador PHP senior para un proyecto corporativo que estoy desarrollando para O2, con el requisito de desplazarse a las oficinas de Praga, y puedo ver cómo el mercado de desarrolladores PHP se ha despejado en los últimos 5 años. PHP ya no mola y no hay mucha gente que quiera hacerlo. No hay suficientes jóvenes en ella.

Al entrevistar a gente más joven, percibo que React y, en general, las tecnologías "finas" son muy populares hoy en día. Desde el punto de vista de la arquitectura de aplicaciones, tiene sentido si se descubre esta dirección en una fase temprana y se dispone de tiempo para adaptarse. En lugar del complejo machaqueo de maquetaciones y formularios web en Latte, para el que necesitas un desarrollador prácticamente mediocre cuando la tarea ya es un poco más compleja, en React sólo necesitas un junior que empezó hace básicamente un mes, y que aún no comete demasiados errores en la futura solución.

React te permite desechar una gran parte del backend que se escribió sólo para que el frontend pudiera existir en primer lugar. En resumen, hace que el desarrollo sea más barato y, como extra, se obtiene una entrega más rápida de nuevas funciones porque los desarrolladores no tienen que lidiar una y otra vez con los complejos problemas que plantea el lenguaje de diseño PHP.

La mayoría de las aplicaciones web ya ni siquiera necesitan un backend, o sólo un backend mínimo. Cuando expones puntos finales de API en Node.js (también una tecnología construida sobre javascript), de repente un desarrollador que antes solo hacía React puede escribir piezas del backend también, porque es el mismo lenguaje.

En un análisis profundo de los proyectos que he desarrollado en los últimos 5 años, sólo hay unas pocas cosas que faltan en Node.js que todavía me hacen usar PHP para algunas operaciones.

A saber:

- Doctrine (y en general el acceso a bases de datos relacionales basado en entidades objeto)
- Envío y gestión de correos electrónicos
- API SOAP (por desgracia, a veces sigue existiendo)
- Sesiones (hay que sustituirlo por un token JWT, por ejemplo)
- Lógica heredada compleja que se escribió en PHP y no puedo refactorizar fácilmente
- Procesamiento rápido de estructuras de datos complejas en las que es necesario mutar los datos
- Personas del equipo a las que hay que volver a formar para que hagan algo nuevo.

Pero entonces llega Node.js, que hace el resto de cosas mejor. Por ejemplo:

- La posibilidad de descargar la aplicación directamente a la nube
- Desarrollo mucho más barato (quizá incluso el doble) de la misma funcionalidad.
- Misma lógica en BE y FE sin tener que escribir código dos veces
- Puntos finales de la API REST
- Llamadas paralelas a varios códigos a la vez
- Posibilidad de enviar una respuesta HTTP, pero el código sigue ejecutándose
- Amiguetes
- Bibliotecas para trabajar con servicios en la nube
- Mejora significativa del tiempo de respuesta al no tener que arrancar una aplicación enorme.
- Paradigma totalmente funcional (te deshaces de DIs que no necesitas en JS, por ejemplo).
- Trabajar con formularios y datos
- Actualizaciones sencillas y una comunidad de desarrolladores activa

**Comentario sobre Doctrine:** Sé que JS trae un montón de librerías para trabajar con bases de datos. O incluso nuevos paradigmas como Mongo. Me gusta hacia dónde se dirige el procesamiento de datos. Por otro lado, creo que las bases de datos relacionales tabulares nunca desaparecerán. Cuando se realiza un proyecto realmente grande en el que se gestionan decenas de millones de registros, simplemente ya se necesita una tecnología tradicional con la que se está muy familiarizado y de la que se sabe qué esperar. Por ejemplo, la idea de que yo quiera añadir una columna (propiedad) y eso suponga remapear todas las entidades con un script de migración me da bastante miedo.
