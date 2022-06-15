Obtener la dirección IP del usuario en PHP
==========================================

> id: '1d6d761e-c139-4624-8c32-b0f2c131a831'
> slug:
> 	cs: ip-adresa
> 	es: obtener-la-direccion-ip-del-usuario-en-php
> 
> perex:
> 	- 'Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem.'
> 	- 'Obtención de la dirección IP del usuario en PHP, almacenamiento de la dirección IP y prohibición del usuario. Investigar la VPN y el usuario detrás de Proxy o NAT.'
> 
> publicationDate: '2020-02-28 10:30:21'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '54a77b5e4cc431b354903e42a27682a6'

En PHP, es muy fácil detectar una dirección IP a nivel básico:

```php
echo 'Ya sabes, tu dirección IP es' . $_SERVER['REMOTE_ADDR'] . '?';
```

> **Atención:** Obtener la dirección IP como clave del campo `$_SERVER['REMOTE_ADDR']` sólo es posible si PHP fue llamado desde el navegador. En el modo CLI (por ejemplo, ejecutando desde el Terminal con cron), la dirección IP no está disponible (esto tiene sentido, ya que no se está realizando ninguna petición de red).

Descubrimiento fiable de la dirección IP
-----------------------------

Después de muchos años de desarrollo, finalmente me quedé con esta implementación:

```php
function getIp(): string
{
    if (isset($_SERVER['HTTP_CF_CONNECTING_IP'])) { // Soporte de Cloudflare
        $ip = $_SERVER['HTTP_CF_CONNECTING_IP'];
    } elseif (isset($_SERVER['REMOTE_ADDR']) === true) {
        $ip = $_SERVER['REMOTE_ADDR'];
        if (preg_match('/^(?:127|10)\.0\.0\.[12]?\d{1,2}$/', $ip)) {
            if (isset($_SERVER['HTTP_X_REAL_IP'])) {
                $ip = $_SERVER['HTTP_X_REAL_IP'];
            } elseif (isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
                $ip = $_SERVER['HTTP_X_FORWARDED_FOR'];
            }
        }
    } else {
        $ip = '127.0.0.1';
    }
    if (in_array($ip, ['::1', '0.0.0.0', 'localhost'], true)) {
        $ip = '127.0.0.1';
    }
    $filter = filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV4);
    if ($filter === false) {
        $ip = '127.0.0.1';
    }

    return $ip;
}
```

Mucho mejor, entonces:

```php
echo 'Ya sabes, tu dirección IP es' . getIp() . '?';
```

Si la IP puede ser detectada directamente, o es sólo IPv6, o está en modo CLI (por ejemplo, cron), devuelve `127.0.0.1` (localhost).

Las implementaciones que tienen en cuenta las cabeceras `X-Forwarded-For` y `X-Real-IP` son extremadamente peligrosas directamente en PHP, ya que los datos pueden ser fácilmente modificados y un atacante puede suplantar una dirección IP falsa para, por ejemplo, ver la administración o activar el modo Debug del sitio (Nette Tracy). Por otro lado, tenemos que aceptar algunas peticiones proxies (por ejemplo, cuando se proxenetiza el tráfico a través de Cloudflare, o cuando se ejecuta Apache y Ngnix en la misma máquina, cuando se llaman localmente uno tras otro).

En el caso del acceso directo del usuario al servidor, sólo hay una solución correcta, y es asegurar en Apache (a través de la extensión `RemoteIP`) y en Nginx a través de la extensión `remote_ip` que `X-Forwarded-For` se establezca a partir de la dirección IP real del visitante, y que la dirección IP no se pueda establecer con una cabecera HTTP.

El campo `$_SERVER['REMOTE_ADDR']` obtiene automáticamente la dirección IP correcta (es decir, la dirección IP desde la que la petición llegó directamente a PHP) y no tenemos que ocuparnos de ello.

Acceso de los usuarios a través de un proxy
----------------------------

A menudo ocurre que un usuario accede a través de un proxy. Entonces la dirección IP real se almacena en la variable `$_SERVER['HTTP_X_FORWARDED_FOR']`.

Este caso puede ocurrir, por ejemplo, cuando el enrutamiento en el servidor se resuelve usando el método `Ngnix -> Apache -> PHP`, donde `Ngnix` sirve como proxy inverso antes de `Apache`. En este caso, PHP sólo ve la dirección IP dentro de la red interna (normalmente de la forma `127.0.0.*`).

Por ejemplo, el servicio **Cloudflare** puede comportarse de esta manera, y hay que tener cuidado si estamos trabajando con la dirección IP del usuario real o del proxy. Para mí, la mejor manera es utilizar la función `getIp()` mencionada al principio de este artículo. Podemos asegurar la detección de Cloudflare verificando la existencia de la clave `$_SERVER['HTTP_CF_CONNECTING_IP']`, que se transmite automáticamente en cada petición proxy.

Almacenamiento de la dirección IP
------------------

Depende de la dirección IP que tengas disponible.

- La dirección IPv4 se puede almacenar en 4 bytes, para ello se utiliza la función `ip2long`,
- Sin embargo, para una dirección IPv6 tenemos que utilizar 16 bytes y no hay ninguna función de conversión.

Si su servidor de base de datos no admite directamente un tipo de datos para la dirección IP, le recomiendo que almacene la dirección IP como `varchar(39)`, donde ambas versiones caben como una cadena y son legibles.

> Al almacenar la dirección IP, considere si tiene sentido almacenar también el nombre de dominio detectado por la función `gethostbyaddr`. Al hacer el listado y la búsqueda, no se pueden averiguar los nombres porque se tarda mucho tiempo y pueden cambiar con el tiempo.

Bloqueo de la dirección IP del visitante
-----------------------------

La solución ideal es crear una lista de direcciones IP bloqueadas y comparar esta lista con la dirección IP actual en cada solicitud. Si las direcciones coinciden, la solicitud se detendrá inmediatamente.

```php
$blackList = [
    'primer-ip',
    'druha-ip',
];

if (\in_array(getIp(), $blackList, true) === true) {
    echo 'Lamentablemente tu dirección ip está bloqueada :-(';
    die; // Solicitud de salida
}
```

El ejemplo asume una implementación de la función `getIp()` como en el ejemplo anterior.

Una solución más potente es comprobar la aparición del índice en el array:

```php
$blackList = [
    'primer-ip' => true,
    'druha-ip' => true,
];

if (isset($blackList[getIp()]) === true) {
    echo 'Lamentablemente tu dirección ip está bloqueada :-(';
    die; // Solicitud de salida
}
```

Dirección IP y nombre del servidor
---------------------------------

La dirección IP del servidor suele estar almacenada en el campo `$_SERVER['SERVER_ADDR']` y su nombre puede obtenerse mediante la construcción `gethostbyaddr($_SERVER['SERVER_ADDR'])`.

Sin embargo, si se utiliza el concepto `Ngnix -> Apache -> PHP` y `Ngnix` está en el papel de proxy inverso, la dirección IP real del servidor no se muestra.

En este caso, el nombre del servidor puede encontrarse en el campo `$_SERVER['SERVER_NAME']`, o utilizando la función `php_uname('n')`. [Documentación oficial de la función uname](https://www.php.net/manual/en/function.php-uname.php).

Podemos entonces utilizar este truco para averiguar la dirección IP pública del servidor: `gethostbyname(php_uname('n'))`.
