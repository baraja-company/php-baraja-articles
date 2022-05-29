Typ danych Obiekt Enum w PHP
============================

> id: '5b96765c-16c2-4f76-8e31-6de6e9f59365'
> slug:
> 	cs: datovy-typ-enum
> 	de: type-php
>
> publicationDate: '2022-05-29 15:30:00'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: '43735e926db87d2cc6c538ddb67e0a69'

Od wersji PHP 8.1 typ danych Enum może być używany do definiowania dokładnych wartości wyliczeniowych dla listy. Jest to przydatne w przypadkach, gdy wiemy, że wartość zmiennej może przyjmować tylko kilka określonych wartości.

Na przykład w ten sposób przechowuję typy powiadomień:

```php
enum OrderNotificationType: string
{
    case Email = 'email';
    case Sms = 'tekst';
}
```

W PHP typ danych Enum jest klasycznym obiektem, który zachowuje się jak specjalny typ stałej, ale posiada również instancję, którą można przekazać dalej. Jednak w przeciwieństwie do zwykłego obiektu, podlega on pewnym ograniczeniom.

Różnice między Enum a obiektami
-----------------------

Chociaż enumy są zbudowane na bazie klas i obiektów, nie obsługują wszystkich funkcji związanych z obiektami. W szczególności obiekty enum nie mogą posiadać stanu wewnętrznego (zawsze muszą być klasą statyczną).

Konkretna lista różnic:

- Zabronione jest stosowanie konstruktorów i destruktorów.
- Dziedziczenie nie jest obsługiwane. Enumy nie mogą być rozszerzane ani dziedziczone przez inne klasy.
- Właściwości statyczne lub obiektowe są niedozwolone.
- Klonowanie określonych wartości Enum (instancji) nie jest obsługiwane, każda indywidualna instancja musi być instancją pojedynczą.
- Metody magiczne, z wyjątkiem wymienionych poniżej, są zabronione.

Następujące właściwości obiektu są dostępne i zachowują się tak samo jak każdy inny obiekt:

- Metody publiczne, prywatne i chronione.
- Metody statyczne publiczne, prywatne i chronione.
- Stałe publiczne, prywatne i chronione.
- Enumy mogą implementować dowolną liczbę interfejsów.
- Atrybuty mogą być dołączane do enumów i przypadków. Filtr docelowy `TARGET_CLASS` zawiera same Enumy. Filtr docelowy `TARGET_CLASS_CONST` obejmuje przypadki Enum.
- Magiczne metody `__call`, `__callStatic` i `__invoke`.
- Stałe `__CLASS__` i `__FUNCTION__` zachowują się jak normalne stałe
- Stała magiczna `::class` na typie Enum jest obliczana jako pełna nazwa typu danych, łącznie z przestrzenią nazw, dokładnie tak, jak w przypadku obiektu. Magiczna stała `::class` na instancji typu `Case` jest również obliczana jako typ Enum, ponieważ jest to instancja innego typu.

Użycie Enum jako typu danych
-----------------------------

Wyobraźmy sobie, że mamy enum reprezentujące typy kombinezonów. W tym przypadku wystarczy zdefiniować typ `Suit` i zapisać poszczególne prawidłowe wartości.

Następnie otrzymujemy instancję danej opcji klasycznie poprzez kwadraturę, tak jak w przypadku pracy ze stałą statyczną.

Przykład definiowania Enum, wywoływania go przez określony typ i przekazywania do funkcji:

```php
enum Suit
{
	case Hearts;
	case Diamonds;
	case Clubs;
	case Spades;
}

function doStuff(Suit $s)
{
	// ...
}

doStuff(Suit::Spades);
```

Porównanie dwóch wartości
---------------------

Podstawową przewagą enumów nad obiektami i stałymi jest to, że łatwo jest porównywać ich wartości.

Podstawowe porównanie, w którym pracujemy z określoną wartością, można przeprowadzić w następujący sposób:

```php
$a = Suit::Spades;
$b = Suit::Spades;

$a === $b; // true
```

Bardzo często trzeba też zdecydować, czy dana wartość należy do poprawnego wyliczenia wartości Enum. Można to łatwo sprawdzić w następujący sposób:

```php
$a = Suit::Spades;

$a instanceof Suit;  // true
```

Odczytanie wartości typu
---------------------

Wartość określonego typu możemy odczytać albo jako nazwę stałej wywołującej, albo bezpośrednio jako zdefiniowaną wartość rzeczywistą (jeśli taka istnieje):

```php
enum Colors
{
	case Red;
	case Blue;
	case Green;

	public function getColor(): string
	{
	    return $this->name;
	}
}

function paintColor(Colors $colors): void
{
	echo "Farba:" . $colors->getColor();
}
```

Wartość stałej wywołującej jest odczytywana przez właściwość `name`. Ważne jest również to, że w typie danych Enum można bezpośrednio zaimplementować funkcję niestandardową, którą można wywoływać nad każdym Enum.

Jeśli dany Enum implementuje również wartości rzeczywiste (które są ukryte pod każdą stałą), można również odczytać ich wartość:

```php
enum OrderNotificationType: string
{
    case Email = 'email';
    case Sms = 'tekst';
}

$type = OrderNotificationType::Email;

echo $type->value;
```

Wszystkie prawidłowe wartości Enum
-----------------------------

Często zachodzi potrzeba wypisania (np. użytkownikowi w komunikacie o błędzie) wszystkich możliwych wartości, jakie może przyjmować Enum. W przypadku używania stałych nie było to możliwe, natomiast w przypadku funkcji Enum jest to łatwe:

```php
Suit::cases();
```

Zwraca `[Suit::Hearts, Suit::Diamonds, Suit::Clubs, Suit::Spades]`.

Sprawdź, czy zmienna jest typu Enum
---------------------------------

Możemy łatwo sprawdzić, czy dana zmienna nieznana zawiera Enum, stosując warunek:

```php
if ($haystack instanceof \BackedEnum) {
```

Każdy obiekt Enum jest automatycznie dzieckiem ogólnego interfejsu `BackedEnum`.

Więcej informacji można znaleźć w dyskusji na GitHubie PhpStan: https://github.com/phpstan/phpstan/issues/7304
