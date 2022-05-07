Apostrofy i cudzysłów
=====================

> id: '526ad995-3412-446e-bb56-9627dff8e29e'
> slug:
> 	cs: apostrofy-a-uvozovky
> 	pl: apostrofy-i-cudzyslow
> 
> perex:
> 	- Použití uvozovek a apostrofů pro ohraničení řetězců v PHP. Escapování řetězců a řešení kontextů.
> 	- Używanie cudzysłowów i apostrofów do ograniczania łańcuchów w PHP. Ucieczka z łańcuchów i rozwiązywanie kontekstów.
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c5b0bf8b74238134be5348f886591e2a

Do rozgraniczania ciągów znaków można używać cudzysłowów lub apostrofów. Osobiście preferuję tylko **apostrofy**, chyba że jest to specjalny znak podziału wiersza lub tabulator.

Powodów jest wiele, przeanalizujmy je konstruktywnie.

> Głównym powodem, dla którego nie należy używać cudzysłowów, jest bezpieczeństwo i niewłaściwa obsługa typów danych.

Używanie znaczników HTML wewnątrz łańcucha znaków
--------------------------------

Jeśli chcemy zwrócić kod HTML w postaci łańcucha i zawijamy ten łańcuch w cudzysłów, musimy uciekać w dość niewygodny sposób:

```php
return "<a href=\"{$link}\">{$text}</a>";
```

Można też zrobić to samo w bardziej czytelny sposób, zachowując cudzysłów bez ucieczki:

```php
return '<a href="' . $link . '">' . $text . '</a>';
```

Większa odległość wzrokowa sznurka od zmiennej
---------------------------------------------

Nie ma nic gorszego niż umieszczanie zmiennych wewnątrz łańcucha bezpośrednio jedna na drugiej, gdzie nie można szybko odróżnić, co jest zmienną, a co łańcuchem:

```php
$url = "{$baseImageUrl}/{$dirName}/{$basename}.{$ext}";
```

Taka notacja nie ma negatywnego wpływu na funkcjonalność, jedynie poszczególne zmienne i stałe znaki z łańcucha są ułożone zbyt blisko siebie, co pogarsza czytelność.

Spróbujmy jeszcze raz i uczyńmy go bardziej czytelnym:

```php
$url = $baseImageUrl . '/' . $dirName . '/' . $basename . '.' . $ext;
```

Główną zaletą, jaką dostrzegam, jest czystość wokół zmiennych, w której nie przeszkadzają żadne niepotrzebne znaki w nazwie.

Ponadto łatwiej będzie zawijać i nie będzie znaków zawijania w łańcuchu ;)

```php
$url = $baseImageUrl
       . '/' . $dirName
       . '/' . $basename . '.' . $ext;
```

Nie jest możliwe wywołanie funkcji wewnątrz cudzysłowów
---------------------------------------

Ale zmienna może, dlatego "coś" jest dołączane na zewnątrz łańcucha (zwłaszcza w funkcjach), a zmienne są zapisywane w środku. Czasami programista dodaje nawet zmienną po łańcuchu znaków. Krótko mówiąc, zamęt nad zamętami.

Dlaczego nie możemy robić rzeczy w sposób jednolity?

Niewłaściwe odwzorowanie innego typu danych na ciąg znaków
---------------------------------------------------

Rozważmy następujące wywołanie funkcji:

```php
echo getFullName("{$user->name}");

function getFullName(string $name): string
{
	// ... implementacja ...
}
```

Możliwe jest wstawianie zmiennych w cudzysłowy, co spowoduje, że zostaną one przepisane jako ciąg znaków. Jeśli jednak zmienna znajduje się w samym łańcuchu i ma inny typ danych niż łańcuch, informacje o oryginalnym typie danych mogą zostać uszkodzone. Na przykład, jeśli konstrukcja `$user->name` zwróciłaby `false` lub `null`, nie bylibyśmy w stanie stwierdzić, że jest to błąd.

Aplikacja powinna zawsze rozpoznać stan błędu i odpowiednio zareagować, a nie ignorować go. Właściwe raportowanie błędów umożliwia lepsze debugowanie w przyszłości.

Niekiedy można spotkać się z nadpisywaniem funkcji, zwłaszcza dlatego, że wymaga ona określonego typu danych:

```php
trim("{$returnText}");
```

W takim przypadku jestem bardziej skłonny do zapisania go:

```php
trim ((string) $returnText);
```

co nie jest tak "magiczne" i nawet dla mniej wprawnych użytkowników jest oczywiste, co się stało ze zmienną.

Usunięcie wartości `null` z bazy danych
----------------------------------

