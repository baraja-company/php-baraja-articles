Przetwarzanie miniatur i meta informacji z Vimeo
================================================

> id: '8b8cbef0-210a-4883-be07-5004e8f43739'
> slug:
> 	cs: zpracovani-nahledovych-obrazku-z-vimea
> 	pl: przetwarzanie-miniatur-i-meta-informacji-z-vimeo
> 
> publicationDate: '2020-09-19 17:32:31'
> mainCategoryId: a0143f3c-ac75-46dc-a514-d3c9417ded4e
> sourceContentHash: '93c29218f162fb5dc8d0f265968b0f8d'

Podczas osadzania filmów z Vimeo na stronie (jako osadzenie HTML) często chcemy również uzyskać obraz i inne przydatne informacje, takie jak długość filmu, pełny tytuł, autor itd.

Na szczęście Vimeo udostępnia proste API HTTP, z którego możemy odczytać wszystkie dane na podstawie tokena wideo.

Aby uniknąć konieczności samodzielnego pisania interfejsu API, wystarczy użyć pakietu [ready package](https://github.com/baraja-core/vimeo-video-api), który w pełni integruje interfejs API.

Pakiet instaluje się za pomocą polecenia:

```shell
composer require baraja-core/vimeo-video-api
```

Jest łatwy w użyciu. Tworzysz instancję usługi `BarajaVimeoAPI` do komunikacji z Vimeo zgodnie z dokumentacją, wywołujesz metodę `getInfo()`, przekazujesz token wideo i otrzymujesz szczegółowe informacje jako encję `VideoInfo`, z której można odczytać wszystkie dostępne informacje (nie zawsze wszystkie informacje są dostępne dla każdego wideo).

W ten sposób można wyszukiwać nawet prywatne i publicznie niedostępne filmy. Zawsze jednak trzeba znać ich token.

Wyszczególnienie wszystkich dostępnych informacji
---------

Podstawowy sposób korzystania z biblioteki wygląda następująco:

```php
$api = new \Baraja\VimeoAPI\VimeoVideoAPI;

$token = 0; // Token wideo jako liczba całkowita
$info = $api->getInfo($token);

echo var_dump($info); // wymienia wszystko

// Wypisuje długość filmu w sekundach:
echo 'Długość filmu wynosi:' . $info->getDuration();
```

Zmienna `$info` przechowuje wszystkie informacje opisowe o danym filmie. Przegląd wszystkich dostępnych metod [można znaleźć w implementacji](https://github.com/baraja-core/vimeo-video-api/blob/master/src/VideoInfo.php).
