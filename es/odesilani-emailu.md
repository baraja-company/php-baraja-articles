Función PHP mail()
==================

> id: '6536e2e2-38e1-4496-80ef-6c1c9a0b5be5'
> slug:
> 	cs: odesilani-emailu
> 	es: funcion-php-mail
> 
> publicationDate: '2019-09-16 08:51:41'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: ca90a7366754a7e7a0ce43f4252296f8

La función `mail()` envía un mensaje de correo electrónico a través de la configuración por defecto del servidor. Para que funcione correctamente, es necesario activar la función en el servidor y configurar el servidor de correo para el envío.

**La función es sólo de envío. Hay que ordenar la recepción de mensajes a nivel de servidor de correo. Por ejemplo, descargar regularmente mensajes mediante el protocolo `IMAP` o `POP3`.

> Desaconsejo encarecidamente el uso de la función por el momento, porque el programador tiene que ocuparse de todo él mismo (por ejemplo, enviar las cabeceras correctas, o establecer la codificación).
>
> Mucho mejor es conectarse a través de <a href="/send-email-smtp">servidor SMTP</a>.

Utilizando
-------

```php
mail ('jan@barasek.com', 'Asunto', 'Texto del correo electrónico...');
```

El primer parámetro es la dirección del destinatario, el segundo el asunto y el tercero el texto del mensaje. El cuarto parámetro (opcional) indica la configuración adicional del mensaje.
