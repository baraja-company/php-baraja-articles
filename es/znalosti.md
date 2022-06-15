Visión general de los conocimientos de desarrollo web
=====================================================

> id: e5e9613c-66fe-4d5e-a686-a182aab8061a
> slug:
> 	cs: znalosti
> 	es: vision-general-de-los-conocimientos-de-desarrollo-web
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7284b1fc11b40ddb7628995070198a1

Saber crear un sitio web y luego cuidarlo de forma integral no es sólo cuestión de hacerlo. Hay muchos obstáculos en el camino y es bueno tener al menos una idea básica de cada cosa. Cuando empecé, no sabía realmente todo lo que había que aprender. Esta página sirve de señalización de los temas que tuve que estudiar gradualmente para poder entender el desarrollo web y hacer frente a las situaciones más comunes.

Administración de servidores
--------------

> Un servidor web es un ordenador que ejecuta la web. Cuando un usuario ve una página, el servidor web se la envía.
>
> En este momento (2021), ya no tiene sentido obtener un alojamiento gratuito si te tomas en serio la web. Un servidor puede ser **alquilado desde 50 coronas al mes**.

- Instalación del servidor (difiere en Windows y Linux)
- Configuración del servidor
	- Configuración de Apache
	- Configuración de PHP
	- <a href="/info">obtención de la configuración PHP actual</a> (función `phpinfo()`)
	- Enrutamiento Ngnix (mejora el rendimiento del servidor)

Internet y navegador web
--------------------------------

- Navegadores web
- Principio de solicitud y respuesta
	- Dirección URL
	- Ajax y Ajaj
	- Generación de código HTML (sistemas de plantillas)

Cuerdas
-----------------

- Lectura, escritura y concatenación de cadenas, especialmente el trabajo básico con cadenas
- Procesamiento de cadenas
	- Búsqueda carácter por carácter
	- <a href="/if">comparar cadenas</a>
	- Similitud de cadenas (funciones `levenshtein()`, `similar_text()` y `soundex()`)
	- <a href="/explode">Explotar</a>, dividir por separador
	- Las expresiones regulares** dividen las cadenas según una máscara universalmente configurable
	- **Tokenizer** rompe las cadenas en partes pequeñas (tokens)
- Serialización de datos en una cadena
	- <a href="/json">Json</a>, un objeto javascript almacenado en una cadena
