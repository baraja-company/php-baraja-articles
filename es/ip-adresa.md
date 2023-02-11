Obtener la dirección IP del usuario en PHP
==========================================

> id: '1d6d761e-c139-4624-8c32-b0f2c131a831'
> slug:
> 	cs: ip-adresa
> 	es: obtener-la-direccion-ip-del-usuario-en-php
> 
> perex:
> 	- 'Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem.'
> 	- 'Obtener la dirección IP del usuario en PHP, almacenar la dirección IP y banear al usuario. Investigación de VPN y usuario detrás de Proxy o NAT.'
> 
> publicationDate: '2020-02-28 10:30:21'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '284c4642c3fa98a026ce5a9e6625bb16'

En PHP, es muy fácil detectar una dirección IP a un nivel básico:

```php
echo 'Ya sabes, tu dirección IP es' . $_SERVER['DIRECCIÓN REMOTA'] . '?';
```

> **Atención:** Obtener la dirección IP como la clave del campo `$_SERVER['REMOTE_ADDR']` sólo es posible si PHP fue llamado desde el navegador. En modo CLI (por ejemplo, ejecutando desde Terminal con cron), la dirección IP no está disponible (esto tiene sentido, ya que no se está realizando ninguna petición de red).

Detección fiable de direcciones IP
-----------------------------

Tras muchos años de desarrollo, finalmente me quedé con esta implementación:

```php
function getIp(): string
{
    if (isset($_SERVER['HTTP_CF_CONNECTING_IP'])) { // Soporte de Cloudflare
        $ip = $_SERVER['HTTP_CF_CONNECTING_IP'];
    } elseif (isset($_SERVER['DIRECCIÓN REMOTA']) === true) {
        $ip = $_SERVER['DIRECCIÓN REMOTA'];
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

Si la IP puede detectarse directamente, o es sólo IPv6, o está en modo CLI (por ejemplo, cron), devuelve `127.0.0.1` (localhost).

Las implementaciones que tienen en cuenta las cabeceras `X-Forwarded-For` y `X-Real-IP` son extremadamente peligrosas directamente en PHP, ya que los datos pueden modificarse fácilmente y un atacante puede suplantar una dirección IP falsa para, por ejemplo, ver la administración o activar el modo Debug del sitio (Nette Tracy). Por otro lado, tenemos que aceptar algunas peticiones proxy (por ejemplo, cuando se proxy el tráfico a través de Cloudflare, o cuando se ejecuta Apache y Ngnix en la misma máquina, cuando se llaman localmente justo después de la otra).

En el caso de acceso directo del usuario al servidor, sólo hay una solución correcta, y es asegurarse en Apache (mediante la extensión `RemoteIP`) y en Nginx mediante la extensión `remote_ip` de que `X-Forwarded-For` se establece a partir de la dirección IP real del visitante, y que la dirección IP no puede establecerse con una cabecera HTTP.

El campo `$_SERVER['REMOTE_ADDR']` obtiene automáticamente la dirección IP correcta (es decir, la dirección IP desde la que la petición llegó directamente a PHP) y no tenemos que ocuparnos de ello.

Acceso de usuarios a través de proxy
----------------------------

A menudo ocurre que un usuario accede a través de un proxy. A continuación, la dirección IP real se almacena en la variable `$_SERVER['HTTP_X_FORWARDED_FOR']`.

Este caso puede ocurrir, por ejemplo, cuando el enrutamiento en el servidor se resuelve usando el método `Ngnix -> Apache -> PHP`, donde `Ngnix` sirve como proxy inverso antes que `Apache`. En este caso, PHP sólo ve la dirección IP dentro de la red interna (normalmente de la forma `127.0.0.*`).

Por ejemplo, el servicio **Cloudflare** puede comportarse de esta manera, y hay que tener cuidado si estamos trabajando con la dirección IP del usuario real o del proxy. Para mí, la mejor manera es utilizar la función `getIp()` mencionada al principio de este artículo. Podemos asegurar la detección de Cloudflare verificando la existencia de la clave `$_SERVER['HTTP_CF_CONNECTING_IP']`, que se transmite automáticamente en cada petición proxy.

Detección de VPN / Proxy
-------------------

No existe una detección fiable del uso de proxy o VPN, pero en un entorno real podemos filtrar al menos parte del tráfico.

Hay varias formas de hacerlo: Tome un rango de direcciones IP y compare la dirección IP de la que procede la solicitud.

De algunos proveedores VPN, las listas de direcciones IP están disponibles extraoficialmente (ver por ejemplo https://gist.github.com/JamoCA/eedaf4f7cce1cb0aeb5c1039af35f0b7), en el caso de los nodos de salida Tor oficialmente (https://blog.torproject.org/changes-tor-exit-list-service, pero los puentes Tor no están allí).

Otra opción es hacer una solicitud en línea en algún sitio, lo que puede tanto retrasar la carga de la página si el servicio no funciona como "filtrar" las direcciones IP de los visitantes a un tercero. A partir de 2023, desaconsejaría encarecidamente este enfoque, ya que empieza a tratarse más de proteger y manipular los datos de los usuarios.

Esta consulta en línea puede ser "ingenua" y sólo necesita ver quién es el propietario del rango o si se trata de un proxy/VPN (algunos servicios pueden devolver esto, pero por defecto no forma parte de la "información IP", por ejemplo, de un servicio whois).

(La mayoría) de las veces se utiliza algún tipo de clasificación de reputación, en la que algunas direcciones IP "ensucian" más que otras. Estadísticamente, hay más basura proveniente de varios proxies, VPNs y Tor que de direcciones IP domésticas (excepto quizás direcciones IP domésticas "infectadas"). Esta evaluación de la reputación la ofrecen algunas listas de bloqueo de DNS, véase alguna lista aleatoria, https://en.m.wikipedia.org/wiki/Comparison_of_DNS_blacklists y la columna "Listing goal", o la ofrecen directamente empresas como Cloudflare en forma de "gestión de bots", etc.

Depende mucho de cuál sea el objetivo de la detección.

Almacenamiento de direcciones IP
------------------

Depende de la dirección IP que tengas disponible.

- La dirección IPv4 se puede almacenar en 4 bytes, para ello se utiliza la función `ip2long`,
- Sin embargo, para una dirección IP IPv6 tenemos que utilizar 16 bytes y no hay función de conversión.

Si su servidor de base de datos no soporta directamente un tipo de datos para la dirección IP, recomiendo almacenar la dirección IP como `varchar(39)`, donde ambas versiones caben como una cadena y son legibles.

> Cuando almacene la dirección IP, considere si tiene sentido almacenar también el nombre de dominio detectado por la función `gethostbyaddr`. No se pueden averiguar los nombres al listar y buscar, porque lleva mucho tiempo y pueden cambiar con el tiempo.

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

Una solución más potente es comprobar la aparición del índice en la matriz:

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

La dirección IP del servidor suele almacenarse en el campo `$_SERVER['SERVER_ADDR']` y su nombre puede obtenerse mediante la construcción `gethostbyaddr($_SERVER['SERVER_ADDR'])`.

Sin embargo, si se utiliza el concepto `Ngnix -> Apache -> PHP` y `Ngnix` tiene el papel de proxy inverso, no se muestra la dirección IP real del servidor.

En este caso, el nombre del servidor puede encontrarse en el campo `$_SERVER['SERVER_NAME']`, o utilizando la función `php_uname('n')`. [Documentación oficial de la función uname](https://www.php.net/manual/en/function.php-uname.php).

A continuación, podemos utilizar este truco para averiguar la dirección IP pública del servidor: `gethostbyname(php_uname('n'))`.
