Wyjątki i ich wyłapywanie w PHP
===============================

> id: '61b0f178-bb1c-4166-9e8a-af49de2e2a8c'
> slug:
> 	cs: vyjimky
> 	pl: wyjatki-i-ich-wylapywanie-w-php
> 
> publicationDate: '2020-02-16 22:18:18'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: d843fe11a092943db429cb8a28384a31

Wyjątki są narzędziem programowania obiektowego, które zapewnia elegancki sposób rzucania i obsługi (leczenia) błędów aplikacji.

Wyjątek jest najpierw rzucany (`thrown`), traktowany (`try`) i łapany (`catch`). Obowiązkowe jest tylko rzucanie.

Filozofia tworzenia wyjątków
-------------------------

Przed pojawieniem się wyjątków obsługa błędów w programowaniu była bardzo skomplikowana, ponieważ musieliśmy polegać na wartościach zwracanych przez funkcje, wychwytywać je na swój własny sposób i odpowiednio się zachowywać.

W rzeczywistości funkcje same w sobie nie wymuszają obsługi błędów, co może prowadzić do fatalnych w skutkach problemów, ale David napisał o tym w <a href="https://phpfashion.com/programatori-chyby-neignoruji">Programiści nie ignorują błędów</a>.

Przykład zapomnianej obsługi błędów:

```php
// przechodzenie z dysku na dysk
copy('c:/oldfile', 'd:/newfile');
unlink('c:/oldfile');
// jeśli pierwsza operacja się nie powiedzie, plik zostanie nieodwracalnie usunięty
```

Dzieje się tak dlatego, że poprawnym sposobem obsługi danych wyjściowych z funkcji `copy()` jest zaniechanie kontynuacji i wyrzucenie błędu. W przypadku starych, dobrych funkcji może to wyglądać następująco:

```php
function backup(): bool
{
   if (copy('c:/oldfile', 'd:/newfile')) {
      return unlink('c:/oldfile');
   }

   return false;
}
```

Nasza funkcja `backup()` zwróci `true` tylko wtedy, gdy funkcja `copy()` nie zawiodła, a funkcja `unlink()` zwróciła `true`. W przeciwnym razie zwróci `false`.

Ale czy teraz jest to bezpieczne dla aplikacji? Nie jest! Ponieważ teraz musimy traktować wyjście funkcji `backup()` w momencie jej wywołania, a jeśli się nie powiedzie, nie będziemy nawet wiedzieli, dlaczego. Krótko mówiąc, zwróci on `false`, a my musimy sami jakoś wykryć błąd. Myślę, że w tym przypadku dobrze jest zobaczyć, że programiści często rezygnują z obsługi błędów lub po prostu zapominają o obsłudze czegoś, a aplikacja wyrzuca z tego powodu trudne do wykrycia błędy.

Rozwiązaniem tego problemu jest stosowanie wyjątków, które wymuszają leczenie, a jeśli nie są leczone, aplikacja ulega całkowitemu zawieszeniu, a my zawsze dowiadujemy się, dlaczego.

Podstawowa definicja wyjątku
--------------------------

W PHP wyjątek jest specjalnym rodzajem interfejsu implementowanym przez natywną klasę `Exception`, której będziemy używać.

Jeśli przetwarzanie jakiejś części programu nie powiedzie się, po prostu rzucamy wyjątek opisujący problem:

```php
if (copy('c:/oldfile', 'd:/newfile') === false) {
   throw new \Exception('Nie można skopiować pliku "oldfile".');
}
```

Rzucanie wyjątków odbywa się za pomocą słowa kluczowego `throw`, po którym następuje utworzenie instancji klasy z wyjątkiem. Możemy również uzyskać instancję w inny sposób (na przykład przekazując ją ze zmiennej), a samo utworzenie instancji wyjątku nie powoduje jego rzucenia.

Pierwszy argument konstruktora klasy `Exception` przyjmuje tekst wyjątku, który powinien wyjaśniać w zwięzły sposób, co się właśnie stało. Dobrą praktyką jest również zamieszczanie informacji o wykonywanej operacji oraz odniesienia do danych. Na przykład, jeśli kopiowanie pliku nie powiodło się, dobrą praktyką jest przekazanie nazwy pliku. Jeśli wykonanie zapytania SQL nie powiedzie się, ponownie przekazujemy wykonywane zapytanie. Pomoże nam to później w obsłudze błędów, ponieważ będziemy mogli dokładnie zobaczyć, na czym polega problem.

