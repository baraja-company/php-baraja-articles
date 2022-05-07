QR-Code-Generator - API
=======================

> id: a9b1804f-438b-49fe-86e7-5a1b30a07210
> slug:
> 	cs: api-qr-kod
> 	de: qr-code-generator---api
> 
> publicationDate: '2019-09-11 10:04:00'
> mainCategoryId: '8598ac8d-7a70-4e2a-be9a-9f4f9ecee935'
> sourceContentHash: '89824ae94a740439f4ad3920bd311d3d'

Ein QR-Code ist ein spezieller 2-dimensionaler Code, der zur Übermittlung kurzer Informationen, z.B. an ein Mobiltelefon, verwendet wird.

QR-Codes können einfach durch Einfügen eines Bildes auf den Servern von Google erstellt werden.

Zum Beispiel:

<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR-Code">

```html
<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR code">
```

Wir können 3 Parameter einstellen:

- Größe in "px" (vertikal und horizontal)
- Kodierung (ich empfehle `UTF-8`)
- Adresse (URL zur Seite, Telefonnummer, ...)

> TIPP: Wenn es eine mobile Version der Website gibt, verlinken Sie stattdessen auf diese.
>
> In den meisten Fällen wird Ihr QR-Code von Ihrem Mobiltelefon verarbeitet.

Es ist sehr einfach, eine eigene Funktion zum Einbetten zu schreiben:

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
