Plik_get_contents
=================

> id: '6c8889f1-95e7-4540-9c3a-0225c6383954'
> slug:
> 	cs: file-get-contents
> 	pl: plik-get-contents
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: f8b180a5f7208dab0d37761a5acdc9e3

Funkcja **file_get_contents** służy do odczytywania pliku i umieszczania jego zawartości w zmiennej. Funkcja ta jest podobna do funkcji <a href="/include">include</a>, ale w odróżnieniu od include może pobierać zdalne pliki z Internetu i przekazywać ich zawartość za pomocą zmiennych.

Przykładowa strona
------

Każda z tych funkcji może być użyta do wczytania pliku lokalnego z dysku:

```php
$news = file_get_contents('news.html');

echo 'Najnowsze wiadomości:<br>' . $news;
```

Lub ze zdalnego adresu URL:

```php
$page = file_get_contents('https://www.google.com');

echo $page;
```

Podczas pobierania adresu URL można pobrać dowolny adres i pobrać jego zawartość w postaci ciągu znaków do zmiennej. W przypadku języka HTML jest to kod źródłowy.

Strona jest wyświetlana niepoprawnie
----------------------------

Dzieje się tak dlatego, że kod HTML jest przekazywany dokładnie w takiej postaci, w jakiej został umieszczony w adresie URL.

Jeśli ścieżka do obrazka to na przykład `<img src="kocka.png">`, to plik ten może nie istnieć w kontekście naszego serwera, dlatego musimy poprawić ścieżkę do na przykład: `<img src="https://server.cz/kocka.png">`.
