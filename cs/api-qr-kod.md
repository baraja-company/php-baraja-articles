Generátor QR kódů - API
=======================

> id: a9b1804f-438b-49fe-86e7-5a1b30a07210
> slugCS: api-qr-kod
> publicationDate: "2019-09-11 10:04:00"
> mainCategoryId: "8598ac8d-7a70-4e2a-be9a-9f4f9ecee935"

QR kód je speciální 2 rozměrný kód, který se používá pro přenos krátké informace např. do mobilního telefonu.

QR kódy lze snadno vygenerovat prostým vložením obrázku na serverech Googlu.

Například:

<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz">

```html
<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz">
```

Můžeme nastavit 3 parametry:

- Velikost v `px` (vertikální a horizontální)
- Kódování (doporučuji `UTF-8`)
- Adresu (URL ke stránce, telefoní číslo, ...)

> TIP: Pokud existuje mobilní verze webu, tak odkazujte raději na tu.
>
> Ve většině případů bude váš QR kód zpracovávat mobilní telefon.

Velmi jednoduše si pro vkládání můžeme napsat vlastní funkci:

```php
function getQrCode(string $url, int $size = 128, $charset = 'UTF-8'): string
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
