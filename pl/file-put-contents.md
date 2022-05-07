File_put_contents
=================

> id: '7ec781c6-e3db-484d-8a49-0b93ab8252b1'
> slug:
> 	cs: file-put-contents
> 	pl: file-put-contents
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '78211a3696ebba56d559045f8a0ab9e4'

Funkcja **file_put_contents** jest odpowiednia do automatycznego zapisu do pliku. Alternatywnie można też użyć <a href="/fopen">fopen()</a>, czego nie polecam dla początkujących.

Przykładowa strona
--------------------------

```php
$file = 'plik.txt';
$content = 'Zawartość, która ma zostać zapisana w pliku.';

file_put_contents($file, $content);
```

Polecenie file_put_contents ma 2 parametry:

- `filename` gdzie zapisywać,
- Treść pliku, który będziemy zapisywać.

> Uwaga: `file_put_contents()` nadpisuje plik najnowszą zawartością.

Uważaj na nadpisywanie
--------------------------

W przypadku zapisywania za pomocą polecenia file_put_contents należy uważać, aby nie nadpisać danych. Funkcja ta usunie całą bieżącą zawartość i zastąpi ją nową zawartością. Jeśli więc chcesz tylko dodać tekst, możesz to zrobić na początku lub na końcu, używając własnego skryptu:

```php
$file = 'plik.txt';
$content = 'Nowa treść.';

$oldContent = file_get_contents($file);
file_put_contents($file, $content . $oldContent);
```

Tak więc najpierw otwierany jest plik, następnie zapisywana jest nowa treść, a po niej oryginalna treść...

Jeśli chcemy dodać starą zawartość przed nową, wystarczy nieco zmodyfikować skrypt:

```php
$file = 'plik.txt';
$content = Nový obsah.';

$oldContent = file_get_contents($soubor);
file_put_contents($file, $oldContent . $content);
```
