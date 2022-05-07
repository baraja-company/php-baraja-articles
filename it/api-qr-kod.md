Generatore di codici QR - API
=============================

> id: a9b1804f-438b-49fe-86e7-5a1b30a07210
> slug:
> 	cs: api-qr-kod
> 	it: generatore-di-codici-qr---api
> 
> publicationDate: '2019-09-11 10:04:00'
> mainCategoryId: '8598ac8d-7a70-4e2a-be9a-9f4f9ecee935'
> sourceContentHash: '89824ae94a740439f4ad3920bd311d3d'

Un codice QR è un codice speciale bidimensionale che viene utilizzato per trasmettere brevi informazioni, ad esempio ad un telefono cellulare.

I codici QR possono essere generati facilmente inserendo semplicemente un'immagine sui server di Google.

Per esempio:

<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR code">

```html
<img src="https://chart.apis.google.com/chart?cht=qr&chs=100x100&choe=UTF-8&chld=H%7C0&chl=https://php.baraja.cz" alt="QR code">
```

Possiamo impostare 3 parametri:

- Dimensione in `px` (verticale e orizzontale)
- Codifica (raccomando `UTF-8`)
- Indirizzo (URL della pagina, numero di telefono, ...)

> CONSIGLIO: Se c'è una versione mobile del sito, collegatevi invece a quella.
>
> Nella maggior parte dei casi, il tuo codice QR sarà elaborato dal tuo cellulare.

È molto facile scrivere la propria funzione per l'incorporazione:

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
