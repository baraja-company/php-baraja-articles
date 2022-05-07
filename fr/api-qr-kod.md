Générateur de codes QR - API
============================

> id: a9b1804f-438b-49fe-86e7-5a1b30a07210
> slug:
> 	cs: api-qr-kod
> 	fr: generateur-de-codes-qr---api
> 
> publicationDate: '2019-09-11 10:04:00'
> mainCategoryId: '8598ac8d-7a70-4e2a-be9a-9f4f9ecee935'
> sourceContentHash: '89824ae94a740439f4ad3920bd311d3d'

Un code QR est un code bidimensionnel spécial utilisé pour transmettre des informations courtes, par exemple à un téléphone portable.

Les codes QR peuvent être facilement générés en insérant simplement une image sur les serveurs de Google.

Par exemple :

<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR code">

```html
<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR code">
```

Nous pouvons régler 3 paramètres :

- Taille en `px` (vertical et horizontal)
- Encodage (je recommande `UTF-8`)
- Adresse (URL de la page, numéro de téléphone, ...)

> CONSEIL : s'il existe une version mobile du site, créez un lien vers celle-ci.
>
> Dans la plupart des cas, votre téléphone portable traitera votre code QR.

Nous pouvons écrire notre propre fonction d'incorporation très facilement :

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
