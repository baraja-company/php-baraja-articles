PHP-funktion fopen()
====================

> id: '733ba757-1d5f-4717-a088-5ddabe730838'
> slug:
> 	cs: fopen
> 	da: php-funktion-fopen
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '6b791877e57d88840d603dea69967b69'

Funktionen `fopen()` repræsenterer adgang på lavt niveau til filer på disken.

Programmøren skal selv gøre alting (åbne filen, læse data, skrive nye data, lukke filen).

Hvis du blot skal læse og skrive filer hurtigt, er der enklere muligheder:

- <a href="/file-get-contents">file_get_contents()</a> - læsning fra en fil
- <a href="/file-put-contents">file_put_contents()</a> - skrivning til en fil

Grundlæggende brug
----------------

```php
$text = 'Enhver tekst, der er gemt...';

$file = fopen('file.html', 'a+'); // åbner fil og tilstand

fwrite($file, $text); // Gemmer i en fil
fclose($file);        // Lukker filen
```

> Hvis vi åbner en fil til læsning, og den ikke er lukket, kan ingen anden proces få adgang til den!

Typer af filhåndteringstilstande
----------------------------

Vi kan arbejde med filer i forskellige tilstande, som giver oplysninger om adgangsrettigheder.

Hvis vi f.eks. ønsker at åbne en fil til skrivebeskyttet læsning, er `r`-tilstanden tilstrækkelig.

Hvis vi åbner filen til skrivning, vil den blive markeret som `open` på disken, og en anden proces (script) vil ikke kunne skrive til den, før vi lukker den igen. Dette sikrer, at filen ikke bliver ødelagt under skrivning.

| Tilstand | Betydning |
|-------|--------|
| `og` | Åbner filen, hvis den ikke findes, oprettes den |
| `a+` | Åbner en fil for at tilføje data og/eller læse data, hvis den ikke findes, oprettes den |
| `r` | Åbn skrivebeskyttet |
| | `r+` | Åbner for læsning og skrivning |
| `w` | Åben for skrivning, originale data slettes og erstattes med nye data, hvis de ikke findes, oprettes de |
| `w+` | Åbner for skrivning og læsning, originale data slettes og erstattes af nye data, hvis de ikke findes, oprettes de |
