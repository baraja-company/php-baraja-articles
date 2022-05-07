Wzorce projektowe w PHP
=======================

> id: c22b3e54-bb66-4196-a998-f4b62b9308b2
> slug:
> 	cs: navrhove-vzory
> 	pl: wzorce-projektowe-w-php
> 
> publicationDate: '2020-02-13 10:32:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '9ee32f0a3ce55bad52725f45fcd1c041'

Wzorce projektowe to sposoby myślenia o programowaniu.

Stanowią one zbiór porad, gotowych rozwiązań, najlepszych praktyk i spostrzeżeń na temat rozwoju. Dla każdego paradygmatu programowania i typu zadania istnieją pewne wzorce projektowe, które są najodpowiedniejsze.

Korzyści - dlaczego warto stosować wzorce projektowe?
---------------------------------------

W programowaniu rozwiązywanie pewnych typów problemów jest powtarzalne, więc sensowne jest wybranie jednej metody rozwiązywania tych problemów i powtarzanie tej metody.

Główna korzyść pojawia się zwłaszcza podczas pracy zespołowej, gdy wszyscy wiedzą, jak będzie tworzona aplikacja (według jakiego wzorca projektowego) i po prostu go stosują. Eliminuje to niepotrzebne dziesiątki godzin spędzonych na debugowaniu dziwnie napisanego kodu i próbach zrozumienia zasad, którymi kierował się autor.

MVC - przykład zastosowania wzorca projektowego
--------------------------------------

Moim ulubionym wzorcem projektowym jest `MVC` (od `Model View Controller`), który mówi, że aplikacja jest podzielona na 3 niezależne warstwy, które wywołują się nawzajem sekwencyjnie i przekazują sobie dane.

Na przykład, podczas renderowania strony, może to wyglądać tak, że najpierw decyduje ona, jakiego typu jest to strona (na przykład szczegóły kategorii), więc wywołuje `CategoryController` z metodą `detail`.

Konkretny przykład (bardzo upraszczam):

```php
class CategoryController
{
    public CategoryManager $categoryManager;

    public function actionDetail(string $id): void
    {
        $this->template->id = $id;
        $this->template->category = $this->categoryManager->getById($id);
    }
}
```

> **Uwaga:**
>
> To jest tylko przykładowy kod, który wyjaśnia zasadę działania wzorca projektowego `MVC`.
>
> W prawdziwej implementacji musielibyśmy dalej kombinować, jak, na przykład, uzyskać instancję `CategoryManager` i jak przekazać ją do właściwości. Zazwyczaj do tego typu zadań stosuje się `wstrzykiwanie zależności`.

Przed wyrenderowaniem strony dla szczegółów kategorii, najpierw wywoływany jest `CategoryController`, który odbiera faktyczne żądanie (tzn. renderujemy szczegóły kategorii o określonym ID, które jest uzyskiwane przez router, na przykład w adresie URL), pobiera dane (odpytując odpowiedni `Model`) i przekazuje ostateczne dane do szablonu w celu wyrenderowania.

Ogromną zaletą tej zasady jest możliwość pisania wielu modeli (logiki aplikacji), które są niezależne od sposobu prezentacji danych (szablonu), co daje w efekcie kod wielokrotnego użytku. W rzeczywistości, jeśli chcemy użyć `CategoryManager` w innym projekcie, po prostu przepuszczamy dane przez `Controller` w określony sposób, który zostanie wyrenderowany zgodnie z szablonem zdefiniowanym przez sam projekt, podczas gdy logika aplikacji pozostaje taka sama i nikogo to nie obchodzi, ponieważ warstwa oprogramowania wypełniła swój uzgodniony interfejs i obowiązki.

> **Uwaga praktyczna:**
>
> Wzorzec projektowy `MVC` jest używany przez większość nowoczesnych frameworków, takich jak Nette, Symfony, Laravel i inne.
>
> Z `MVC` możemy się również spotkać przy tworzeniu aplikacji mobilnych i innych rodzajów oprogramowania, w których musimy pobierać dane i renderować je w szablonie zgodnie z typem strony lub widoku.

Ważne wzorce projektowe dla tworzenia stron internetowych
---------------------------------------

Generalnie w programowaniu istnieje wiele wzorców projektowych, które nie nadają się do tworzenia stron internetowych. Na tej liście opisano najważniejsze wzorce, z których sam korzystam i które powinieneś znać.

Na osobnej stronie można znaleźć <a href="/categories-design-patterns">pełną listę wszystkich wzorców projektowych</a>, przykłady ich użycia oraz szczegółowe wyjaśnienia.

- **MVC** - Zasada podziału aplikacji na `Model` (logika aplikacji i dane), `Widok` (szablon i widok danych) oraz `Kontroler` (łączący `Model` i `Widok`).
- **Wstrzykiwanie zależności** - zamiast tworzyć instancje klas, definiujemy tzw. usługi, które automatycznie wstrzykujemy. Kod uwzględnia wszystkie zależności, poszczególne części mogą być wymieniane, a aplikacja jest budowana dynamicznie.
- **Singleton (Singleton)** - Każda klasa ma tylko jedną instancję (osobiście używam tego w połączeniu z `Dependency injection`, gdzie każda usługa ma tylko jedną instancję, która jest przekazywana przez całą aplikację).
- **Luźna inicjalizacja (opóźniona inicjalizacja)** - instancję obiektu tworzymy, gdy jest ona potrzebna po raz pierwszy.
- **Adapter** - Jeśli musimy zapewnić komunikację między dwiema niekompatybilnymi klasami, `Adapter` konwertuje dane z jednego typu na drugi (zazwyczaj konwertuje natywne typy danych PHP na typy danych bazy danych i z powrotem).
- **Polecenie** - Zamiast bezpośrednio wykonywać określone zadanie (np. wysłać wiadomość e-mail), po prostu pamiętamy, że miało się ono odbyć. Następnie inny lub ten sam proces przejmie wniosek i zajmie się jego przetwarzaniem. Główną zaletą jest to, że możemy przetwarzać aplikację leniwie (a więc bardzo szybko), ponawiać próby nieudanych działań i po prostu przechowywać parametry wywołania. Jednocześnie pozwoli nam to odbierać żądania od różnych klientów, za pośrednictwem interfejsów API itp. Wysłane działania można również umieszczać w kolejce, nadawać im priorytety i ewentualnie anulować przed oceną.
- **Strategia** - Obejmuje pewien rodzaj algorytmów lub obiektów, które mają być zmieniane, tak aby były zamienne dla klienta.

Istnieje wiele innych wzorców projektowych, ale te są najważniejsze, które powinieneś znać.

Antywzór
------------

Niektóre techniki programowania są uważane za `antypattern`, co jest dokładnym przeciwieństwem wzorca projektowego. Zwykle jest to technika, która tworzy dziwny kod, którego nie da się łatwo debugować, utrzymywać i który zachowuje się `magicznie`.

Typowym przykładem jest użycie <a href="/global-variable">zmiennych globalnych</a>.
