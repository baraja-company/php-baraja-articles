Cómo configurar un certificado HTTPS / SSL - guía completa
==========================================================

> id: '0346a091-2b2c-494f-b2fb-573796d4fb46'
> slug:
> 	cs: jak-nastavit-https-ssl-certifikat
> 	es: como-configurar-un-certificado-https-ssl-guia-completa
> 
> publicationDate: '2019-09-16 09:01:35'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '67b2b3ccce4815204a4bcadce781fa59'

Al desplegar el protocolo `https` en los sitios de los clientes, a menudo me encontraba con diversas dificultades derivadas de la falta de comprensión de los problemas y de la excesiva complejidad de los conceptos.

En este tutorial describo en detalle los pasos para obtener y desplegar un certificado válido en un servidor web.

**En cada subtítulo, siempre resumo brevemente el paso para los usuarios avanzados, y al final comento los detalles para los principiantes.

> **Atención:** El proceso completo de despliegue de un certificado puede durar más de una hora y a menudo es intermitente (el sitio puede no estar disponible).

Requisitos de entrada
-----------------

Las instrucciones suponen que tenemos acceso a un servidor web de Terminal que funciona en Linux y que utiliza Apache.

Para Nginx toda la teoría se aplica igualmente, solo que la vinculación del archivo del certificado es diferente.

## Conectando con el servidor web

Nos conectamos al servidor a través de SSH.

- En Windows recomiendo el programa **Putty**,
- En Mac o Linux, basta con utilizar el Terminal incorporado.

En Mac o Linux, llame al comando

```htaccess
ssh uživatel@server
```

Por ejemplo, quiero conectarme con el usuario `root` en el sitio web `baraja.cz`:

```htaccess
ssh root@baraja.cz
```

O al usuario `jan` a una dirección IP específica:

```htaccess
ssh jan@127.0.0.1
```

Tras enviar su consulta, la conexión se realizará directamente o se le pedirá una contraseña. No se muestra nada al escribir la contraseña, así que confirme la contraseña con la tecla de entrada y espere a que se autorice la conexión.

> **Atención:** Si no tenemos derechos sobre una acción, debemos asignarlos. O bien cambiamos directamente al usuario `root` con el comando `sudo su`, o precedemos el comando que queremos ejecutar bajo root con la palabra `root` al principio, por ejemplo `root rm <nombre>` bajo root borrará el archivo `<nombre>`. Al utilizar el comando `sudo`, es posible que periódicamente se nos pida una contraseña.

**Detalles:**

El acceso SSH es configurado por el hosting particular donde has alquilado un servidor.

- En el caso de los VPS siempre tendrás acceso SSH.
- En el caso del alojamiento, es posible que no tengas SSH en absoluto, y la configuración se hace de forma diferente (normalmente a través de la interfaz web, o contactando con el soporte).

## Cambio de HTTP a HTTPS

Si está convirtiendo un sitio existente de `http` a `https`, debe garantizar que todo el tráfico se redirige al nuevo protocolo `https`.

En el caso de Apache, esto se puede conseguir fácilmente utilizando una redirección en el archivo `.htaccess`:

```htaccess
<IfModule mod_rewrite.c>
	RewriteEngine On

	RewriteCond %{HTTPS} !on
	RewriteRule .? https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```

Esta configuración asegurará que todas las peticiones a `http` sean redirigidas con `código HTTP 301` a `https`. Esta configuración es la predeterminada para el marco Nette, pero se aplica también a todos los demás casos.

**Detalles:**

El archivo `.htaccess` contiene la configuración específica del servidor web que afecta a cada petición. Suele colocarse en el mismo directorio que `index.php`, u otros archivos accesibles desde Internet.

Su configuración sólo es válida para el servidor Apache y puede estar desactivada o restringida para algunos hosts. Para obtener información más detallada, póngase siempre en contacto con el alojamiento donde aloja su sitio.

-----

A veces sucede que `.htaccess` no quiere cooperar bien y la configuración es extremadamente difícil (por ejemplo, para muchos dominios, cada uno de los cuales se comporta de manera diferente). En tal caso, la redirección a HTTPS puede ser manejada directamente en PHP en un apuro:

```php
if (empty($_SERVER['HTTPS']) || $_SERVER['HTTPS'] === 'fuera de') {
	$location = 'https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'];
	header('HTTP/1.1 301 Movido permanentemente');
	header('Ubicación:' . preg_replace('/^(https:\/\/(?:www\.)(.*)$/', '$1$2', $location));
	die;
}
```

Poner el script en `index.php`. Sin embargo, no recomiendo esta solución.

## Encontrar archivos con hosts virtuales

En el caso de Apache, necesitamos encontrar el archivo de hosts virtuales.

