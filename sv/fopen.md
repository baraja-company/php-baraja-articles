PHP-funktion fopen()
====================

> id: '733ba757-1d5f-4717-a088-5ddabe730838'
> slug:
> 	cs: fopen
> 	sv: php-funktion-fopen
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '6b791877e57d88840d603dea69967b69'

Funktionen `fopen()` representerar lågnivååtkomst till filer på disken.

Programmeraren måste göra allt själv (öppna filen, läsa data, skriva nya data, stänga filen).

Om du bara behöver läsa och skriva filer snabbt finns det enklare alternativ:

- <a href="/file-get-contents">file_get_contents()</a> - läser från en fil
- <a href="/file-put-contents">file_put_contents()</a> - skriver till en fil

Grundläggande användning
----------------

```php
$text = 'Varje text som sparas...';

$file = fopen('file.html', 'a+'); // Öppnar fil och läge

fwrite($file, $text); // Sparar i en fil
fclose($file);        // Stänger filen
```

> Om vi öppnar en fil för läsning och den inte stängs kan ingen annan process få tillgång till den!

Typer av filhanteringslägen
----------------------------

Vi kan arbeta med filer i olika lägen, vilket ger information om åtkomsträttigheter.

Om vi till exempel vill öppna en fil för enbart läsning räcker det med läget `r`.

Om vi öppnar filen för att skriva kommer den att markeras som "öppen" på disken och en annan process (skript) kommer inte att kunna skriva till den förrän vi stänger den igen. Detta garanterar att filen inte skadas under skrivningen.

| Läge | Betydelse |
|-------|--------|
| `och` | Öppnar filen, om den inte finns kommer den att skapas |
| `a+` | Öppnar en fil för att lägga till data och/eller läsa data, om den inte finns kommer den att skapas |
| `r` | Öppna skrivskyddad information |
| | `r+` | Öppna för läsning och skrivning |
| `w` | Öppna för skrivning, ursprungliga data raderas och ersätts med nya data, om de inte finns kommer de att skapas |
| `w+` | Öppna för skrivning och läsning, ursprungliga data raderas och ersätts med nya data, om de inte finns kommer de att skapas |
