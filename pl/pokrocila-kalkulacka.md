Kalkulator w PHP: przetwarzanie wyrażenia matematycznego jako łańcucha znaków
=============================================================================

> id: e6798758-031d-4e7c-b24e-1f77cf61558d
> slug:
> 	cs: pokrocila-kalkulacka
> 	pl: kalkulator-w-php-przetwarzanie-wyrazenia-matematycznego-jako-lancucha-znakow
> 
> publicationDate: '2020-02-16 17:07:38'
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e
> sourceContentHash: '63f5336bf2bbabe5312121b57e1ce34e'

Wyobraź sobie, że stoisz przed zadaniem przetworzenia prostego przykładu matematycznego, który użytkownik wprowadza jako ciąg tekstowy do pola wyszukiwania. Zazwyczaj użytkownik chce wykonać prostą operację numeryczną na liczbach. W tym artykule opisano proces myślowy i konkretne instrukcje, jak to zrobić.

Implementacja naiwna
-------------------

Przez długi czas zastanawiałem się, czy proste wyrażenie matematyczne można przetworzyć za pomocą jakiejś sztuczki, tak aby kod był jak najkrótszy... i po wielu latach znalazłem rozwiązanie.

Podane rozwiązanie należy traktować **tylko jako przykład**, ponieważ jest ono **wyjątkowo niebezpieczne** i nieuczciwy użytkownik może łatwo podlinkować ciąg znaków, co spowoduje np. usunięcie całej aplikacji lub kradzież bazy danych!

```php
// Zapytanie użytkownika
$query = '5 + 3 * 2';

// Przetwarzanie wyrażenia jako regularnego kodu PHP
eval('$result = @(' . $query . ');');

// Wypisanie zmiennej z rozwiązaniem wyrażenia
echo $result; // drukuje 11
```

Sztuczka polega na tym, że funkcja <a href="/function-eval">eval()</a> wykonuje łańcuch znaków tak, jakby znajdował się on w kontekście kodu PHP. To szalone, ale działa. Owijka wyłącza komunikaty o błędach.

Radzenie sobie z bardziej złożonymi danymi wejściowymi
--------------------------

Oprócz tego, że **przetwarzanie wyrażeń za pomocą eval() jest bardzo niebezpieczne**, nie zapewnia ono również wystarczająco elokwentnej składni, która odpowiadałaby każdemu. Jeśli użytkownik popełni choćby jedno naruszenie składni, całe wyrażenie nie będzie mogło zostać przetworzone.

Dlatego rozwiązaniem jest najpierw **zrozumienie** i poprawienie zapytania użytkownika zgodnie z aspektem formalnym (tzw. normalizacja do postaci kanonicznej), a następnie przekazanie i przetworzenie go dalej.

