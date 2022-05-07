Zasada hermetyzacji w PPE
=========================

> id: '54968a42-b678-4385-91ac-c13ba96c9b34'
> slug:
> 	cs: zapouzdreni
> 	pl: zasada-hermetyzacji-w-ppe
> 
> publicationDate: '2020-02-16 21:21:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '3fbce6cdcbf5f274312496604569d9c2'

Jedną z głównych zasad OOP jest **zasada hermetyzacji**, która mówi, że złożone problemy powinny być rozbite na wiele małych problemów, które możemy rozwiązywać niezależnie i jednocześnie. Jednocześnie nas, jako użytkowników, nie interesuje, jak to się dzieje, a dane (stan wewnętrzny) pozostają odizolowane.

Na przykład, jeśli rozwiązujemy problem, jak zwrócić wynik `1,6` na podstawie zapytania użytkownika zawierającego wyrażenie `(5+3)*(2/(7+3))`, prawdopodobnie nikt z nas nie jest w stanie napisać jednej funkcji lub metody, która rozwiąże ten problem od razu.

**WSKAZÓWKA:** Gotowe rozwiązanie tego typu przykładu znajduje się w artykule <a href="/pokrocila-kalkulacka">Przetwarzanie wyrażenia matematycznego jako łańcucha</a>, ale przygotuj się na to, że nie jest to łatwe.

Enkapsulacja wprowadza abstrakcję do obiektów
-----------------------------------------

Dzięki enkapsulacji będzie można używać obiektów "jak użytkownik", czyli wywoływać ich metody i nie martwić się o to, jak działają wewnętrznie.

Załóżmy, że mamy do czynienia z obliczaniem pensji pracownika i chcemy wykorzystać do tego celu istniejącą klasę innego programisty. Musimy znać tylko obowiązkowe parametry konstruktora i możemy "po prostu używać" klasy:

```php
$mzda = new MzdaZamestnance(
    25000, // wynagrodzenie brutto
    6,     // liczba lat przepracowanych w firmie
    10,    // liczba lat doświadczenia
    true   // Czy to jest mężczyzna?
);

echo $mzda->getHruba(); // 25000

echo $mzda->getCista(); // 17800
```

Parametry obiektu są fikcyjne i nie odpowiadają rzeczywistemu sposobowi obliczania płacy. W szczególności zasadę tę ilustruje fakt, że **potrzebujemy znać tylko ogólnodostępny interfejs** i nie musimy zajmować się **wewnętrznym stanem obiektu**, ani nawet **wewnętrzną implementacją**, a już na pewno nie tym, **dlaczego obiekt działa tak, a nie inaczej**. Po prostu wywołujemy metodę `getCista()` i otrzymujemy wypłatę netto.

Enkapsulacja jest kwestią projektową
----------------------------

Należy zauważyć, że **enkapsulacja sama w sobie nie jest cechą ani składnią języka**. To, że klasa i aplikacja są hermetyzowane, jest tylko kwestią programisty projektującego aplikację i myślącego o kodzie.

Zawsze myśl o projektowaniu klasy w ten sposób:

- KISS (keep it simple), utrzymuj prosty interfejs i nie zmuszaj użytkownika do niepotrzebnego myślenia. Rozwiąż skomplikowaną logikę za użytkownika, a będzie on wdzięczny.
- Użytkownik klasy (inny programista lub Ty w przyszłości) nie musi w ogóle znać wewnętrznej logiki i wystarczą mu nazwy metod oraz ich parametry.
- Jeśli do obliczeń potrzebuję obliczeń pomocniczych, które nie interesują użytkownika i mają charakter wyłącznie techniczny, nie ma sensu w ogóle tworzyć dla nich gettera i powinny być one obliczane tylko wewnętrznie.
- Klasa musi spełniać podstawowe własności algorytmu, w szczególności to, że działa on ogólnie dla dowolnych danych.
- Ogólnodostępne metody powinny być zaprojektowane w taki sposób, aby dostarczały wystarczająco dużo informacji umożliwiających łatwe rozszerzenie obiektu o nowe funkcje w przyszłości, tak abyśmy mogli łatwo obliczać nowe dane na podstawie tego, co już wiemy.

Zachowaj niepubliczny charakter danych wewnętrznych
-------------------------------

Dla właściwości i metod, które dotyczą wewnętrznej logiki, sensowne jest ustawienie widoczności jako `private`. Główną zaletą takiego rozwiązania jest to, że nie będą one wywoływane z zewnątrz, a użytkownik będzie zmuszony do korzystania z zaprojektowanego przez Ciebie interfejsu, co zapewni ochronę danych i stanu wewnętrznego obiektu.

Przykładowo, mamy obiekt reprezentujący konto bankowe, na którym chcemy księgować płatności i zarządzać bieżącym saldem:

```php
class BankAccount
{
    private int $sum;

    public function __construct(int $startSum)
    {
        $this->sum = $startSum >= 0 ? $startSum : 0;
    }

    public function getSum(): int
    {
        return $this->sum;
    }

    public function pay(int $price): void
    {
        $newSum = $this->sum - $price;
        if ($newSum < 0) {
            throw new \Exception('Nie masz takich pieniędzy!');
        }

        $this->sum = $newSum;
    }

    public function addMoney(int $money): void
    {
        $this->sum += $money;
    }
}
```

Zauważ, że klasa zawiera tylko jedną `prywatną` właściwość `$suma`, która zawiera aktualne saldo.

Jeśli chcemy uzyskać aktualny stan konta, mamy do tego metodę `getSum()`, ale nie mamy możliwości zmiany nowej wartości stanu konta. Pieniądze możemy usunąć tylko metodą `pay()` lub dodać używając metody `addMoney()`.

Dzięki tej zasadzie zawsze wiemy na pewno, że nikt nie może złamać obiektu.

Jeśli użytkownik próbuje wpłacić więcej pieniędzy, niż faktycznie znajduje się na koncie, metoda `pay()` nie pozwoli na to, ponieważ przed nadpisaniem właściwości `$sum` wykonuje obliczenia kontrolne i jeśli saldo powinno być ujemne (mniejsze od zera), rzucany jest wyjątek błędu i operacja zostaje zatrzymana.

Wniosek
-----

Zademonstrowaliśmy podstawową zasadę hermetyzacji, która pozwala nam lepiej myśleć o abstrakcji obiektów i wnosi zupełnie nową perspektywę.

Kiedy już dobrze zrozumiesz tę zasadę, zobaczysz, że <a href="/proc-use-frameworks">frameworki zaczynają mieć ogromny sens</a>, ponieważ wewnętrznie hermetyzują wiele sprytnych rozwiązań, które możesz po prostu wykorzystać.

Następnym razem przyjrzymy się <a href="/dedicacy-and-visibility">dedicacy and visibility</a>.
