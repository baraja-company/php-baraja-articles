Envío de correos electrónicos (funciones mail() y SMTP) en PHP
==============================================================

> id: '3051b5ea-9408-4aaa-8209-e3c527ef8ad2'
> slug:
> 	cs: odesilani-emailu-mail-smtp
> 	es: envio-de-correos-electronicos-funciones-mail-y-smtp-en-php
> 
> perex:
> 	- 'Možnosti odesílání e-mailů v PHP, funkce mail(), SMTP, hlavičky, konfigurace a Nette Mailer.'
> 	- 'Opciones de envío de correo electrónico en PHP, mail(), SMTP, cabeceras, configuración y Nette Mailer.'
> 
> publicationDate: '2019-11-26 12:00:02'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '53dc8075e16053ea9284299987c64952'

En PHP, tenemos básicamente 2 formas de enviar correos:

- La función nativa `mail()`, que tiene bastantes limitaciones,
- o a través de un servidor SMTP.

La función `mail()` tiene que utilizar el servidor SMTP, que es una forma muy sencilla de enviar correo a través del servidor SMTP.
---------------

La idea de usar esto es simple: se llama a la función:

```php
mail('jan@barasek.com', 'Asunto', 'El texto del mensaje...');
```

Y PHP hará el envío por sí mismo.

Internamente, el envío funciona leyendo la configuración de `php.ini` y buscando el servidor SMTP por defecto para entregar el correo. Por lo tanto, requiere que se configure primero el servidor web.

El principal escollo de la función `mail()` es que el programador tiene que averiguar toda la lógica por sí mismo. Esto implica, por ejemplo, desechar las cabeceras sobre el cifrado, vincular los certificados para cifrar los mensajes, etc.

En caso de un fallo de envío, el valor de retorno es `false`, que tenemos que atrapar y procesar nosotros mismos. Podemos averiguar el error específico de forma limitada llamando a `error_get_last()`, así por ejemplo:

```php
if (@mail($to, $subject, $message) === false) {
	throw new \Exception(
		'No se puede enviar el correo:'
		. (@error_get_last()['mensaje'] ?? '')
	);
}
```

> **TIP:** Observe que no hemos especificado la dirección desde la que queremos enviar el correo ni la codificación a utilizar.
>
> Todos estos ajustes deben ser pasados a través de las cabeceras.

Si todavía necesita utilizar la función `mail()` (por ejemplo, debido al alojamiento), le recomiendo que utilice el paquete `nette/mail` y el servicio `SendmailMailer`, que maneja bien el envío de correo.

Servidor SMTP
-----------

SMTP son las siglas de `Simple Mail Transfer Protocol`, lo cual (como pronto verás) es muy cierto.

SMTP, a diferencia de `mail()`, es un protocolo más avanzado con opciones de configuración avanzadas no sólo desde el lado de PHP, sino también directamente en el servidor de correo.

El soporte de SMTP en los hosts es excelente en 2018.

El SMTP funciona básicamente cuando PHP establece primero una conexión con el servidor SMTP (requiere la extensión `php_openssl.dll` en PHP, que probablemente ya tengas activa), se autentifica (verificando la corrección de las credenciales de acceso) durante la conexión, y luego podemos comunicarnos con el servidor de forma similar a como lo hacemos con una base de datos, es decir, enviando peticiones individuales, pero manteniendo una única conexión en todo momento. Una gran ventaja de SMTP es el soporte directo para la encriptación (conocido como `TLS`).

Envío de correos electrónicos desde localhost - una solución sencilla
--------------------------------------------------

A menudo necesito enviar correos electrónicos desde localhost cuando estoy probando una aplicación recién escrita.

> **Para la propina:**
>
> En un Mac, la situación es simple porque el servidor MAMP de alguna manera "mágica" encuentra la cuenta de Apple Mail actualmente iniciada y los mensajes siempre se envían desde la cuenta actual.

Sin embargo, no siempre se puede confiar en este comportamiento y es una buena idea establecer su propia solución. Si tienes una conexión a Internet y una cuenta de Google, es muy fácil utilizar una cuenta de Gmail a la que puedes conectarte directamente desde PHP y enviar correos a través de ella.

Si utiliza el paquete `nette/mail`, la configuración es sencilla:

```neon
mail:
	smtp: true
	host: smtp.gmail.com
	username: janbarasek@gmail.com
	password: *********
	secure: ssl
```

> La contraseña no es la contraseña de inicio de sesión de su cuenta (eso sería inseguro y no podría utilizar la **autenticación de dos factores**, por ejemplo).
>
> Necesitas usar lo que se llama una "contraseña de aplicación", que implementativamente significa que <a href="https://myaccount.google.com/apppasswords">registra tu aplicación</a> directamente en tu cuenta de Google, a la que se le asigna alguna contraseña generada aleatoriamente que introduces en PHP y puede ser enviada.
>
> Las instrucciones detalladas están <a href="https://support.google.com/accounts/answer/185833?hl=cs">en el sitio web de Google</a>.

Configurar el correo en Wedos
---------------------------

Sólo se pueden enviar 500 correos electrónicos al día a través del alojamiento de Wedos, y tuve problemas con la conexión SMTP durante un tiempo.

A través del paquete `nette/mail` se hace así (una solución factible):

```neon
mail:
	smtp: true
	host: smtp-*******.wedos.net
	username: jan@barasek.com
	password: ******
	secure: tls
	port: 587
```

El parámetro `host` es diferente para cada alojamiento y se encuentra en el correo electrónico que Wedos envía cuando se registra el alojamiento.

El `nombre de usuario` representa el buzón desde el que se enviarán los correos. El buzón debe existir. Cuando enviamos un correo en PHP, también necesitamos establecer el envío a la misma dirección (en Nette con el método `->setFrom()`).

Si no rellenamos la configuración de forma precisa y correcta, se lanzarán varios mensajes de error y no se podrán enviar los correos.

Cuando se supere el número de mensajes enviados, se lanzará una excepción informando de la superación del límite.
