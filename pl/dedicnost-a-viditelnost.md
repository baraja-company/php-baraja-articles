Dziedziczność i widoczność w PPE
================================

> id: '71896a55-dcfd-4bb6-9f53-64f22b1514eb'
> slug:
> 	cs: dedicnost-a-viditelnost
> 	pl: dziedzicznosc-i-widocznosc-w-ppe
> 
> publicationDate: '2020-02-16 22:17:05'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '4584f3dcfdcfdc506d34a37c36fe6dd1'

Jedną z podstawowych właściwości programowania obiektowego jest **dziedziczenie** i <a href="/enkapsulacja">enkapsulacja</a>. Dzięki tym cechom można łatwo budować złożone logiki aplikacji, zachowując przy tym dobrą czytelność implementacji.

Zasada dziedziczenia
-------------------

Dziedziczenie wyraża to, że implementacja jednej klasy jest oparta na innej. W terminologii OOP mówimy o **descendencie** (klasa, która dziedziczy) i **ancestorze** (klasa, po której dziedziczymy).

Ogólnie rzecz biorąc, dziedziczenie polega na tym, że potomek otrzymuje wszystkie cechy przodka, albo przejmuje je dokładnie w takiej postaci, w jakiej miał je przodek, albo modyfikuje je na swój własny sposób, albo całkowicie je zastępuje i używa własnej implementacji.

Zastosowanie tego podejścia jest bardzo szerokie, a dziedziczenie jest wykorzystywane przez wiele <a href="/design-patterns">wzorców projektowych</a>.

Rzeczywiste wykorzystanie dziedziczenia - prezentery w aplikacji
--------------------

Dziedziczenie dobrze nadaje się do projektowania tzw. **prezenterów**, czyli specjalnego rodzaju klas reprezentujących logikę łączenia we wzorcu projektowym **MVC**.

Na przykład, mamy trzy strony `Strona domowa`, `Kontakt` i `Login`.

Podczas implementacji każdej strony duża część logiki będzie się powtarzać (np. przyjmowanie żądania, budowanie adresu URL, renderowanie szablonu i przesyłanie wynikowego kodu HTML). Dlatego wygodnie jest zaimplementować taką logikę w pojedynczym przodku i używać jej w potomkach.

Zaczynamy od zdefiniowania przodka (nazwa klasy nie ma znaczenia, używam konwencji z frameworka Nette):

```php
abstract class BasePresenter
{
   public function link(string $route, array $params = []): string
   {
      // implementacja metody budowania adresu URL
   }

   public function renderTemplate(string $path, array $params = []): string
   {
      // logika renderowania szablonu
   }
}
```

Podczas definiowania klasy użyłem nowego słowa kluczowego `abstract`, które mówi, że klasa `BasePresenter` jest abstrakcyjna. Oznacza to, że nie możemy utworzyć jego instancji, a jedynie użyć go w taki sposób, aby inna klasa dziedziczyła po nim i implementowała go. Abstrakcja ma też inne zalety, które omówimy później. Klasa nie musi być abstrakcyjna, aby mogła być dziedziczona - jest to tylko jedno z możliwych ustawień.

Teraz możemy zaimplementować drugą klasę, na przykład `HomepagePresenter`:

```php
final class HomepagePresenter extends BasePresenter
{
   public function run(): void
   {
       // logika renderowania
       $this->renderTemplate('strona domowa', [
          'contactLink' => $this->link('Kontakt: domyślny'),
       ]);
   }
}
```

Teraz masz już działającą klasę `HomepagePresenter`. Zauważ, że klasa jest `ostateczna`, co oznacza, że nie może być już dziedziczona, co gwarantuje, że metody będą używane dokładnie tak, jak je podaliśmy.

Kiedy zaimplementowaliśmy klasę, stworzyliśmy nową metodę `run()`, którą może obsługiwać tylko `HomepagePresenter`. Wewnątrz tej metody wywołujemy metodę `renderTemplate()` oraz `link()`, których klasa nie zawiera. Nie ma to jednak znaczenia, ponieważ słowo kluczowe `extends` mówi nam, skąd mają być dziedziczone metody, więc te są używane.

Dzięki dziedziczeniu udało nam się uzyskać możliwość wielokrotnego wykorzystania kodu, ponieważ raz napisane metody mogą być używane w wielu miejscach.

Nadpisanie implementacji określonej metody
------------

Bardzo często podczas dziedziczenia przydatne może być nadpisanie zachowania określonej metody. Na przykład, gdybyśmy chcieli zmienić zachowanie metody `link()` z poprzedniego przykładu w `ContactPresenter`, wyglądałoby to tak:

```php
final class ContactPresenter extends BasePresenter
{
   public function run(): void
   {
      // logika renderowania
      echo $this->link('Strona główna:domyślna', []);
   }

   public function link(string $route, array $params = []): string
   {
      return 'https://baraja.cz';
   }
}
```

Aby nadpisać implementację, wystarczy ponownie zdefiniować metodę w elemencie potomnym i nadpisać jej treść. Ważne jest, aby spełnić wymagania interfejsu i zaimplementować te same argumenty wejściowe.

Widoczność dziedziczenia
--------------------------

Czasami chcielibyśmy ukryć niektóre metody podczas dziedziczenia i używać ich tylko wewnętrznie. Można też zezwolić na używanie ich tylko podczas dziedziczenia, a nie jako interfejsu publicznego.

Ogólnie rzecz biorąc, istnieją więc pewne proste zasady widoczności. Metody oznaczamy symbolami `public`, `protected` lub `private`, a reguły widoczności są następujące:

- Każdy może wywoływać metody `public` z dowolnego miejsca, to znaczy podczas tworzenia instancji zarówno przodka, jak i potomka.
- Tylko przodek lub potomek może wywoływać metody `protected`, ale nie mogą być one wywoływane z interfejsu publicznego, gdy tworzona jest instancja. Są to wewnętrzne metody służące do dziedziczenia (wygodnym zastosowaniem jest na przykład metoda `link()` w poprzednim przykładzie).
- Tylko bieżąca klasa może wywoływać metody `private`, niezależnie od ustawień dziedziczenia i interfejsu publicznego.

Zmiana widoczności w czasie pracy
----------------------------

W bardzo szczególnych przypadkach może być przydatna zmiana widoczności metody w czasie wykonywania, a następnie jej wywołanie. Jest ona wykorzystywana na przykład przez różne biblioteki Doctrine.

Aby zmienić widoczność, używamy natywnej klasy **ReflectionClass** zaimplementowanej przez samo PHP.
