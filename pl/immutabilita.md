Niezmienność obiektu - kluczowa koncepcja projektowa
====================================================

> id: '057467db-4e3b-4e18-9ea5-dfb25feb3800'
> slug:
> 	cs: immutabilita
> 	pl: niezmiennosc-obiektu-kluczowa-koncepcja-projektowa
> 
> publicationDate: '2022-07-24 15:00:00'
> mainCategoryId: ae4c1c70-11b3-433e-b1d0-e590155bb8b9
> sourceContentHash: '88c26f35883c860426b4708f0be8761f'

Niezmienność jest jedną z najważniejszych koncepcji projektowych przy budowaniu stabilnych aplikacji. Podstawowa zasada mówi, że raz zapisany stan może być później tylko odczytany bez możliwości jego modyfikacji. Jeśli potrzebujemy zmienić stan, musimy stworzyć nową instancję i zastąpić cały obiekt innym.

Typy danych można zatem bardzo zgrubnie podzielić na dwie szerokie kategorie:

- **Mutable** (stan zmienny w obrębie jednej instancji)
- **Immutable** (niezmienny stan wewnętrzny)

Obiekty mutowalne mogą być zmieniane wewnętrznie. Czyli dostarczają operacji, które wywołane w różnych kombinacjach powodują, że otrzymujemy różne wyniki. Immutability próbuje zapobiec temu zachowaniu.

Definicja
--------

> Klasa jest **immutable** właśnie wtedy, gdy dane instancji nie mogą być w żaden sposób zmienione po utworzeniu instancji.

Tak więc wszystkie dane są ustalone w konstruktorze. Wszystkie skalarne typy danych są również automatycznie niezmienne.

Duża zaleta
--------------

Projektowanie aplikacji z niezmiennymi stanami zapewnia fundamentalną przewagę w zakresie bezpieczeństwa wykonywania operacji. Jeśli wiemy, że raz zapisane dane nie mogą być później zmienione (zmutowane), to możemy np. bardzo rzetelnie debugować lub podzielić aplikację na podfunkcje bez ryzyka zapomnienia któregoś ze stanów pośrednich.

Idea niezmienności jest ogólnie przeciwna zasadzie przechowywania stanów we właściwościach w obiektach/podmiotach i raczej opisuje podejście funkcjonalne, w którym dane po prostu "przepływają" przez aplikację, tak jak robi to na przykład javascript.

Z perspektywy wydajności możemy automatycznie powiedzieć o niezmiennych obiektach, że mogą być buforowane w nieskończoność, ponieważ nigdy nie będą przestarzałe.

Prawdziwy przykład z PHP
--------------------

Zdecydowanie najczęstszym zastosowaniem obiektów niezmiennych w PHP jest obiekt `DateTimeImmutable`, który raz utworzony może być wywoływany tylko przez metody formatujące. Jeśli zmodyfikujemy wewnętrzne ustawienia, metoda zwróci nową instancję. Cecha ta jest kluczowa w przypadku korzystania z ORM-a wykorzystującego tzw. wzorzec tożsamości - pozwala nam np. zagwarantować, że gdy odczytamy datę utworzenia zamówienia, będzie ona taka sama wszędzie w aplikacji i integralność referencyjna nie zostanie naruszona.

Konkretny przykład obiektu mutowalnego:

```php
$date = new DateTime('2021-05-14');
$tomorrow = $date->modify('+1 dzień');
echo $date->format('Y-m-d'); // 2021-05-15
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

Ta sama data została wydrukowana, ponieważ metoda `modify()` zmieniła tylko wewnętrzny stan obiektu `DateTime` i zwróciła tę samą instancję. Nie było więc tzw. **mutacji stanu wewnętrznego**, która jest podstawowym zachowaniem programowania obiektowego. Aktualizacja zmiennej spowodowała również zmianę oryginalnej.

A teraz przykład niezmiennego obiektu:

```php
$date = new DateTimeImmutable('2021-05-14');
$tomorrow = $date->modify('+1 dzień');
echo $date->format('Y-m-d'); // 2021-05-14
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

Obiekt `DateTimeImmutable` jest niezmienny, co oznacza, że jego stan wewnętrzny nigdy się nie zmienia. Nowa zmodyfikowana instancja (również niezmienna) jest przechowywana w zmiennej po wywołaniu metody `modify()`. Gdybyśmy nie zapisali nowej wartości w zmiennej, nie dałoby się jej później wykorzystać.

Oryginalna wartość nigdy nie jest dotykana i pozostaje bezpiecznie przechowywana.

Kiedy klasa powinna być niezmienna?
---------------------------

O ile nie masz bardzo dobrego powodu, aby uczynić go mutowalnym, zawsze pisz klasę lub funkcję jako niezmienną. To uprości twój projekt w przyszłości.

Klasy mutowalne powinny zmieniać się w jak najmniejszym stopniu. Zawsze polecam dokumentowanie zachowania niezmienności.

Być może jedyną wadą niezmienności jest to, że nowa instancja musi zostać utworzona przy każdej zmianie atrybutu, co ma niewielki wpływ na wydajność. Jeśli Twoja aplikacja (jak większość aplikacji) ma tendencję do wyświetlania danych i ich rzadszej zmiany, ta wada jest raczej nieistotna przy wydajności dzisiejszych komputerów.

Jakie typy danych powinny być niezmienne?
------------------------------------

- Wszystkie identyfikatory i niepowtarzalne kody
- Większość sesji baz danych ManyToOne i OneToOne
- Daty, godziny, wartości kalendarza
- Cykliczne przechodzenie przez tablice, gdzie chcemy wykonać tę samą operację na każdym elemencie
- Interwały, pary, trójki, ...
- Figury geometryczne, punkty, linie, współrzędne GPS, adresy fizyczne, ...
- Dzienniki i zapisy historyczne
- Informacje o zrealizowanych zamówieniach i większość danych finansowych
- Meta dane o powiązanym podmiocie

Co nie powinno być niezmienne:

- Duże obiekty z dużą ilością właściwości
- Większość tabelarycznych danych wyjściowych z bazy danych, takich jak encje Doctrine
- Stopniowo budowane obiekty z mniejszych części