W przeszłości zaprogramowałem [QueryNormalizer](https://github.com/mathematicator-core/engine/blob/master/src/QueryNormalizer.php) dokładnie do tego zadania.

Samo przetwarzanie jest bardzo trudnym zadaniem, ponieważ trzeba prawidłowo zrozumieć różne konteksty. Na przykład, że nawiasy oznaczają bloki zagnieżdżone i muszą być obliczane rekursywnie. Na przykład, wyrażenie `5+2^(1+3/2)` nie może być rozwiązane w prosty sposób, ponieważ najpierw należy rozwiązać ułamek, dodać go do liczby w nawiasie, następnie rozwiązać go dla potęgi liczby całkowitej, a na końcu dodać na poziomie podstawy.

Aby spełnić to wymaganie, nie można już traktować wyrażenia jako zwykłego ciągu znaków i trzeba **przejść na wyższy poziom abstrakcji**. W istocie matematyka jest rodzajem języka, który opisuje relacje między operacjami i liczbami, ponieważ mamy do czynienia z priorytetami operatorów, różnymi znaczeniami, kontekstami, rekurencją, a nawet typami danych. W tym właśnie celu stosuje się **proces tokenizacji zapytań**.

> Zajmuję się problemem tokenizacji matematyki od 2015 roku i od tego czasu napisałem kilka różnych parserów.

Najlepszym z nich, który obecnie zasila nowego Mathematicatora, jest [dostępny jako otwarte źródło w serwisie GitHub](https://github.com/mathematicator-core/tokenizer).

Celem tokenizacji jest **przekształcenie ciągu znaków**, podzielenie go na grupy mniejszych ciągów o znanych typach, a następnie przekształcenie ich w obiekty (typy danych). Przekształcona tablica obiektów jest następnie przekształcana przez **sprytną logikę w drzewo binarne**, które może opisywać zależności i rekurencję. Jest to bardzo wymagający proces, ponieważ istnieją setki możliwych scenariuszy, a użytkownicy mogą być bardzo kreatywni w swoich zapytaniach.

Główną zaletą tablicy tokenów jest to, że można ją bardzo łatwo przekazać do następnej warstwy, która na przykład [wykona właściwe obliczenia](https://github.com/mathematicator-core/calculator) lub [przerysuje drzewo do LaTeX-a](https://github.com/mathematicator-core/tokenizer/blob/master/src/TokensToLatex.php).

Sposób użycia może wyglądać w następujący sposób:

```php
$tokenizer = new Tokenizer(/* niektóre zależności */);

// Konwertowanie formuły matematycznej na tablicę tokenów:
$tokens = $tokenizer->tokenize('(5+3)*(2/(7+3))');

// Teraz można przekonwertować tokeny na bardziej użyteczny format:
$objectTokens = $tokenizer->tokensToObject($tokens);

dump($objectTokens); // Zwróć typowane tokeny z meta danymi

// Renderuj do LaTeX-a
echo $tokenizer->tokensToLatex($objectTokens);

// Renderuj do drzewa debugowania (bardzo szybko):
echo $tokenizer->renderTokensTree($objectTokens);
```

Wyświetl procedury
-----------------

Wielu użytkowników doceni to, że **podczas obliczeń program wyświetla procedurę**, aby pokazać, jak to zrobił. Jest to przydatne także dla programisty, ponieważ może on łatwo sprawdzić, gdzie w obliczeniach wystąpił błąd, i odpowiednio skorygować algorytm. Gdy połączymy to wszystko z uczeniem maszynowym opartym na testach automatycznych, otrzymamy coś niesamowitego.

Zobacz, jak `QueryNormalizer` był w stanie zrozumieć Twoje zapytanie, przekazać dane do tokenizera, ten z kolei wyrenderował zapytanie do LaTeX-a zgodnie z nim, a następnie przekazał drzewo obiektów do kalkulatora, który zwrócił wynik ogólny.

Příklad: [5+2^(1+3/2)](https://mathematicator.com/search/5%2B2%5E%281%2B3/2%29).

Reprezentacja procedury jest realizowana przez kalkulator przemierzający drzewo danych wejściowych i oceniający po kolei jedną regułę zgodnie z zawartymi w niej tokenami i regułami. Gdy dowolna reguła jest obliczana, informacje o kroku są umieszczane w tablicy. Czasami może się zdarzyć, że jakiś krok okaże się niepoprawny i trzeba będzie się cofnąć i obrać inną ścieżkę obliczeń, ale kryje się za tym sporo magii, która na razie pozostanie ukryta, a można ją poznać w implementacji.

Wniosek
-----

Powyższa procedura opisuje, jak w elegancki sposób obsługiwać wyrażenia matematyczne, w których występują liczby, operacje i relacje z nimi. W ten sposób nie można na przykład modyfikować wyrażeń ani rozwiązywać równań, ale tym zajmiemy się następnym razem.

*Jeśli masz inne pomysły na to, jak efektywnie przetwarzać matematykę, chętnie się z nimi zapoznam.
