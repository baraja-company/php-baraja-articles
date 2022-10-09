QR-kodegenerator - API
======================

> id: a9b1804f-438b-49fe-86e7-5a1b30a07210
> slug:
> 	cs: api-qr-kod
> 	da: qr-kodegenerator-api
> 
> publicationDate: '2019-09-11 10:04:00'
> mainCategoryId: '8598ac8d-7a70-4e2a-be9a-9f4f9ecee935'
> sourceContentHash: '89824ae94a740439f4ad3920bd311d3d'

En QR-kode er en særlig todimensional kode, der bruges til at overføre korte oplysninger, f.eks. til en mobiltelefon.

QR-koder kan nemt genereres ved blot at indsætte et billede på Googles servere.

For eksempel:

<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR-kode">

```html
<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR code">
```

Vi kan indstille 3 parametre:

- Størrelse i `px` (lodret og vandret)
- Kodning (jeg anbefaler `UTF-8`)
- Adresse (URL til siden, telefonnummer, ...)

> TIP: Hvis der findes en mobilversion af webstedet, skal du i stedet linke til den.
>
> I de fleste tilfælde vil din mobiltelefon behandle din QR-kode.

Vi kan meget nemt skrive vores egen funktion til indlejring:

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
