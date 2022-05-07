Zmienne superglobalne
=====================

> id: bc2107ee-6551-4559-8c02-9cecdf98d33b
> slug:
> 	cs: superglobalni-promenna
> 	pl: zmienne-superglobalne
> 
> publicationDate: '2019-11-01 09:29:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '792e217f1331f38ed0719d7abeb05e67'

Zmienne superglobalne są używane do przekazywania globalnego stanu aplikacji i komunikacji HTTP.

Główną zaletą tych zmiennych jest to, że są one zawsze i wszędzie dostępne. W praktyce są to tablice wartości, w których dostęp do określonych informacji uzyskuje się za pomocą indeksu. W różnych kontekstach dostępność kluczy może być różna (wyjaśnienie poniżej).

Typy zmiennych superglobalnych
--------------------------------

Wszystkie superglobale w PHP są tablicami i są oznaczane znakiem dolara, po którym następuje podkreślnik (z wyjątkiem `$GLOBALS`) i wielkie litery.

W `PHP 7` występują w szczególności następujące elementy:

| Zmienna | Opis |
|-------------|-------|
| `$_GET` | Parametry adresu URL <a href="/metody-odesilani-dat">wysłane metodą GET</a>.
| `$_POST` | Dane formularza <a href="/methods-odesilani-dat">wysłane przez POST</a>. Zwróć uwagę, że <a href="/ajax-post">może zachowywać się inaczej w ajaxie</a>.
| `$_REQUEST` | Dane formularza wysłane dowolną metodą (`$_GET`, `$_POST` i `$_REQUEST`).
| `$_FILES` | Informacje techniczne o aktualnie załadowanych plikach, na przykład poprzez konstrukcję `<input type="file">`.
| `$_SERVER` | <a href="/info">ustawienia serwera WWW</a>, adres IP, konfiguracja... różni się w zależności od środowiska (przy wywoływaniu skryptu PHP z Terminala będzie zawierał inne wartości i na przykład będzie brakowało informacji o bieżącym żądaniu).
| `$_COOKIE` | Skonfigurowane <a href="/cookies">cookies</a>.
| `$_SESSION` | Dane sesji (<a href="/sessions">session</a>), jeśli istnieje i została ustawiona w przeszłości.
| `$GLOBALS` | **Ostrzeżenie, nie zawiera podkreślenia w nazwie!** Jest to tak zwana <a href="global-variable">globalna zmienna</a> i alternatywna notacja dla słowa kluczowego `globalny`. Jeśli masz w swojej aplikacji zmienną globalną `$zmienna`, możesz również uzyskać do niej dostęp za pomocą konstrukcji `$GLOBALS["zmienna"]`. Jednak używanie zmiennych globalnych jest z założenia złym i nieczystym rozwiązaniem, więc lepiej tego nie robić.
| `$_ENV` | Informacje o bieżącym środowisku, w którym PHP jest uruchomione.

Wypisanie wszystkich istniejących wartości jest łatwe do wykonania:

```php
foreach ($_SERVER as $key => $value {
	echo $key . ':' . $value . '<br>.';
}
```

> Uwaga: Nie wszystkie indeksy muszą istnieć zawsze (na przykład jeśli skrypt uruchamia crona w trybie CLI, indeks z adresem URL strony lub adresem IP żądania nie będzie istniał).

Dostęp do zmiennych
-------------------

Zalecam, aby wszystkie zmienne globalne (z wyjątkiem `$_SESSION`) były tylko do odczytu. Dzieje się tak dlatego, że zawierają one globalne dane aplikacji i inny kod może je uwzględniać (na przykład inna zainstalowana biblioteka).

Inną wadą stanu globalnego jest to, że nie zawsze można polegać na dokładnych wartościach, nawet jeśli one istnieją, więc zawsze należy sprawdzać ich klucze za pomocą konstrukcji `isset()`.

Aby zapisać nowy plik cookie, należy użyć `setcookie()`, a nie wstawiać wartość bezpośrednio. Dzieje się tak, ponieważ jest to funkcja tylko do odczytu.

Wyciągnięte wnioski
-------

Nigdy nie należy ślepo ufać wartościom zmiennych superglobalnych!

Użytkownik może użyć adresu URL i przesłanych nagłówków, aby wpłynąć na sposób ustawiania wartości. Wszystkie dane wejściowe powinny być zawsze starannie zatwierdzane.

Rejestrowanie globali - problemy ze starą wersją PHP
------------------------------------------

W starej wersji PHP (aż do `5.4.0`) istniała specjalna dyrektywa `register-globals` (konfigurowalna w `php.ini`), która powodowała, że wszystkie parametry przekazywane w adresie URL były automatycznie rejestrowane jako zmienne.

Na przykład:

Użytkownik wszedł na adres URL: `https://example.com/script.php?var=24`.

PHP automatycznie utworzyło w skrypcie zmienną `$var` o wartości `24`.

Tak więc działało to klasycznie:

```php
echo $var;
```

Każdy może więc wrzucić dowolną zmienną do skryptu i zmienić jego treść. Oczywiście bezpieczeństwo nie zawsze było priorytetem. Nie do końca.

Inne źródła
------------

Bardziej szczegółowy opis można znaleźć w <a href="https://www.php.net/manual/en/language.variables.superglobals.php">oficjalnym podręczniku</a>.
