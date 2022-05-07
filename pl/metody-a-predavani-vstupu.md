Metody w PPE i przekazywanie danych wejściowych
===============================================

> id: '843fbfb4-daf2-4c2e-9d94-28d494025b2e'
> slug:
> 	cs: metody-a-predavani-vstupu
> 	pl: metody-w-ppe-i-przekazywanie-danych-wejsciowych
> 
> publicationDate: '2020-02-16 20:49:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3e1bca690ed70479ef9807a1f2a1f23

Metody reprezentują zachowanie obiektu, ponieważ pozwalają na pracę z jego wewnętrznym stanem, a także na wzajemne oddziaływanie obiektów.

Przedstawianie metod w świecie rzeczywistym
----------------------------------

Weźmy pod uwagę dowolny obiekt rzeczywisty, na przykład kota. Kot ma pewne właściwości (imię, kolor, waga, ...), które opisujemy za pomocą właściwości, a także zachowanie (miauczenie, chodzenie, spanie, ...), które opisujemy za pomocą metod. Ponieważ na świecie jest wiele kotów ("I tak szczekam jak grom, niech będzie milion kotów"), należy pamiętać, że metoda jest czymś ogólnym, co stosuje się jednakowo do wszystkich obiektów danego typu.

Praktyczna realizacja metody
-----------------------------

Z punktu widzenia składni języka, zdefiniujmy klasę dla kota:

```php
class Cat
{
    public string $name;

    public string $sound = 'Meow';
}
```

Po utworzeniu instancji danego kota możemy po prostu wymienić np. dźwięk:

```php
$cat = new Cat;

echo $cat->sound; // mówi "Miau".
```

Ale co zrobić, jeśli chcemy sformatować dźwięk w określony sposób podczas zapisu? Wtedy do gry wchodzą metody:

```php
class Cat
{
    public string $name;

    public string $sound = 'Meow';

    public function getFormattedSound(): string
    {
        return 'Wydaję dźwięk".' . $this->sound . '"!';
    }
}
```

Na pewno znasz już zasadę działania funkcji z przeszłości. Klasy umożliwiają zapisanie funkcji bezpośrednio w jej ciele ze zdefiniowaną dostępnością (tak jak w przypadku właściwości) i są nazywane metodami.

Zmienna `$this` zachowuje się nieco "magicznie". Przechowuje ona bieżącą instancję obiektu, w którym się znajdujemy. Jeśli chcemy odczytać wartość z właściwości lub wywołać inną metodę wewnątrz metody, musimy to zrobić po prostu nad zmienną `$this`.

Metoda ta jest następnie wywoływana z wnętrza obiektu jako klasyczna funkcja (z nawiasem na końcu), aby zaznaczyć, że nie jest to właściwość:

```php
$cat = new Cat;

echo $cat->sound; // mówi "Miau".

echo $cat->getFormattedSound(); // prints "I'm making a "Meow" sound!
```

Konstruktor - metoda wywoływana podczas tworzenia instancji
--------------------------------------------------

Podczas tworzenia instancji obiektu często musimy określić, jak ustawić jego podstawowy stan oraz jakie parametry (dane wejściowe) są obowiązkowe.

W OOP, aby rozwiązać ten problem, istnieje specjalna metoda publiczna `__construct`, którą możemy zaimplementować dobrowolnie i która jest wywoływana zawsze i tylko podczas tworzenia instancji.

Przykład praktyczny:

```php
class Cat
{
    public string $name;

    public string $sound;

    public function __construct(string $name, string $sound)
    {
        $this->name = $name;
        $this->sound = $sound;
    }

    public function getFormattedSound(): string
    {
        return 'Wydaję dźwięk".' . $this->sound . '"!';
    }
}
```

Definiując konstruktor, upewniliśmy się, że podczas tworzenia instancji zawsze musimy przekazać 2 obowiązkowe parametry (`name` i `sound`), a obiekt zostanie ustawiony bezpośrednio.