Obsługa wyjątków
-----------------

Na przykład funkcja `backup()`, która wykonuje kopię zapasową danych i może wyrzucić parę błędów:

```php
function backup(): void
{
   if (copy('c:/oldfile', 'd:/newfile')) {
      if (unlink('c:/oldfile') === false) {
         throw new \Exception('Nie można usunąć starego pliku.');
      }
   }

   throw new \Exception('Nie można skopiować plików kopii zapasowej.');
}
```

Zwróć uwagę, że funkcja nie zwraca żadnego wyjścia, a w definicji podajemy typ `void`. Funkcja nie musi niczego zwracać, ponieważ sukces jest uważany za stan, w którym nie jest rzucany żaden błąd, a my nie musimy traktować pozytywnego scenariusza.

Gdybyśmy mieli użyć tej funkcji w aplikacji bez leczenia, na przykład w następujący sposób:

```php
echo 'Tworzenie kopii zapasowych plików...';
backup();
echo 'Tworzenie kopii zapasowej zakończone.';
```

To jest normalny sposób działania. Jeśli jednak wystąpi błąd, skrypt zostanie automatycznie zakończony i na wyjściu zostanie wyświetlony tekst wyjątku. Co ważne, nie będzie on kontynuował wykonywania kodu i wiemy, że nie dojdzie do uszkodzenia danych.

Jeśli chcemy kontynuować wykonywanie programu, musimy **wyczyścić** błąd, co robimy za pomocą konstrukcji `try` i `catch`:

```php
echo 'Tworzenie kopii zapasowych plików...';
try {
   backup();
} catch (\Exception $e) {
   echo 'Tworzenie kopii zapasowej nie powiodło się:' . $e->getMessage();
}
echo 'Tworzenie kopii zapasowej zakończone.';
```

Jeśli zostanie rzucony wyjątek, wywoływany jest kod w obszarze `catch()` (który akceptuje wyjątek, jeśli pasuje do jego typu danych) i wykonywany jest kod wewnętrzny.

Zawsze otrzymujemy instancję klasy wyjątków, która może być użyta na przykład do wyświetlenia komunikatu o błędzie, co jest obsługiwane przez metodę `getMessage()`. Przydatna jest również znajomość metody `getFile()`, która zwraca ścieżkę dyskową do pliku zawierającego błąd, `getCode()`, która zwraca kod statusu błędu, oraz `getLine()`, która zwraca numer linii, w której został rzucony wyjątek.

Przygotowane wyjątki
------------------------

Oprócz podstawowego wyjątku `Exception`, PHP zawiera inne predefiniowane typy wyjątków, które są odpowiednie dla różnych przypadków użycia.

| Typ danych | Objaśnienie |
|------------|-----------|
| Błąd logiczny, przewidywalny przy projektowaniu programu.
| `BadFunctionCallException` | Błąd wywołania funkcji; funkcja nie została znaleziona; wywołanie niedozwolone |
| `BadMethodCallException` | Błąd wywołania metody |
| `InvalidArgumentException` | Zły (nieprawidłowy) argument przekazany do funkcji lub metody |
| Wyjątek `OutOfRangeException` | Wartość spoza zakresu tablicy lub kolekcji |
| Wartość przekracza dozwoloną długość |
| Wartość nie mieści się w wymaganej domenie lub zakresie |.
| `RuntimeException` | Błąd wykrywalny tylko w czasie pracy (np. brak możliwości zapisu do pliku) |
| `OverflowException` | Przepełnienie bufora lub operacji arytmetycznych, często spowodowane przetwarzaniem większej ilości danych niż oczekiwano |
| | niedopełnienie bufora lub operacji arytmetycznej, przekazano mniej danych niż oczekiwano |
| Wyjątek `OutOfBoundsException` | Indeks poza zakresem tablicy lub kolekcji
| Wartość nie mieści się w żądanym zakresie.
| Wyjątek `UnexpectedValueException` | Nieoczekiwana wartość (np. wartość zwracana funkcji) |

Wyjątkom `LogicException` i `RuntimeException` należy zapobiegać poprzez odpowiednie zaprojektowanie programu. Osobiście używam ich tylko w wyjątkowych sytuacjach, takich jak brak możliwości zapisu do pliku czy komunikacja z usługą zewnętrzną.

Zalecam, aby w ogóle nie łapać wyjątków `RuntimeException` i pozwolić aplikacji zawieść. Jest to zazwyczaj poważny problem, który należy jak najszybciej zgłosić.
