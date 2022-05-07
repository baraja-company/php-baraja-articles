Specjalne znaczenie cudzysłowów
===============================

> id: '2649942d-94d6-48c3-9c78-5a303165bf72'
> slug:
> 	cs: uvozovky-vyznam
> 	pl: specjalne-znaczenie-cudzyslowow
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c1b63b98a3e9d9c642247c363747616a

W PHP często można zastąpić cudzysłów apostrofami, uzyskując ten sam efekt. Czasami nawet celowo używamy kombinacji cudzysłowów i apostrofów, aby uzyskać różne wyniki bez konieczności stosowania ucieczki.

**TLDR: Używanie cudzysłowów może być niebezpieczne w niektórych kontekstach, dlatego wszędzie należy używać apostrofów. Cudzysłów jest odpowiedni tylko w szczególnych przypadkach.**

| Postać | Nazwa |
|------|-----------
| `"` | Znak cudzysłowu |
| `'` | Apostrof |

Przykład pierwszy - ten sam wynik
-----------------------------

```php
echo "Witaj, świecie!"; // to jest to samo
echo 'Witaj, świecie!'; // jak wyżej
```

W tym przypadku użycie cudzysłowów lub apostrofów nie ma znaczenia. Na pierwszy rzut oka może się więc wydawać, że nie ma różnicy między cudzysłowem a apostrofem.

Kombinacja cudzysłowów i apostrofów
------------------------------

Rozważmy następujący kod:

```php
echo 'Mój ulubiony kolor to "czerwony"!';
```

Gdybym użył `carriage returns` do rozgraniczenia wypisywanego łańcucha, byłoby **niejednoznaczne**, gdzie łańcuch **się zaczyna**, a gdzie kończy**, więc użyłem `apostrofów` do rozgraniczenia łańcucha, co rozwiązuje ten problem.

Wstawianie cudzysłowów do łańcucha ograniczonego cudzysłowami
---------------------------------------------------

Czasami mogą zdarzyć się sytuacje, w których będę potrzebował wypisać zarówno `cytat`, jak i `apostrofę` w tym samym łańcuchu. Można używać nie tylko konkatenacji dwóch łańcuchów, ale także tak zwanego **escapingu** znaków.

```php
echo "Ten łańcuch zawiera cudzysłów.";
```

Odwrotny ukośnik w tym przypadku ma znaczenie "dokładnie ten znak" i dlatego cudzysłów nie będzie postrzegany jako koniec łańcucha (ani niczego innego), a więc zawsze będzie cudzysłowem. Inne dziwne znaki mogą być oznaczane w ten sposób, gdy nie można zagwarantować ich trwałości, a właściwe znaczenie może zostać źle zrozumiane.

Zmienna w łańcuchu znaków
-----------------------

Znaki cudzysłowu mogą być niezwykle podchwytliwe, ponieważ umożliwiają wstawianie zmiennych bezpośrednio do ciągu znaków:

```php
$color = 'Czerwony';

echo "Moje oblíbená barva je {$color}, a tvoje?";
```

Dużo bezpieczniejsze są apostrofy, które nie pozwalają na to i trzeba zaginać sznurek:

```php
$color = 'Czerwony';

echo 'Mój ulubiony kolor to' . $color . 'a ty?';
```

Dobre nawyki
--------------------------

Ogólnie rzecz biorąc, zalecam używanie apostrofów (jeśli to możliwe) do rozgraniczania czegokolwiek, ponieważ są one znacznie rzadziej spotykane w łańcuchach niż cudzysłów.

Co więcej, PHP jest językiem sieciowym, tzn. służy do generowania dokumentów HTML, w których cudzysłów jest bardzo powszechny właśnie dlatego, że jest on również używany do generowania części kodu HTML. Osobiście polecam przyzwyczajenie się do bezwzględnego używania apostrofów wszędzie, ponieważ wtedy nie trzeba pamiętać, co się obejmuje.

Zachowanie znaku cudzysłowu
--------------------------

Uważaj! Nie wyrzucaj całkowicie cudzysłowu! Mają one pewne szczególne zalety, które mogą być przydatne w zaawansowanej pracy z PHP - jednak początkujący traktują je jako błędy i nie rozumieją ich.

```php
$x = 10; // ustaw zmienną
echo "Hodnota proměnné je: $x, přesně."; // oraz zestawienie
```

W cudzysłowach można też podawać wartości zmiennych, a znak dolara powoduje, że wszystko, co się po nim znajduje, staje się zmienną. Jeśli więc nie chcesz podawać wartości zmiennej, lecz znak dolara, musisz uciec.

```php
$price = 25; // cena w dolarach
echo "Cena produktu: $price\$"; // prints "Cena produktu: 25$"
```

Zaleta cudzysłowu jest w tym przypadku wątpliwa i być może lepiej będzie użyć apostrofów i po prostu połączyć te ciągi.

```php
$price = 25;
echo 'Cena produktu:' . $price . '$'; // wypisuje to samo, co poprzedni przykład
```

> **Uwaga:** Cena w dolarach jest poprawnie zapisana w formacie `$25`, co spowodowałoby jeszcze większe zamieszanie, ponieważ notacja `$cena` w rzeczywistości wywołuje coś, co nazywamy `zmienną` (w nazwie zmiennej podajemy nazwę zmiennej, która ma być wywołana - ot, takie zamieszanie).

