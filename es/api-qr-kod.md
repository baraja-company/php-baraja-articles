Generador de códigos QR - API
=============================

> id: a9b1804f-438b-49fe-86e7-5a1b30a07210
> slug:
> 	cs: api-qr-kod
> 	es: generador-de-codigos-qr-api
> 
> publicationDate: '2019-09-11 10:04:00'
> mainCategoryId: '8598ac8d-7a70-4e2a-be9a-9f4f9ecee935'
> sourceContentHash: '89824ae94a740439f4ad3920bd311d3d'

Un código QR es un código especial bidimensional que se utiliza para transmitir información breve, por ejemplo, a un teléfono móvil.

Los códigos QR se pueden generar fácilmente con sólo insertar una imagen en los servidores de Google.

Por ejemplo:

<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="Código QR">

```html
<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR code">
```

Podemos establecer 3 parámetros:

- Tamaño en `px` (vertical y horizontal)
- Codificación (recomiendo `UTF-8`)
- Dirección (URL de la página, número de teléfono, ...)

> SUGERENCIA: Si hay una versión móvil del sitio, enlace a ella.
>
> En la mayoría de los casos, su código QR será procesado por su teléfono móvil.

Es muy fácil escribir su propia función para incrustar:

```php
function getQrCode(string $url, int $size = 128, string $charset = 'UTF-8'): string
{
    $size = $size < 16 ? 16 : ($size > 2048 ? 2048 : $size);

    return '<img src="https://chart.apis.google.com/chart?cht=qr&chs='
       . $size . 'x' . $size
       . '&choe=' . urlencode($charset)
       . '&chld=H%7C0&chl=' . urlencode($url)
       . '">';
}

echo getQrCode('https://php.baraja.cz');
```
