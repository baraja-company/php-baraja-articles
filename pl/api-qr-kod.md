Generator kodów QR - API
========================

> id: a9b1804f-438b-49fe-86e7-5a1b30a07210
> slug:
> 	cs: api-qr-kod
> 	pl: generator-kodow-qr---api
> 
> publicationDate: '2019-09-11 10:04:00'
> mainCategoryId: '8598ac8d-7a70-4e2a-be9a-9f4f9ecee935'
> sourceContentHash: '89824ae94a740439f4ad3920bd311d3d'

Kod QR to specjalny dwuwymiarowy kod, który służy do przesyłania krótkich informacji, np. do telefonu komórkowego.

Kody QR można łatwo wygenerować, po prostu wstawiając obraz na serwery Google.

Na przykład:

<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="Kod QR">.

```html
<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR code">
```

Można ustawić 3 parametry:

- Rozmiar w `px` (pionowo i poziomo)
- Kodowanie (zalecam `UTF-8`)
- Adres (URL do strony, numer telefonu, ...)

> WSKAZÓWKA: Jeśli istnieje wersja mobilna witryny, należy zamieścić do niej link.
>
> W większości przypadków kod QR zostanie przetworzony przez telefon komórkowy.

Bardzo łatwo jest napisać własną funkcję do osadzania:

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