Normalmente se encuentran en la ruta `/etc/apache2/sites-available`.

Listar el contenido del directorio con el comando `ls -al` y encontrar el archivo donde se configura el virtual para nuestro sitio.

Los VirtualHosts suelen estar ubicados en archivos con la extensión `.conf`. El valor por defecto suele estar en `000-default.conf`.

**Detalles:**

- El directorio se abre con el comando `cd`, por ejemplo `cd /etc/apache2/sites-available`.
- El contenido del archivo se escribe en el Terminal utilizando el comando `cat <nombre>`, o se edita utilizando los comandos `nano <nombre>` o `vim <nombre>`.
- Normalmente se accede al editor utilizando el atajo de teclado `CTRL + X` o pulsando dos veces `ESC`.
- Los archivos pueden ser parcialmente explorados en la GUI con Midnight commander (comando `mc`), que en el caso de Ubuntu se instala simplemente con `apt-get install mc` o `sudo apt-get install mc`.

## Configurar un host virtual para el puerto 443

Dentro del host virtual necesitamos preparar uno nuevo para el puerto `443` (un problema común puede ser el bloqueo del puerto 443 en el firewall).

El contenido del archivo podría ser, por ejemplo, el siguiente

```htaccess
<IfModule mod_ssl.c>
     <VirtualHost *:443>
         ServerName baraja.cz

         ServerAdmin jan@barasek.com
         DocumentRoot /var/www/baraja.cz/www
         Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains; preload"
         ErrorLog ${APACHE_LOG_DIR}/baraja.cz.ssl.error.log
         CustomLog ${APACHE_LOG_DIR}/baraja.cz.ssl.access.log combined
         SSLEngine on
         SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
         SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
         SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
         SSLProtocol             all -SSLv3
         SSLCipherSuite          ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS
         SSLHonorCipherOrder     on
         SSLCompression          off
         <FilesMatch "\.(cgi|shtml|phtml|php)$">
                 SSLOptions +StdEnvVars
         </FilesMatch>
         <Directory /usr/lib/cgi-bin>
                 SSLOptions +StdEnvVars
         </Directory>
		 BrowserMatch "MSIE [2-6]" nokeepalive ssl-unclean-shutdown downgrade-1.0 force-response-1.0
         # MSIE 7 and newer should be able to use keepalive
         BrowserMatch "MSIE [17-9]" \
         ssl-unclean-shutdown
     </VirtualHost>
</IfModule>
```

En el archivo está el área más importante:

```htaccess
SSLEngine on
SSLCertificateFile      /etc/ssl/baraja.cz/baraja.cz.crt
SSLCertificateKeyFile   /etc/ssl/baraja.cz/baraja.cz.key
SSLCertificateChainFile /etc/ssl/baraja.cz/rapidssl.crt
```

Entender esta configuración es absolutamente crítico. Si necesitas buscar información más detallada en Google, utiliza las palabras `SSLCertificateFile`, `SSLCertificateKeyFile` y `SSLCertificateChainFile` que son comunes a todos los servidores Apache.

> **Atención:** ¡El tema del SSL es relativamente antiguo y a menudo se utilizan diferentes nombres para la misma cosa para mantener la compatibilidad hacia atrás! Por lo tanto, es importante entender el principio.

**Detalles:**

Es importante que VirualHost contenga:

- `<IfModule mod_ssl.c>` dice que queremos usar SSL
- `<VirtualHost *:443>` que toda la comunicación se ejecutará en el puerto 443
- `SSLEngine on` que SSL está habilitado para este VirtualHost
- Los archivos `SSLCertificateFile`, `SSLCertificateKeyFile` y `SSLCertificateChainFile` son archivos con claves específicas.

No se necesita nada más.

## Entender el principio de lo que vamos a hacer y cómo funciona el certificado

En la configuración final de VirtualHost, realmente sólo necesitaremos 3 archivos `SSLCertificateFile`, `SSLCertificateKeyFile` y `SSLCertificateChainFile`, que podemos colocar en cualquier lugar del servidor (el nombre y la ubicación no importan). Es importante proporcionar una ruta de trabajo y que el contenido de los archivos sea válido.

El método específico de obtención de los certificados puede variar de una CA a otra. Lo importante es entender el principio y luego aplicarlo a su caso.

| Archivo | Significado |
|---------------------------|-------------------------------------|
| `SSLCertificateFile` | Este certificado **es enviado por la autoridad** |
| `SSLCertificateKeyFile` | **Mi clave privada generada** |
| `SSLCertificateChainFile` | He descargado de la web tipo **intermedio + root** |

## Obtener la clave privada y solicitar el certificado

