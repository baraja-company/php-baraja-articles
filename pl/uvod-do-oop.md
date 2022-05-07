Wprowadzenie do programowania obiektowego w PHP
===============================================

> id: '44a79461-dcfd-4dd0-b6fd-ba5db76db6de'
> slug:
> 	cs: uvod-do-oop
> 	pl: wprowadzenie-do-programowania-obiektowego-w-php
> 
> publicationDate: '2020-01-01 19:54:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '888db2cf35845892eebade9fdcc18871'

Witamy w pierwszym artykule kursu online OOP w PHP. Pełna lista artykułów znajduje się na stronie <a href="/oop">strona przeglądowa</a>.

> **Informacje o treści:**
>
> Celem tej serii jest **najlepsze wyjaśnienie istoty** programowania obiektowego, tak abyś nie musiał spędzać setek godzin na eksperymentowaniu z rzeczami, które nie mają sensu. Wszystkie techniki i tutoriale pochodzą z praktyki i są wyjaśnione tak, jak sam chciałbym je przeczytać, gdy uczyłem się programowania. Niektóre **koncepcje są bardzo uproszczone** kosztem 100% poprawności merytorycznej, ponieważ uważam, że zawsze lepiej jest **zrozumieć głównie zasady** i mieć przynajmniej jakieś pojęcie o problemach, niż pogubić się w programowaniu i widzieć wszystko jako jeden wielki bałagan.
>
> Jak się wkrótce przekonasz, programowanie w języku OOP ma ogromne korzyści dla Twojego kodu. Zapewnia on nowy poziom abstrakcji aplikacji i umożliwia rozwiązywanie bardzo złożonych problemów, które nie byłyby możliwe do rozwiązania w inny sposób.

Czym są obiekty
------------------

W podstawowym pojęciu programowania obiekty to specjalne typy danych, które oprócz własnej wartości mogą definiować szczegółowy stan, metody, zachowanie i umożliwiają wykonywanie różnych operacji, podobnie jak w świecie rzeczywistym.

Obiekt w programowaniu to coś w rodzaju "rzeczy" ze świata rzeczywistego, którą możemy nazwać (np. rzeczownikiem) i która ma właściwości podobne do rzeczy ze świata rzeczywistego. Na przykład obiektem może być artykuł, który ma pewne wartości (tytuł, treść, autor, data publikacji, ...) i metody (wstaw nowy tytuł, przeczytaj treść, oblicz liczbę dni od publikacji, ...). W tym artykule zajmiemy się głównie tworzeniem obiektu i pracą z nim na poziomie podstawowym.

Obiekty i klasy
---------------

Jak już ustaliliśmy, **przedmiot to coś, co istnieje**. Słowo "istnieje" jest na tym etapie bardzo ważne, ponieważ - jak się wkrótce przekonamy - pozostaje jeszcze praktyczna implementacja, którą trzeba się zająć w programowaniu.

Ponieważ w programowaniu piszemy kod, a obiekt jest czymś "żywym", od tego momentu będziemy rozróżniać dwa różne pojęcia, dla których ważne jest szczegółowe zrozumienie znaczenia i ścisłe ich rozróżnienie:

- Obiekt** to coś "żywego", co istnieje w tej chwili (ma instancję) i może wykonywać jakieś działania lub reagować na inne obiekty.
- **klasa** to statycznie napisany kod, który nie został jeszcze obliczony i pozostaje taki sam przez cały czas.

Ponieważ podczas programowania zawsze piszemy kod jako statyczny ciąg znaków (tekst), będziemy rozróżniać programy na `klasy` (statyczna definicja całego obiektu) i `obiekty` (`żywa` instancja klasy).

Przykład definicji klasy:

```php
class Article
{
    public string $title;

    public ?string $autor = null;
}
```

> **Uwagi praktyczne:**
>
> W rzeczywistych aplikacjach każda klasa jest zwykle zapisywana w osobnym pliku PHP, który ma taką samą nazwę jak nazwa klasy.
>
> Tak więc dla klasy `Article` tworzymy na przykład `src/Article.php`. Konwencja ta nie jest wymagana w PHP, ale pomaga uczynić aplikację bardziej czytelną, dzięki czemu nie ma w niej zbyt wiele kodu w jednym miejscu.
>
> Każda klasa może istnieć co najwyżej raz w całej aplikacji (ma unikalną nazwę), ale może istnieć (prawie) nieskończenie wiele jej instancji (w zależności od pojemności pamięci RAM). Co więcej, pojedyncza klasa nie może być dzielona na wiele plików i musi być zdefiniowana w jednym miejscu na raz. Jeśli trzeba utworzyć wiele klas o tej samej nazwie, należy użyć **przestrzeni nazw**.
>
> Później pokażemy również, że dobrze skonstruowany kod pozwala na jego ponowne wykorzystanie w wielu projektach.

