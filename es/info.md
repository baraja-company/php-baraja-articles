Información de configuración de PHP y del servidor (phpinfo(), php.ini)
=======================================================================

> id: '6f4a78c1-ca06-4619-9a28-a716a65a12a8'
> slug:
> 	cs: info
> 	es: informacion-de-configuracion-de-php-y-del-servidor-phpinfo-php.ini
> 
> perex:
> 	- 'PHP info, informace o nastavení a konfiguraci webového serveru. Změna konfigurace přes php.ini'
> 	- 'PHP info, información sobre la instalación y configuración del servidor web. Cambiar la configuración a través de php.ini'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '9ad0961571b298fb1f30ea3e67b2973e'

A menudo necesitamos averiguar toda la información posible sobre el servidor, la función nativa `phpinfo()` es genial para esto:

```php
phpinfo();

die;	// después de escribir la configuración, salir del script
```

Esto hace que sea fácil ver la versión instalada, las extensiones, las bibliotecas y mucho más.

> Consulte el final de este artículo para obtener información sobre la configuración y el cambio de ajustes.

Encontrar una sección de configuración específica
-------------------------------------

A veces es útil listar sólo información específica, por lo que podemos establecer el primer parámetro para especificar exactamente lo que nos interesa:

```php
phpinfo(INFO_MODULES);
```

Se utilizan constantes predefinidas para el ajuste:

| Nombre de la constante Valor Descripción
|-------------------|-----------|------
| INFO_GENERAL | 1 | Configuración general, ubicación de php.ini, fecha de la última actualización, servidor web, información del sistema y más.
| INFO_CREDITS | 2 | Créditos PHP, para más ver `phpcredits()`.
| 4. Directivas de ubicación y configuración actuales. La función `ini_get()` proporcionará más información.
| Información sobre los módulos instalados. Para más información, consulte la función `get_loaded_extensions()`.
| INFO_ENVIRONMENT | 16 | Información sobre la variable `Environment`, disponible como `$_ENV`.
| INFO_VARIABLES | 32 | Un resumen de la configuración de las variables superglobales, conocidas como `EGPCS` (Environment, GET, POST, Cookie, Server).
| INFO_LICENSE | 64 | Información sobre la licencia de uso de PHP y otras condiciones de uso.
| INFO_ALL | -1 | Mostrar toda la información (valor por defecto)

Variable superglobal `$_SERVER`.
---------------------------------

Podemos averiguar bastante información sobre la configuración del servidor directamente mientras se ejecuta el script (por ejemplo, el correo electrónico del administrador de la web, la dirección IP actual del visitante o la URL llamada actualmente).

Listar todos los valores existentes es fácil:

```php
foreach ($_SERVER as $key => $value) {
    echo $key . ':' . $value . '<br>';
}
```

> **Atención:** No es necesario que existan todos los índices (por ejemplo, si el script ejecuta cron en modo CLI, el índice con la URL de la página o la dirección IP de la petición no existirá).

Lectura de directivas de configuración específicas
-----------------------------------------

Gran parte de la configuración se almacena en `php.ini` y no es accesible directamente desde PHP de forma normal. Por ejemplo, el tamaño máximo de los archivos que se pueden subir.

Para leer la configuración directamente, utilice la función `ini_get()` (nota: esta función puede no estar habilitada en todos los servidores, esto es especialmente cierto para los hosts).

Por ejemplo, si queremos averiguar el tamaño máximo de archivo que podemos subir, tenemos que escribir nuestra propia implementación:

```php
/**
 * @autor Jan Barášek
 */
public static function getMaxUploadFileSize(): int
{
    $maxUpload = min(
        ini_get('tamaño_máximo_de_post'),
        ini_get('upload_max_filesize')
    );

    if (strncmp($maxUpload, 'M', 1) === 0) {
        return (int) str_replace('M', '', $maxUpload);
    }

    return (int) $maxUpload;
}
```

Devuelve el valor máximo de `upload_max_filesize` y `post_max_size` en MB.

Configurar el servidor y cambiar los ajustes
-------------------------------------

Los ajustes se almacenan en el archivo `php.ini`. Su ubicación se puede encontrar fácilmente utilizando la función `phpinfo()` o llamando al comando `php --ini`.

```shell
> php --ini

Configuration File (php.ini) Path: /etc/php/7.1/cli
Loaded Configuration File:         /etc/php/7.1/cli/php.ini
Scan for additional .ini files in: /etc/php/7.1/cli/conf.d
Additional .ini files parsed:      /etc/php/7.1/cli/conf.d/10-mysqlnd.ini,
/etc/php/7.1/cli/conf.d/10-opcache.ini,
/etc/php/7.1/cli/conf.d/10-pdo.ini,
/etc/php/7.1/cli/conf.d/20-calendar.ini,
/etc/php/7.1/cli/conf.d/20-ctype.ini,
/etc/php/7.1/cli/conf.d/20-exif.ini,
/etc/php/7.1/cli/conf.d/20-fileinfo.ini,
/etc/php/7.1/cli/conf.d/20-ftp.ini,
/etc/php/7.1/cli/conf.d/20-gd.ini,
/etc/php/7.1/cli/conf.d/20-gettext.ini
```

Alternativamente, la ruta puede ser analizada inteligentemente (funciona en sistemas Linux):

```shell
php -r "phpinfo();" | grep php.ini
```

Volverá:

```shell
Configuration File (php.ini) Path => /etc/php/7.1/cli
Loaded Configuration File => /etc/php/7.1/cli/php.ini
```

> **Importante:** Normalmente la configuración se divide en varios archivos según el entorno y los paquetes, donde `php.ini` es válido globalmente para todos, mientras que por ejemplo la configuración `CLI` es válida sólo para el modo CLI, es decir, para llamar a un cron o a un comando desde el Terminal.

Configurar el límite de tamaño de los archivos subidos
----------------------------------------------

Un ejemplo de una propiedad que suele configurarse directamente en `php.ini` es el tamaño máximo del archivo de subida (por defecto es de `2 MB`, que ya es poco en 2018).

Dentro del archivo de configuración esto se escribe de la siguiente manera, por ejemplo:

```shell
; Maximum allowed size for uploaded files.
upload_max_filesize = 40M

; Must be greater than or equal to upload_max_filesize
post_max_size = 40M
```

*Punto medio significa comentario, seguido de directivas de configuración específicas.*