Ponieważ konstruktor jest metodą klasyczną, może on zrobić coś innego z umieszczonymi w nim danymi. Na przykład odpowiednio sformatować ciąg znaków, ale tym zajmiemy się później.

Tworzenie instancji jest wtedy znowu proste, wystarczy przekazać parametry początkowe (są one zapisane w nawiasach przy nazwie klasy, gdzie następuje wywołanie nazwy klasy i utworzenie instancji):

```php
$cat = new Cat('Minda', 'Vrr');

echo $cat->name; // Tu jest napisane "Minda".
echo $cat->sound; // drukuje "Vrr".

echo $cat->getFormattedSound(); // prints 'I'm making a 'Vrr' sound!'
```

Gettery i settery
-----------------

Niektóre metody są nazywane `getterami` i `setterami`. Są to metody powszechne, dlatego przyjęło się je nazywać w ten sposób. Metody są używane do pobierania i wprowadzania danych do obiektu. Główną zaletą jest to, że możemy sprawdzić poprawność danych przed ich wstawieniem i ewentualnie je poprawić lub odrzucić dane i wyrzucić błąd.

Dla każdej właściwości, która może być pobierana (chcemy umożliwić pobieranie danych), zwyczajowo tworzy się getter zaczynający się od słowa `get`. Jeśli właściwość zwraca wartość typu boolean (`true` lub `false`), zwyczajowo na początku nazwy metody umieszcza się słowo `is`.

Przykład uproszczony:

```php
class Cat
{
    public string $name;

    public function __construct(string $name)
    {
        $this->setName($name);
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function isEmpty(): bool
    {
        return $this->name === '';
    }

    public function setName(string $name): void
    {
        $this->name = trim($name);
    }
}
```

Podczas tworzenia instancji kota przekazujemy jego nazwę do konstruktora (co zawsze jest obowiązkowe). Ponieważ jednak chcemy dzielić logikę wstawiania nazwy zarówno dla konstruktora, jak i dla settera (metody wstawiającej nową nazwę), wywołujemy metodę `setName()` wewnątrz konstruktora, aby zapewnić, że nazwa zostanie wstawiona.

Wewnątrz settera używamy funkcji <a href="/function-trim">trim()</a>, która automatycznie usuwa białe znaki z obu stron wstawianego łańcucha.

Do wyprowadzenia danych możemy użyć metody `getName()`. Do sprawdzenia pustości można użyć metody `isEmpty()`.

> **Praktyczne zalety:**
>
> Ogromną zaletą getterów i setterów są **gwarantowane typy danych**. Jeśli getter twierdzi, że zwraca `string`, możemy zawsze na nim polegać i zawsze otrzymamy string.
>
> Setter w swojej implementacji również twierdzi, że akceptuje `string`, co dla aplikacji oznacza, że cokolwiek wprowadzi użytkownik, zostanie albo przekonwertowane na `string` (jeśli używa kompatybilnego typu, takiego jak `integer`), albo otrzyma błąd, że typ nie może zostać przekonwertowany (jak `array`).

Największa wartość dodana getterów i setterów zostanie doceniona zwłaszcza w praktycznym zastosowaniu.

Magiczna metoda `__toString()`.
-----------------------------

Wewnątrz klasy można zdefiniować metodę magiczną `__toString()`, która jest automatycznie wywoływana przy próbie wypisania obiektu jako łańcucha znaków. Metoda musi zawsze zwracać ciąg znaków.

Przykładowe wdrożenie:

```php
class Cat
{
    public string $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function __toString(): string
    {
        return 'Cześć, jestem' . $this->name . ';)';
    }
}
```

Zastosowanie jest więc następujące:

```php
$cat = new Cat('Minda');

echo $cat; // Widnieje na nim napis "Cześć, jestem Minda ;)".
```

Streszczenie
-------

Pokazaliśmy, jak definiować metody, a następnie wywoływać je w obrębie instancji obiektu.

Następnym razem przyjrzymy się zasadzie <a href="/encapsulation">encapsulation principle</a>, która w pełni wykorzystuje właściwości metod.
