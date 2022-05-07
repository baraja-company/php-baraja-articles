Funkcja PHP fopen()
===================

> id: '733ba757-1d5f-4717-a088-5ddabe730838'
> slug:
> 	cs: fopen
> 	pl: funkcja-php-fopen
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '6b791877e57d88840d603dea69967b69'

Funkcja `fopen()` reprezentuje niskopoziomowy dostęp do plików na dysku.

Programista musi wszystko robić sam (otwierać plik, odczytywać dane, zapisywać nowe dane, zamykać plik).

Jeśli chcesz tylko szybko odczytywać i zapisywać pliki, istnieją prostsze opcje:

- <a href="/file-get-contents">file_get_contents()</a> - odczyt z pliku
- <a href="/file-put-contents">file_put_contents()</a> - zapis do pliku

Użycie podstawowe
----------------

```php
$text = 'Każdy zapisany tekst...';

$file = fopen('file.html', 'a+'); // Otwiera plik i tryb

fwrite($file, $text); // Zapisuje do pliku
fclose($file);        // Zamyka plik
```

> Jeśli otworzymy plik do odczytu i nie zostanie on zamknięty, żaden inny proces nie będzie miał do niego dostępu!

Rodzaje trybów obsługi plików
----------------------------

Z plikami można pracować w różnych trybach, które informują o prawach dostępu.

Na przykład, jeśli chcemy otworzyć plik tylko do odczytu, wystarczy tryb `r`.

Jeśli otworzymy plik do zapisu, to zostanie on oznaczony na dysku jako `otwarty` i inny proces (skrypt) nie będzie mógł do niego pisać, dopóki go nie zamkniemy. Dzięki temu plik nie zostanie uszkodzony podczas zapisu.

| Tryb | Znaczenie |
|-------|--------|
| i` | Otwiera plik, jeśli nie istnieje, zostanie utworzony.
| `a+` | Otwiera plik w celu dodania danych lub odczytu danych, jeśli plik nie istnieje, zostanie utworzony |
| `r` | Otwórz tylko do odczytu |
| `r+` | Otwórz do odczytu i zapisu |
| `w` | Otwórz do zapisu, oryginalne dane zostaną usunięte i zastąpione nowymi, jeśli nie istnieją, zostaną utworzone |
| `w+` | Otwórz do zapisu i odczytu, oryginalne dane zostaną usunięte i zastąpione nowymi, jeśli nie istnieją, zostaną utworzone |