Ten kod definiuje klasę `Article` z dwiema właściwościami (`title` i `author`). Komentarze są ignorowane przez PHP i są używane tylko do statycznego sprawdzania typów (zazwyczaj odczytywane przez PhpStorm).

Kod:

```php
public string $title;
```

oznacza definicję `properties`, która jest właściwością klasy `Article`. Można to rozumieć w ten sposób, że `Article` jest rodzajem pola, a `title` jest jego indeksem, do którego możemy wpisać wartość. Każda właściwość musi mieć zdefiniowaną dostępność (`public`, `protected` lub `private`), później wyjaśnimy, co oznaczają poszczególne ustawienia. Jeśli nie wiadomo, którą dostępność wybrać w danym przypadku, należy umieścić `public`.

Instantowanie klasy i tworzenie obiektu
----------------------------------

Pojęcie `instancji` jest jednym z najtrudniejszych do zrozumienia, dlatego ważne jest, abyś przeczytał poniższe wiersze bardzo uważnie i zalecam, abyś uczciwie wypróbował wszystkie przykłady.

Jeśli nasza aplikacja ma już zdefiniowaną klasę w kodzie źródłowym, to tak naprawdę nie jest ona nigdzie używana i PHP o niej nie wie. Dzieje się tak, ponieważ OOP wprowadza zasadę hermetyzacji danych, co oznacza, że klasa ma wewnętrzny (lokalny) kontekst, który jest ważny tylko wewnątrz klasy. Dzieje się tak dlatego, że w programowaniu bezobiektowym byliśmy przyzwyczajeni do oceniania kodu zawsze w całości. Staraj się też myśleć o klasie jako o jakiejś formie funkcji lub zewnętrznie wywoływanego programu.

Teraz możemy utworzyć naszą pierwszą instancję, używając do tego celu operatora `new`.

```php
$myArticle = new Article;
$myArticle->title = 'Mój pierwszy artykuł'; // zapisuje wartość

echo $myArticle->title; // listy "Mój pierwszy artykuł"
```

Zwróć uwagę, że cały obiekt `Article` utworzony przez operator `new` umieszczamy w zmiennej `$myArticle`. Oznacza to, że obiekt jest w rzeczywistości `typem danych`, więc możemy go przenosić między zmiennymi i kontynuować pracę z nim.

Operator strzałki `->` jest używany do manipulowania obiektem. Jeśli mamy instancję danego obiektu (mamy zmienną z jego zawartością), możemy łatwo zapisywać wartości do jego wewnętrznych właściwości, odczytywać je lub zmieniać na różne sposoby za pomocą metod (o czym później).

**Instancja obiektu to utworzenie lokalnej, "żywej" kopii klasy w pamięci (zmiennej).

Tworzenie wielu instancji tej samej klasy w tym samym czasie
---------------------------------------------

Jak już mówiliśmy, każdy obiekt ma lokalny kontekst, który ma zastosowanie w jego wnętrzu. Dzięki tej zasadzie możemy tworzyć wiele niezależnych instancji tej samej klasy i dowolnie nimi manipulować. Możemy nawet tworzyć dynamiczną liczbę instancji i na przykład przechowywać je jako elementy tablicy. Możliwości są nieograniczone.

> **Ostrzeżenie:**
>
> Przy tworzeniu dużej liczby instancji (setki lub więcej) należy pamiętać o ilości zajmowanej pamięci, ponieważ PHP musi przechowywać informacje o wszystkich instancjach w pamięci RAM. W praktyce jednak problem przepełnienia pamięci z powodu wielu instancji nie występuje.

Przykład:

```php
$firstArticle = new Article;
$firstArticle->title = 'Mój pierwszy artykuł';

$secondArticle = new Article;
$secondArticle->title = 'O psie i kocie';

echo $firstArticle->title;  // listy "Mój pierwszy artykuł"
echo $secondArticle->title; // wypisuje "O psie i kocie".
```

Zwróć uwagę, że sposób wypisania właściwości `title` (`->title`) zależy od tego, z której instancji czytamy.

Streszczenie
-------

Pokazaliśmy, jak zdefiniować naszą pierwszą klasę i utworzyć jej instancję (obiekt), do której wpiszemy kilka wartości.

W następnej części wyjaśnimy pojęcie <a href="/methods-and-passing-input">konstruktora, metod i przekazywania danych wejściowych</a>.
