Generátor QR kódov - API
========================

> id: a9b1804f-438b-49fe-86e7-5a1b30a07210
> slug:
> 	cs: api-qr-kod
> 	sk: generator-qr-kodov---api
> 
> publicationDate: '2019-09-11 10:04:00'
> mainCategoryId: '8598ac8d-7a70-4e2a-be9a-9f4f9ecee935'
> sourceContentHash: '89824ae94a740439f4ad3920bd311d3d'

Kód QR je špeciálny dvojrozmerný kód, ktorý sa používa na prenos krátkych informácií, napr. do mobilného telefónu.

Kódy QR možno jednoducho vygenerovať jednoduchým vložením obrázka na serveroch spoločnosti Google.

Napríklad:

<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR kód">

<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR code">

Môžeme nastaviť 3 parametre:

- Veľkosť v `px` (vertikálne a horizontálne)
- Kódovanie (odporúčam `UTF-8`)
- Adresa (URL adresa stránky, telefónne číslo, ...)

> TIP: Ak existuje mobilná verzia stránky, odkazujte na ňu.
>
> Vo väčšine prípadov váš mobilný telefón spracuje kód QR.

Vlastnú funkciu na vkladanie môžeme napísať veľmi jednoducho:

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
