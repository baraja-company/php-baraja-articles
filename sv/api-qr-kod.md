QR-kodgenerator - API
=====================

> id: a9b1804f-438b-49fe-86e7-5a1b30a07210
> slug:
> 	cs: api-qr-kod
> 	sv: qr-kodgenerator---api
> 
> publicationDate: '2019-09-11 10:04:00'
> mainCategoryId: '8598ac8d-7a70-4e2a-be9a-9f4f9ecee935'
> sourceContentHash: '89824ae94a740439f4ad3920bd311d3d'

En QR-kod är en speciell tvådimensionell kod som används för att överföra kort information, t.ex. till en mobiltelefon.

QR-koder kan enkelt genereras genom att helt enkelt lägga in en bild på Googles servrar.

Till exempel:

<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR-kod">

```html
<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR code">
```

Vi kan ställa in 3 parametrar:

- Storlek i `px` (vertikalt och horisontellt)
- Kodning (jag rekommenderar `UTF-8`)
- Adress (URL till sidan, telefonnummer, ...)

> TIPS: Om det finns en mobilversion av webbplatsen kan du länka till den i stället.
>
> I de flesta fall behandlas QR-koden av din mobiltelefon.

Det är mycket enkelt att skriva en egen funktion för inbäddning:

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
