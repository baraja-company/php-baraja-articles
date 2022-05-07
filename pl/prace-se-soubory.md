Praca z plikami
===============

> id: '12330cfb-5729-419a-b2e4-cb3d1db998cc'
> slug:
> 	cs: prace-se-soubory
> 	pl: praca-z-plikami
> 
> publicationDate: '2020-02-16 16:30:05'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '0f5b9e84a974285dcb63baafc912be5f'

W PHP istnieje wiele funkcji służących do pracy z plikami.

- <a href="/fopen">fopen()</a>, niskopoziomowy dostęp do plików na dysku
- <a href="/file-get-contents">file_get_contents()</a>, pobieranie zawartości pliku lub adresu URL
- <a href="/file-put-contents">file_put_contents()</a>, zapisując łańcuch znaków do pliku lokalnego.

Funkcje dysków
--------------

- `unlink($path)`, usuń plik
- `copy($from, $to)`, kopiuje plik z jednej lokalizacji do innej (może być z adresu URL na dysk lokalny)
- rename()`, zmiana nazwy lub przeniesienie pliku na dysku
- `chmod()`, zmiana uprawnień do odczytu, zapisu lub wykonania pliku
