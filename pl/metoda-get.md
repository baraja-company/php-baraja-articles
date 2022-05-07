Pobieranie parametrów z adresu URL za pomocą metody GET
=======================================================

> id: bbf2cb2c-f7f7-4be9-a9cd-960014db0f51
> slug:
> 	cs: metoda-get
> 	pl: pobieranie-parametrow-z-adresu-url-za-pomoca-metody-get
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: caebe20b6b7cdf0b3703ff55dad6f094

Masz otwartą stronę, podążasz za adresem URL i widzisz znak zapytania z pewnymi parametrami. Niedoświadczony programista pomyślałby, że są to oddzielne pliki, ale oto one. Spróbuj utworzyć plik, który ma w nazwie znak zapytania (to nie działa). **To jest powód, dla którego powstał ten artykuł**.

Co to jest?
--------------------------

Właściwie chodzi o to, że jest to pojedynczy plik, do którego przekazuje się zmienne za pośrednictwem adresu URL, więc mam, powiedzmy, plik **index.php** i przekazuję mu nazwę artykułu: **index.php?clanek=o-php**.

Kod + wyjaśnienie
--------------------------

Zmienna superglobalna `$_GET` zawiera klucze z parametrami z adresu URL

```php
echo $_GET['Artykuł'] ?? '';
```

Ograniczenia dotyczące bezpieczeństwa i długości
--------------------------

Metoda GET nie jest bezpieczna, dlatego nie należy przesyłać za jej pośrednictwem poufnych danych. Jednym z głównych powodów jest to, że jest to komunikacja nieszyfrowana, a po drugie - przechowywana w historii.

Dane poufne lub po prostu wszystko należy przesyłać za pomocą metody <a href="/method-post">POST</a>. GET jest bardziej odpowiednie dla furmularzy, w których dobrze jest pokazać parametry (np. wyszukiwarki, strona z artykułami), aby można było utworzyć odnośnik do strony.

Czas trwania GET nie jest nieograniczony! Wielu początkujących za to płaci. Maksymalna długość wynosi około 1024 znaków (w niektórych miejscach mówi się o 1088). Dlatego w przypadku dłuższych tekstów należy wysłać <a href="/method-post">POST</a> z.