En el servidor, primero generamos la clave privada. La palabra **privada** es importante, significa que nadie más la conoce excepto el servidor web. Lo ideal es que no salga nunca del servidor y se coloque en un lugar seguro. La pérdida de esta clave supone una pérdida de seguridad, ya que un atacante podrá hacerse pasar por un servidor concreto.

Para generar la clave, utilice el comando

```shell
openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.key -out yourdomain.csr
```

Para generarlo, necesitamos tener el programa `openssl` instalado en el servidor, que podemos obtener por ejemplo ejecutando `sudo apt install openssl`.

Puede haber varios tipos de claves, en este caso generamos una clave RSA de 2048 bytes (`rsa:2048`).

La salida del comando son 2 archivos (que usted nombra según su dominio):

- `sudominio.clave` - es la clave privada. Guarde la ruta de esta clave en Apache VirtualHost en `SSLCertificateKeyFile`.
- `sudominio.csr` - se trata de una `solicitud de certificado`, o una solicitud para emitir un certificado.

El contenido de la solicitud debe presentarse siempre a la AC para su aprobación. Esto suele hacerse a través de la interfaz web en la administración del sitio donde se venden los certificados. La aprobación de la solicitud varía en función del tipo de certificado. La mayoría de las veces la realiza un robot de forma automática y tarda entre 5 minutos y 8 horas. En el caso de los certificados caros, en los que también se verifica la propiedad física del sitio y de la empresa operadora, la verificación se hace manualmente y puede llevar varios días.

Si tiene prisa, existe una agencia de certificación `Let's encrypt` que autoriza las solicitudes automáticamente con una validez de 3 meses.

> **Nota:** En algunos casos, la CA ofrece la generación automática de solicitudes. Esto no es una práctica recomendada porque conoce la clave privada. Si lo hace, debe obtener siempre la clave privada de la agencia y colocarla en un archivo del servidor de la misma manera que si generara la solicitud.

## Obtención de una clave para una CA/ autoridad de seguridad específica

Mientras tanto, a la espera de que se apruebe la solicitud y se emita el certificado, aseguraremos la **clave pública** de la CA. En Apache VirtualHost esto está representado por el archivo `SSLCertificateChainFile` (en inglés se llama `Intermediate and Root CA`). Esta clave es, en principio, pública e indica al navegador web quién es la CA. Este archivo suele descargarse del sitio web de la AC o se nos entrega en un correo electrónico.

Siempre debes descargar la clave correcta según el algoritmo que elijas. En el caso de RapidSSL, por ejemplo, se puede descargar aquí: https://knowledge.digicert.com/generalinformation/INFO1548.html

## Obtener el certificado

Basándose en la solicitud, la autoridad de certificación nos ha emitido un certificado. En el caso de Apache VirtualHost, está representado por el archivo `SSLCertificateFile`.

Es importante destacar que el certificado tiene una validez limitada y hay que repetir este proceso (idealmente antes de que caduque). Puede encontrar fácilmente la fecha de caducidad en Chrome haciendo clic en el candado verde del navegador y viendo los detalles del certificado, en caso de que lo comunique la CA.

Después de la fecha de caducidad, el certificado no es válido y el sitio se negará a conectarse y los usuarios recibirán un mensaje de error sobre el fallo de seguridad.

## Comprobar y hacer cambios

La corrección de la configuración de Apache puede verificarse parcialmente con el comando `apache2ctl -S`.

Para que los cambios surtan efecto, hay que reiniciar Apache, por ejemplo con el comando

```shell
sudo service apache2 restart
```

Si no se lanza ningún mensaje de error, pasamos inmediatamente a verificar el funcionamiento a través de un navegador web (recomiendo Google Chrome, que muestra la mayor cantidad de detalles).

En caso de error, llamamos al comando `apache2ctl -S`, que puede mostrar la ruta a los registros, donde podemos ver aproximadamente lo que está mal. Es importante realizar todos los pasos de este manual en el orden correcto y no intercambiar ninguna de las claves.

## Apoyo futuro

No olvide marcar la fecha de caducidad en su calendario para poder renovar su certificado a tiempo. Algunas CAs envían una notificación por correo electrónico, pero esto no siempre es fiable.

Puedes hacer el proceso de renovación y obtener un nuevo certificado en paralelo mientras el actual está en funcionamiento, y luego simplemente reemplazarlo al mismo tiempo.

Para la renovación automática, recomiendo [Certbot](https://certbot.eff.org), que puede supervisar automáticamente la validez y renovar los certificados. La renovación se realiza, por ejemplo, mediante un cron que llama a un comando una vez al mes por la noche para emitir un nuevo certificado y lo despliega inmediatamente.

Si no entiende la gestión de certificados, es una buena idea alojarse con una empresa establecida que le proporcione un alojamiento funcional, incluyendo un certificado. Esto le ahorrará muchos problemas.
