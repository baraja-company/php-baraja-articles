Cykle i ich typy w PHP
======================

> id: e2927790-d0fb-4de5-9848-01fdd088464b
> slug:
> 	cs: cykly
> 	pl: cykle-i-ich-typy-w-php
> 
> perex:
> 	- 'Cykl for, while, foreach a rekurze v PHP + vysvětlení rozdílů mezi cykly. Možnosti iterativního zpracování úloh.'
> 	- 'Cykle for, while, foreach i rekurencja w PHP + wyjaśnienie różnic między cyklami. Możliwości iteracyjnego przetwarzania zadań.'
> 
> publicationDate: '2020-04-11 18:56:34'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '297931d6e7b8c9e011dbbe06b414cdde'

Pętle w ogólności służą do powtarzania tego samego fragmentu kodu (zwykle nad zbiorem danych) Dla każdego typu zadania odpowiedni jest inny typ pętli, który różni się głównie znaczeniem i składnią. Należy również zauważyć, że wszystkie typy pętli są wzajemnie wymienialne, tylko nie zawsze jest to opłacalne z punktu widzenia złożoności kodu i czasu obliczeń.

Podstawowy podział pętli ze względu na typ elementu:

- `for`: Ogólna pętla, w której z góry znamy liczbę powtórzeń lub możemy ją po prostu z góry obliczyć,
- `while` i `until-while`: Ogólna pętla, w której nie znamy z góry liczby powtórzeń,
- `foreach`: Przeglądanie tablic i obiektów,
- Rekursja: Rozkładanie złożonego problemu na zbiór mniejszych.

Ogólne właściwości pętli
-----------------------

Cykle działają na ogół przez powtarzanie zaznaczonego fragmentu kodu **w kółko**, **dopóki spełniony jest warunek zakończenia**. Pętla jest zatrzymywana albo przez niespełnienie warunku zakończenia, albo przez ręczne zatrzymanie jej przez wywołanie `break;`.

Jeśli nic nie zatrzymuje pętli, to nic nie stoi na przeszkodzie, aby działała ona w nieskończoność, aż do momentu ręcznego zamknięcia aplikacji lub wyczerpania limitu czasu przetwarzania skryptu PHP (zwykle 30 sekund).

**Ważne terminy:**

- `Cycle` - wyrażenie językowe pozwalające na powtarzanie określonej części kodu
- Iteracja" - jedno określone wykonanie ciała pętli
- Inicjalizacja" - początek wykonywania pętli (przed rozpoczęciem pierwszej iteracji)
- `Increment` - inkrementacja zmiennej iteracyjnej o jeden
- Warunek wyjścia - warunek, który jest sprawdzany w każdej iteracji (miejsce sprawdzania zależy od typu pętli). Pętla jest wykonywana tak długo, jak długo spełniony jest warunek

Dla pętli
----------

Opcja `Dla` jest przydatna w przypadkach, gdy liczba powtórzeń jest z góry znana (lub może być łatwo obliczona). Doskonale nadaje się na przykład do liniowego przemierzania przedziałów.

Zazwyczaj składa się z 3 części:

```php
for (inicializace; výraz; inkrementace) {
    // ciało cyklu
}
```

- Inicjalizacja`: określenie stanu początkowego pętli (jakie zmienne są nam potrzebne),
- `wyrażenie`: Warunek. Dopóki jest ona ważna, pętla będzie wykonywana,
- `increment`: zmiana stanu pętli z poprzedniego przebiegu (`increment` - inkrementacja o jeden).

Przykładem może być wyświetlenie serii liczb od `0` do `10`:

```php
for ($i = 0; $i <= 10; $i++) {
    echo $i . '<br>.';
}
```

Lub wielokrotności dwójki od `2` do `100`:

```php
for ($i = 2; $i <= 100; $i += 2) {
    echo $i . '<br>.';
}
```

Pętla for jest przydatna do różnych sztuczek, takich jak <a href="/abeceda-cisla-intervals">uzyskanie alfabetu, tablicy liczb i interwałów</a>.

Podczas gdy pętla do-while
-----------------------

Pętla `While` działa tak długo, jak długo spełniony jest warunek. Różnica między `while` a `do-while` polega na tym, kiedy warunek zostanie obliczony.

```php
while (výraz) {
    // ciało cyklu
}
```

Alternatywnie:

```php
do {
    // ciało cyklu
} while(výraz);
```

- Polecenie `while` najpierw oblicza warunek, a następnie uruchamia wewnętrzną pętlę,
- `do-while` najpierw przetwarza wewnętrzną pętlę, a następnie oblicza warunek.

Jest to szczególnie korzystne, gdy nie możemy z góry przewidzieć, czy pętla zostanie kiedykolwiek wykonana i chcemy zagwarantować, że zawsze zostanie wykonana przynajmniej raz (wtedy używamy `do-while`).

Przykład:

> W zmiennej `$x = 2` mamy zapisaną pewną liczbę. Chcemy również wygenerować liczbę z przedziału `<0; 10>` w zmiennej `$y` tak, aby była ona różna od liczby w zmiennej `$x`. Prosto mówiąc: Wygeneruj liczbę losową z przedziału `0` do `10`, która nie jest `2`.

W takim przypadku wygodnie jest użyć pętli `do-while`. Nie wiemy z góry, ile razy pętla będzie musiała zostać uruchomiona (możemy mieć pecha i trafiać ciągle na tę samą liczbę, a wtedy pętla będzie działać długo), a ponadto chcemy zagwarantować, że zostanie ona uruchomiona przynajmniej raz, zanim warunek zostanie spełniony, ponieważ najpierw musimy wygenerować liczbę.

```php
$x = 2;
$y = $x;