Załóżmy, że pobieramy nazwę hotelu z bazy danych:

```php
return "{$row->hotel->name}";
```

Ale co zrobić, jeśli nazwa nie istnieje i jest `null`? Używając cudzysłowów, stworzyliśmy potencjalnie trudny do wykrycia błąd, ponieważ zostanie on przepisany na pusty ciąg znaków. Chcielibyśmy jednak zwrócić prawdziwy `null`. Różnica polega na tym, że rekord nie istnieje lub istnieje, ale jest pusty.

Właściwym sposobem jest więc nie podawanie żadnych danych:

```php
return $row->hotel->name;
```

Czy funkcja wymaga zwrócenia określonego typu danych i czy jest w tym względzie ścisła? Jeśli nie możemy spełnić wymagań specyfikacji, powinniśmy rzucić wyjątek.

```php
function getHotelName(int $hotelId): string
{
   // implementacja

   if ($row->hotel->name === null) {
      throw new \Exception('Nazwa hotelu nie istnieje.');
   }

   return $row->hotel->name;
}
```

Niespójności w kodowaniu znaków i typów danych
--------------------------------------------

Nierzadko można spotkać się z konstrukcjami w rodzaju:

```php
echo "{$returnCode}" . self::CRLF;
```

Co lepiej zapisać jako:

```php
echo $returnCode . "\n";
```

Użycie cudzysłowu wokół zmiennej jest w tym przypadku niepotrzebne i tylko utrudnia czytanie, ponieważ zawiera dodatkowe, zbędne znaki. Jednocześnie, zawijanie linii typu `CRLF` nie jest nowoczesne i lepiej jest używać tylko ``n``, które jest natywne dla kodowania `UTF-8`.

Prawidłowo można by jeszcze sprawdzać poprawność typu danych w kodzie. Na przykład, że jest to liczba i ewentualnie zwróci zamiennik. Można to zrobić za pomocą <a href="/ternary-operator">operatoraternary</a>:

```php
echo (is_int($returnCode) ? $returnCode : '?') . "\n";
```

> Uwaga: Użycie funkcji `is_int()` świadczy o złym zaprojektowaniu aplikacji, ponieważ nigdy nie powinno się zdarzyć, że nie znamy typu danych zmiennej.

Zawijanie łańcucha wyjściowego w apostrofy
---------------------------------------

Często konieczne jest ujęcie w cudzysłów lub apostrofy ciągu wyjściowego ze zmiennej w tekście wyjątku. Jednak niepotrzebnie tworzy to "odpłatność" dla obu typów ograniczników łańcucha, a jeśli łańcuch zaczyna się i kończy tym samym znakiem, nie jest możliwe szybkie jednoznaczne określenie, gdzie łańcuch się zaczął i skończył w przypadku wielu łańcuchów, jeśli w środku pojawia się apostrof:

```php
throw new \Exception(__METHOD__ . ": Unsupported data type '{$fileType}'");
```

Osobiście rozwiązuję ten problem, umieszczając łańcuch w innym typie znaków (dobrze sprawdzają się nawiasy kwadratowe), dzięki czemu początek i koniec łańcucha są dobrze widoczne:

```php
throw new \Exception(__METHOD__ . ': Niewłaściwy typ danych [' . $fileType . ']');
```

Lub:

```php
throw new \Exception("Status '{$status}' is invalid");
```

Może być wymieniony na:

```php
throw new \Exception('Status [.' . $status . '] jest nieważny');
```

Parsowanie według wierszy
--------------------

Znaki cudzysłowu są przydatne na przykład przy przetwarzaniu przez nowy wiersz:

```php
foreach(explode("\n", $text) as $line) {
	// implementacja
}
```

Zostały one stworzone specjalnie do tego celu.

Jeśli jednak trzeba przetworzyć bardziej złożony dokument (np. kod źródłowy języka programowania lub stronę HTML), polecam użycie **Tokenizatora** wraz z <a href="/regex">wyrażeniami regularnymi</a>.

Nie należy łączyć dwóch metod zamykania
-----------------------------------

Jeśli z jakiegoś powodu i tak zamierzasz używać cudzysłowów, to przynajmniej zachowaj ich spójność w całym tekście.

Jest to sprawa o charakterze odstraszającym:

```php
throw new \Exception("Obraz w adresie URL nie istnieje. ResponseSize:" . strlen($result) . ')');
```

Początek łańcucha jest ograniczony cudzysłowami, a koniec apostrofami.

Usterka ta występuje zwłaszcza podczas używania narzędzi do automatycznego formatowania (takich jak **Code Sniffer**), które próbują ujednolicić konwencje, ale nie zawsze im się to udaje.
