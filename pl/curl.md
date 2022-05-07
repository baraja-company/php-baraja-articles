cURL w PHP - pobieranie danych przez URL
========================================

> id: '0bd1aed6-460d-4b63-9afe-f5087d1c6046'
> slug:
> 	cs: curl
> 	pl: curl-w-php---pobieranie-danych-przez-url
> 
> perex:
> 	- Knihovna cURL je robustní PHP knihovna pro pokročilou HTTP komunikaci.
> 	- Biblioteka cURL jest solidną biblioteką PHP służącą do zaawansowanej komunikacji HTTP.
> 
> publicationDate: '2020-02-15 22:14:32'
> mainCategoryId: '02d93aa8-ed83-460a-ab97-4de21118f019'
> sourceContentHash: '7c565449293cf1ce4950f309abaf04d0'

Biblioteka PHP `cURL` jest dobrym sposobem na pobieranie danych z obcego serwera.

Na podstawie zapytania tworzy on żądanie HTTP, które wysyła do serwera docelowego, a po pobraniu zawiera interfejs API umożliwiający (stosunkowo) łatwą obsługę danych.

W przeciwieństwie do natywnej funkcji `file_get_contents` (za pomocą której również możemy wykonywać żądania HTTP), oferuje ona znacznie lepsze opcje konfiguracyjne i pobiera strony/pliki jak prawdziwa przeglądarka.

> Funkcja `file_get_contents` używa wewnętrznie biblioteki `cURL`, po prostu nie ma tak szczegółowych opcji konfiguracyjnych.

Wykrywanie trybu cURL w żądaniu
----------------------------

Często przydatne jest wykrycie, czy bieżące żądanie zostało wykonane przez `cUrl`, czy klasycznie w przeglądarce.

W PHP nie ma bezpośredniej implementacji tej funkcji, ale możemy sami napisać prostą funkcję:

```php
function isCurl(): bool
{
    return str_contains($_SERVER['HTTP_USER_AGENT'] ?? '', 'curl');
}
```

Jeśli korzystasz z systemu Linux i jego Terminala lub masz komputer Mac, spróbuj wykonać to polecenie:

```shell
curl https://php.baraja.cz/curl
```

Polecenie wysyła wewnętrzne zapytanie do tej witryny i zwraca jego wynik.

Jeśli aplikacja nie wykryła żądania cURL, kod HTML zostanie zwrócony w taki sposób, jakby żądanie pochodziło z przeglądarki. Ponieważ jednak typy żądań są wykrywane, nic nie stoi na przeszkodzie, aby zwrócić wyczyszczony artykuł w formacie Markdown.

Zaletą tego rozwiązania jest znacznie lepsze oczyszczanie danych. Użytkownikowi w przeglądarce pokazujemy sformatowany HTML, a robotowi tylko podstawową treść.

Szczegółowe wykorzystanie interfejsu API w PHP
--------------------------

Jeśli szukasz szczegółowych informacji na temat używania cUrl, polecam przeczytanie <a href="https://www.php.net/manual/en/book.curl.php">oficjalnej dokumentacji</a>, która jest zawsze aktualna.

Do zwykłego użytku dostępna jest biblioteka <a href="https://docs.guzzlephp.org/en/stable/">**Guzzle**</a>, która obsługuje
