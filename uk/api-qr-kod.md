Генератор QR-кодів - API
========================

> id: a9b1804f-438b-49fe-86e7-5a1b30a07210
> slug:
> 	cs: api-qr-kod
> 	uk: generator-qr-kodiv-api
> 
> publicationDate: '2019-09-11 10:04:00'
> mainCategoryId: '8598ac8d-7a70-4e2a-be9a-9f4f9ecee935'
> sourceContentHash: '89824ae94a740439f4ad3920bd311d3d'

QR-код - це спеціальний двовимірний код, який використовується для передачі короткої інформації, наприклад, на мобільний телефон.

QR-коди можна легко згенерувати, просто вставивши зображення на серверах Google.

Наприклад:

<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR code">

```html
<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR code">
```

Ми можемо задати 3 параметри:

- Розмір в `px` (по вертикалі та горизонталі)
- Кодування (рекомендую `UTF-8`)
- Адреса (URL на сторінку, номер телефону, ...)

> ПОРАДА: Якщо є мобільна версія сайту, замість цього зробіть посилання на неї.
>
> У більшості випадків ваш мобільний телефон обробить ваш QR-код.

Ми можемо дуже легко написати власну функцію для вбудовування:

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