do {
   $y = rand(0, 10);
} while($x == $y);

echo 'X :' . $x . '<br>.';
echo 'Y:' . $y;
```

Foreach
-------

`foreach` jest idealny do przeglądania pól i obiektów. Możemy uzyskać nie tylko określone wartości, ale także klucze.

Jeśli chcemy uzyskać tylko określone wartości:

```php
foreach ($data as $value) {
    // przetwarzanie danych
}
```

Możemy też zdobyć klucze:

```php
foreach ($data as $key => $value) {
    // przetwarzanie danych
}
```

Polecenie `foreach` jest idealne do przeglądania rzeczywistych danych - na przykład wyników z bazy danych lub pól z kluczami:

```php
$countries = [
    'pl' => 'Republika Czeska',
    'Zob. .' => 'Słowacja',
];

foreach ($countries as $shortcut => $name) {
    echo $shortcut . ':' . $name . '<br>.';
}
```

Rekursja
-------

Rekursja występuje wtedy, gdy funkcja lub metoda wywołuje samą siebie. Służy do rozbicia złożonego problemu na grupę mniejszych.

> Zrozumienie rekurencji może być trudne dla początkujących, ponieważ jest ona oparta na idei przekazywania odpowiedzialności.
>
> Funkcja faktycznie mówi coś w stylu "Nie potrafię rozwiązać tego problemu, ale znam kogoś, kto potrafi...", więc dzwoni do niego, on dzwoni do kogoś, ... dopóki nie zostanie wywołany ostatni człon, co spowoduje rozwiązanie problemu.

Warto zauważyć, że każdy algorytm rekurencyjny można przepisać tak, aby używał pętli tam, gdzie rekurencja nie jest potrzebna (prawdą jest też odwrotna zależność). Rekursja jest dobrym sługą, ale złym panem. Pomaga to rozwiązywać niektóre typy problemów w sposób prosty i bardzo wydajny, natomiast pętle po cyklach są przydatne w innych sytuacjach.

Ogólnie rzecz biorąc, rekurencja jest pamięciożerna, ponieważ cały czas tworzy nowe instancje i konteksty funkcji i metod. Jest to jednak jedyny rozsądny sposób rozwiązania problemu, na przykład w przypadku przechodzenia przez struktury drzewiaste.

Przykład:

Musimy obliczyć czynnik potęgi liczby `5`, tak to się robi w matematyce:

`5! = 5 * 4 * 3 * 2 * 1`

To znaczy, że istnieje kolejne mnożenie serii liczb od `1` aż do liczby, której faktorem jesteśmy zainteresowani.

Można to bardzo elegancko rozwiązać w sposób rekursywny:

```php
function factorial(int $n): int
{
   if ($n === 0) {
      return 1;
   }

   return $n * factorial($n - 1);
}
```

Alternatywą jest jeszcze krótsza implementacja z użyciem operatora trójskładnikowego:

```php
function factorial(int $n): int
{
    return $n === 0 ? 1 : $n * factorial($n - 1);
}
```

To samo można przepisać, aby wykorzystać cykle:

```php
function factorial(int $n): int
{
    $result = 1;

    for ($i = $n; $i > 0; $i--) {
        $result *= $i;
    }

    return $result;
 }
```

Zapisywanie z cyklem jest nieco dłuższe, ale ponownie ma znacznie mniejszy ślad w pamięci. Do obliczeń używamy tylko jednej zmiennej `$result`, której wartość zmieniamy w sposób ciągły i nie musimy tworzyć nowych instancji `factorial()`.

Bardzo ostrożnie pisz pętle nieskończone
-----------------------------------

Bardzo łatwo jest doprowadzić do sytuacji, w której pętla staje się nieskończona (osiągamy to, nie spełniając nigdy warunku zakończenia). Powinieneś wziąć to pod uwagę i ewentualnie samemu zatrzymać pętlę za pomocą `break;`.

Na przykład, turlaj kość aż do uzyskania liczby `6`. Implementacja kusi, aby użyć pętli `while`:

```php
while (true) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'Wartość spadła' . $value;
      break;
   }
}
```

Pisząc `while(true)` zapewniliśmy sobie nieskończoną liczbę powtórzeń, ponieważ `true` zawsze będzie prawdą. Musimy więc sami zatrzymać pętlę za pomocą konstrukcji `break;`, ale co jeśli wartość `6` nigdy nie spada i nie zatrzymujemy pętli?

Osobiście zawsze liczę liczbę powtórzeń i jeśli przekroczy ona jakiś krytyczny limit, przymusowo przerywam pętlę. Na przykład, jeśli przekroczymy liczbę `1000` prób. W takim przypadku lepiej jest użyć `for` niż `while` i policzyć liczbę przebiegów.

Jeśli liczba przebiegów zostanie przekroczona, należy poinformować o tym programistę, rzucając wyjątek.

```php
for ($i = 0; $i <= 1000; $i++) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'Wartość spadła' . $value;
      break;
   }
}

if ($i === 1000) {
   throw new \Exception('Przekroczona została maksymalna liczba rzutów.');
}
```
