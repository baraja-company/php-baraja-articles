Cookies en PHP
==============

> id: '392dd88b-d2f5-4943-a993-01aaad7ccd32'
> slug:
> 	cs: cookies
> 	es: cookies-en-php
> 
> perex:
> 	- 'Cookies je krátká textová informace v paměti prohlížeče, kam můžeme v PHP zapisovat a označit si uživatele.'
> 	- Una cookie es un breve trozo de información de texto en la memoria del navegador donde podemos escribir y marcar al usuario en PHP.
> 
> publicationDate: '2019-09-11 10:18:29'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '78f9aad0278acd51b98179e7d29fe3d8'

> **Atención:** Este artículo fue escrito hace muchos años y parte de la información puede ser obsoleta o incorrecta. Téngalo en cuenta al leerlo.

Las cookies son pequeños fragmentos de información textual que se almacenan en el navegador del visitante de un sitio web. Siempre se transfieren con cada página que se recarga, y pueden ser borrados, modificados y leídos por el usuario en cualquier momento, por lo que no son adecuados para almacenar información personal.

*Advertencia: si su sitio web utiliza cookies para rastrear a los usuarios o complementos de terceros (por ejemplo, el botón "Me gusta" de Facebook, el medidor de tráfico de Google Analytics, los anuncios de banner), debe informar al usuario al respecto.

> "Una nota más: su sitio no debe contener publicidad ni códigos de medición hasta que haya obtenido el consentimiento. Y eso apesta".
>
> -- <a href="https://phpfashion.com/jak-na-souhlas-s-cookie-ve-zkurvene-eu">David Grudl</a>

Leer un valor de una cookie
--------------------------

Todas las cookies se almacenan en la variable superglobal `$_COOKIE`, que almacena cada clave como un array.

Por ejemplo, si hemos almacenado el nombre del usuario actualmente conectado bajo la clave `user` en la cookie, podemos recuperarlo fácilmente:

```php
echo $_COOKIE['usuario'];
```

Tenga en cuenta: es posible que no siempre existan cookies (por ejemplo, si es un usuario nuevo). Por lo tanto, debemos comprobar siempre la existencia de cookies antes de cualquier anuncio y ofrecer un mensaje de error alternativo si es necesario.

```php
if (isset($_COOKIE['usuario']) && $_COOKIE['usuario']) {
    echo 'Usuario registrado:' . $_COOKIE['usuario'];
} else {
    echo 'No se ha registrado nadie.';
}
```

Obtener todas las cookies disponibles
--------------------------------

Dado que todas las cookies se almacenan en la variable superglobal `$_COOKIE`, se pueden listar fácilmente:

```php
var_dump($_COOKIE);
```

O, alternativamente, recorrer el ciclo y obtener todas las claves y valores:

```php
foreach($_COOKIE as $key => $value) {
    echo $key . ':' . $value;	// escribir la clave y el valor
    echo '<br>';				// envolver la línea
}
```

Almacenar el valor en una cookie
--------------------------

La función `setcookie()` se utiliza para almacenar datos en las cookies.

El primer parámetro es la clave de la cookie, que se utiliza para leerla desde el campo `$_COOKIE`, y el segundo parámetro son los datos en sí mismos como cadena.

Con el tercer parámetro podemos (opcionalmente) establecer el periodo de validez durante el cual la cookie estará disponible. La hora de disponibilidad se da como <a href="/fecha">timestamp</a>, por lo que si queremos establecer una cookie con una validez de 1 hora a partir de este momento, sólo tenemos que escribir `time() + 3600`.

```php
$data = 'Algunos contenidos que queremos almacenar.';

setcookie('TestCookie', $data);
setcookie('TestCookie', $data, time() + 3600);
```

Almacenamiento de datos más grandes
-------------------

Las cookies no son adecuadas para almacenar datos de mayor tamaño (los navegadores suelen permitir que se almacenen sólo 4 kB y un máximo de 20 cookies, el tamaño también incluye los nombres de las cookies, los ajustes de validez, etc.).

Es mejor almacenar datos más grandes en el servidor y sólo poner un identificador en la cookie, por el cual podemos saber a qué usuario pertenece. Este método se llama `$_SESSION` y se discute en un artículo separado.

Si no necesitas almacenar los datos siempre de forma sincrónica en el servidor, puedes utilizar el almacenamiento **<a href="https://jecas.cz/localstorage">localstorage</a>** disponible en javascript. Su capacidad es del orden de MB y los datos no están sujetos a caducidad.
