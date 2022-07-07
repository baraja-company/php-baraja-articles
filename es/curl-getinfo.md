Obtención de información de peticiones HTTP a través de cURL
============================================================

> id: '310e5143-6b38-44c1-a1bf-de37f94ee17d'
> slug:
> 	- null
> 	es: obtencion-de-informacion-de-peticiones-http-a-traves-de-curl
> 
> cs: curl-getinfo
> publicationDate: '2022-07-06 19:50:00'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '36748f0e9ced28da3da80155a0253140'

La función PHP `curl_getinfo()` proporciona información detallada sobre la petición cURL ejecutada. Este artículo explica el significado de cada campo.

Ejemplo de uso
---------------

Llama a la función sobre el resultado del contexto de `curl_init()`:

```php
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, 'https://baraja.cz');
curl_setopt($ch, CURLOPT_HEADER, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 0);
curl_setopt($ch, CURLOPT_NOBODY, 1);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 0);
curl_exec($ch);
$info = curl_getinfo($ch);
curl_close($ch);

dump($info);
```

Tabla de valores
--------------

La función `curl_getinfo()` devuelve un array asociativo del que se pueden recuperar claves y valores individuales.

| Clave, valor de ejemplo, explicación, etc.
|------|-----------------|------------|
| `url` | 'https://baraja.cz/' | URL descargada. |
| `content_type` | 'text/html; charset=utf-8' | Codificación y tipo de contenido utilizado (reclamado por el servidor de destino). |
| `http_code` | 200 | Código de estado HTTP devuelto. 200 significa OK. |
| Tamaño de la cabecera de la petición HTTP en bytes.
| `request_size` | 47 | Tamaño de la solicitud. |
| | `filetime` | -1 | Hora del archivo (reclamaciones del servidor). |
| `ssl_verify_result` | 0 | Comprobación SSL. |
| | `redirect_count` | 0 | Número de redirecciones antes de llegar al documento de destino.
| 0.233384 Tiempo total de espera de la respuesta. Dado en segundos. |
| 0.021608 Tiempo de resolución del dominio en los registros DNS. Se especifica en segundos. |
| | `connect_time` | 0.035031 | Tiempo para establecer una conexión con el servidor de destino. Se especifica en segundos. |
| | `pretransfer_time` | 0.187275 | Tiempo requerido para transferir los datos. Se especifica en segundos. |
| `upload_size` | 0.0 | Tamaño de los datos cargados en bytes. |
| `size_download` | 0.0 | Tamaño de los datos descargados en bytes. |
| 0.0 | Velocidad de descarga en bytes por segundo.
| 0.0 | Velocidad de carga en bytes por segundo.
| `download_content_length` | 15522.0 | Tamaño de los datos descargados en bytes. |
| `upload_content_length` | -1.0 | Tamaño de los datos subidos en bytes. |
| `starttransfer_time` | 0.233354 | Indica el valor de TTFB (Time To First Byte) en segundos. |
| 0.0 | Tiempo de redirección para descargar el contenido canónico.
| | `redirect_url` | `''` | URL canónica y destino de la redirección. |
| `primary_ip` | '76.76.21.21' | Desde qué IP se descargó el contenido. |
| `certinfo` | array (0) | Más detalles sobre el certificado del sitio de destino. |
| El puerto de red utilizado (80 significa HTTP, 443 significa HTTPS).
| `local_ip` | '192.168.0.186' | Dirección IP local de la máquina que envió la solicitud. |
| `puerto_local` | 56568 | Puerto de la máquina local desde la que se envió la solicitud. |
| `http_version` | 3 | Versión del protocolo HTTP. |
| | `protocolo` | 2 | Código del protocolo utilizado. |
| `ssl_verifyresult` | 0 | Resultado de la verificación SSL. |
| `scheme` | 'HTTPS' | Protocolo al principio de la URL. |
| `appconnect_time_us` | 186220 | Tiempo de conexión con el servidor de destino. Se especifica en nanosegundos. |
| | `connect_time_us` | 35031 | Tiempo de conexión con el servidor de destino. Se especifica en nanosegundos. | |
| | `namelookup_time_us` | 21608 | Tiempo necesario para reescribir el dominio a través de los registros DNS. Se especifica en nanosegundos. |
| | `pretransfer_time_us` | 187275 | Tiempo de transferencia de datos. Se especifica en nanosegundos. |
| `redirect_time_us` | 0 | Tiempo de redirección para descargar el contenido canónico. Dado en nanosegundos. |
| `starttransfer_time_us` | 233354 | Indica el valor del tiempo TTFB (Time To First Byte). En nanosegundos. |
| | `total_time_us` | 233384 | Tiempo total de espera de una respuesta. Se especifica en nanosegundos. |

Es posible que algunas llaves no estén siempre disponibles. Verifique siempre la existencia de la clave y la validez del valor antes de leer el valor.