**TIP:** Możesz być zainteresowany <a href="/promenna-zmienna">zmienną zmienną</a>.

Oznaczenie łańcuchowe
--------------------------

Ogólnie rzecz biorąc, wszystko, co jest ujęte w cudzysłów lub apostrofy, jest traktowane jako łańcuch. Tak więc:

```php
$x = 5;   // to jest coś innego,
$x = "5"; // niż to.
```

W pierwszym przypadku **liczba** 5 jest przechowywana w zmiennej **$x**, w drugim przypadku **łańcuch** "5" jest przechowywany w tej samej zmiennej. Na szczęście w PHP nie ma to znaczenia i możesz pracować z każdym z wariantów (prawie) w ten sam sposób, ponieważ PHP potrafi automatycznie **przekształcać** zmienne zgodnie z ich zawartością. Na ogół jednak nie zaleca się pisania liczb w cudzysłowie, zwłaszcza w przypadku operacji obliczeniowych, w których mogą wystąpić błędy zaokrąglenia.

Czasami mogę chcieć zapisać wynik działania funkcji w zmiennej:

```php
$pi = 3.14159;
$zaokrouhleni = round($pi); // tak jest poprawnie
$zaokrouhleni = "round($pi)"; // to jest "złe" (pytanie brzmi, jakich danych wyjściowych oczekuję).
```

W drugim przypadku błąd znajduje się tylko w cudzysłowie, gdy wynik działania funkcji **round()** nie jest przechowywany w zmiennej **$round**, lecz w łańcuchu wywołującym tę funkcję.
> **TIP:** Wartość `$pi` nie musi być wprowadzana bezpośrednio do skryptu w ten sposób i możemy użyć funkcji `pi()`, która jest dokładniejsza w przypadku bardziej złożonych obliczeń.

Specjalne znaczenie ucieczki
--------------------------

> **Ostrzeżenie:** Poniższe przykłady działają tylko w cudzysłowach, jeśli zawrzesz je w apostrofach, będą zachowywać się jak zwykłe znaki bez specjalnego znaczenia (z wyjątkiem ``, który ucieka od apostrofu).
Ucieczka służy do wyprowadzenia jakiegoś znaku specjalnego wewnątrz cudzysłowu lub apostrofu, który mógłby zostać zinterpretowany jako wyrażenie językowe, a więc przetworzony, nawet jeśli programista nie miał takiego zamiaru. Przedstawiliśmy już przykład; w tej części opisano możliwe wyjątki w zachowaniu.

Dzieje się tak dlatego, że czasami sama ucieczka ma specjalne znaczenie. Przykład:

```php
echo "Tekst długi, podzielony na dwa wiersze.";
```

W poprzednim przykładzie wydrukowano:

```php
Dlouhý text, rozdělený na
dva řádky.
```

Jeśli więc chcemy wypisać ukośnik, musimy go również wymazać (wymykanie znaku `n` nie jest w tym przypadku konieczne, ponieważ zostałby on zrozumiany jako ponowne zawinięcie, lub w tym przypadku nie musimy go w ogóle wymykać):

```php
echo "Tekst długi, podzielony na dwa wiersze.";
```

Takich znaków specjalnych jest więcej, na przykład `t` tworzy tabulator. Pełna lista znajduje się w oficjalnej dokumentacji.

Rzeczywiste użycie znaków specjalnych w ucieczce
-----------------------------------------------

Na wstępie chciałbym zaznaczyć, że w PHP można zrobić prawie wszystko na wiele sposobów, a podane tu przykłady mają jedynie zilustrować, jak można podejść do problemu.

Na przykład, jeśli chcemy przetworzyć tekst wiersz po wierszu, możemy użyć funkcji <a href="/explode">explode</a>.

```php
$parser = explode("\n", $retezec); // parsuje tekst linia po linii
```

W tym przypadku użycie znaku specjalnego `n` ma sens, ponieważ możemy bardzo sprawnie powiedzieć, że chcemy wykonywać parsowanie według przerw między wierszami.

```php
$parser = explode('
', $retezec);
```

> **Ostrzeżenie:** W systemach Unix i Windows znaki używane do oznaczania końca linii są mylone. Na przykład, Windows używa `CRLF` (para znaków `CRLF`), podczas gdy Linux używa tylko `LF` (pojedynczy znak `LF`). Należy o tym pamiętać przy przetwarzaniu danych w poszczególnych wierszach. Zwykle problem ten rozwiązuje się przez normalizację znaków tylko do `LF`.

Streszczenie
-------

Jeśli możesz, używaj wszędzie **apostrofów**.

Dobrze jest znać zasady stosowania cudzysłowu i używać go tylko tam, gdzie jest to konieczne (lub ogólnie dobre). W przypadku wypisywania tekstu, który może zawierać cudzysłów, należy go ująć w apostrofy (które zachowują się wtedy bardziej przewidywalnie). Osobiście używam cudzysłowów do wyrażania różnych znaków specjalnych, których wprowadzanie w apostrofach jest niepotrzebnie skomplikowane i wymagałoby skomplikowanej ucieczki.
