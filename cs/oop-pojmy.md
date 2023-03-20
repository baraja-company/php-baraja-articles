Vysvětlení pojmů Objektově orientovaného programování
=====================================================

> id: b574127d-38c2-4a44-ab4f-458f60c66ad9
> slug:
> 	cs: oop-pojmy
>
> publicationDate: "2020-02-16 18:53:09"
> mainCategoryId: "3e45c55a-a4cd-4745-b1bb-0332702fefbf"

Objektově orientované programování (OOP) je programovací paradigma, které se používá pro strukturování a organizaci kódu. OOP je založeno na konceptu objektů, které obsahují data a funkce, které mohou být použity k manipulaci s těmito daty. V PHP, jako i v mnoha jiných programovacích jazycích, existuje mnoho pojmenování, které se používají v objektově orientovaném programování. V tomto článku se podíváme na některé z těchto pojmů a vysvětlíme, co znamenají.

Třída
-----

Třída je definice pro objekt v PHP. Třída určuje, jaké vlastnosti a metody bude mít objekt. V třídě můžeme definovat vlastnosti (nebo také proměnné) a metody (nebo také funkce). Vlastnosti jsou proměnné, které jsou součástí objektu. Metody jsou funkce, které mohou být volány nad objektem.

```php
class Person {
    public $name;
    public $age;

    public function getName() {
        return $this->name;
    }

    public function getAge() {
        return $this->age;
    }
}
```

V tomto příkladu jsme vytvořili třídu `Person`, která má dvě vlastnosti `name` a `age`. Dále jsme vytvořili dvě metody `getName` a `getAge`, které vracejí hodnoty vlastností `name` a `age`.

Objekt
------

Objekt je instance třídy. Můžeme si to představit jako jednu konkrétní věc, například jednoho člověka. Každý objekt má své vlastnosti a metody, které jsou definovány v třídě, ze které byl vytvořen.

```php
$person = new Person();
$person->name = "John";
$person->age = 25;
```

V tomto příkladu jsme vytvořili objekt person z třídy `Person`. Nastavili jsme vlastnosti name a age pomocí operátoru `->`.

Konstruktor
-----------

Konstruktor je speciální metoda, která se volá při vytváření objektu. Konstruktor může být použit k inicializaci vlastností objektu.

```php
class Person {
    public $name;
    public $age;

    public function __construct($name, $age) {
        $this->name = $name;
        $this->age = $age;
    }

    public function getName() {
        return $this->name;
    }

    public function getAge() {
        return $this->age;
    }
}

$person = new Person("John", 25);
```

V tomto příkladu jsme vytvořili konstruktor v třídě `Person`, který má dva parametry `$name` a `$age`. Konstruktor nastavuje vlastnosti name a age na hodnoty těchto parametrů. Poté jsme vytvořili objekt person z třídy Person, a to s parametry `"John"` a `25`.

Dědičnost
---------

Dědičnost umožňuje vytvoření nové třídy, která dědí vlastnosti a metody z jiné třídy. Tato nová třída se nazývá podtřída nebo potomek. Třída, ze které dědíme, se nazývá rodičovská třída nebo nadřazená třída.

```php
class Animal {
    public function makeSound() {
        echo "The animal makes a sound.";
    }
}

class Cat extends Animal {
    public function makeSound() {
        echo "The cat meows.";
    }
}

$animal = new Animal();
$cat = new Cat();

$animal->makeSound();   // The animal makes a sound.
$cat->makeSound();      // The cat meows.
```

V tomto příkladu jsme vytvořili třídu Animal, která obsahuje metodu makeSound. Poté jsme vytvořili třídu Cat, která dědí od třídy Animal a přepisuje metodu makeSound. Nakonec jsme vytvořili objekty animal a cat a zavolali metodu makeSound nad každým z nich. Výsledkem volání nad objektem animal je řetězec "The animal makes a sound.", zatímco výsledkem volání nad objektem cat je řetězec "The cat meows.".

Rozhraní
--------

Rozhraní definuje soubor metod, které třída musí implementovat. Třída může implementovat více rozhraní. Rozhraní umožňují definovat společné metody, které lze použít na různých třídách.

```php
interface Animal {
    public function makeSound();
}

class Cat implements Animal {
    public function makeSound() {
        echo "The cat meows.";
    }
}

class Dog implements Animal {
    public function makeSound() {
        echo "The dog barks.";
    }
}

$cat = new Cat();
$dog = new Dog();

$cat->makeSound();  // The cat meows.
$dog->makeSound();  // The dog barks.
```

V tomto příkladu jsme vytvořili rozhraní Animal, které definuje jednu metodu makeSound. Poté jsme vytvořili třídy Cat a Dog, které implementují toto rozhraní. Každá z těchto tříd přepisuje metodu makeSound. Nakonec jsme vytvořili objekty cat a dog a zavolali metodu makeSound nad každým z nich. Výsledkem volání nad objektem cat je řetězec "The cat meows.", zatímco výsledkem volání nad objektem dog je řetězec "The dog barks.".

Zapouzdření
-----------

Zapouzdření umožňuje skrýt vnitřní stav objektu před vnějším světem a umožňuje přístup k tomuto stavu pouze pomocí speciálních metod, které jsou součástí třídy.

```php
class Person {
    private $name;
    private $age;

    public function __construct($name, $age) {
        $this->name = $name;
        $this->age = $age;
    }

    public function getName() {
        return $this->name;
    }

    public function getAge() {
        return $this->age;
    }

    public function setName($name) {
        $this->name = $name;
    }

    public function setAge($age) {
        $this->age = $age;
    }
}

$person = new Person("John", 25);
echo $person->getName();   // John
$person->setName("Peter");
echo $person->getName();   // Peter
```

V tomto příkladu jsme vytvořili třídu Person, která má dvě privátní vlastnosti name a age. To znamená, že tyto vlastnosti nejsou přístupné z vnějšku třídy. Namísto toho jsme vytvořili metody getName, getAge, setName a setAge, které jsou veřejné a umožňují přístup k těmto vlastnostem. V tomto příkladu jsme vytvořili objekt person s vlastnostmi "John" a 25. Poté jsme zavolali metodu getName, abychom získali hodnotu vlastnosti name. Poté jsme nastavili vlastnost name na "Peter" pomocí metody setName a zavolali jsme opět metodu getName, abychom získali novou hodnotu vlastnosti name. Výstupem bude "John" a "Peter".

Polymorfizmus
-------------

Polymorfizmus umožňuje třídám mít stejná jména metod, ale různé implementace. To znamená, že metody mohou být volány pomocí stejného jména, ale přiřazené objekty se chovají odlišně v závislosti na typu objektu.

```php
class Animal {
    public function makeSound() {
        echo "The animal makes a sound.";
    }
}

class Cat extends Animal {
    public function makeSound() {
        echo "The cat meows.";
    }
}

class Dog extends Animal {
    public function makeSound() {
        echo "The dog barks.";
    }
}

function animalSound(Animal $animal) {
    $animal->makeSound();
}

$animal = new Animal();
$cat = new Cat();
$dog = new Dog();

animalSound($animal);  // The animal makes a sound.
animalSound($cat);     // The cat meows.
animalSound($dog);     // The dog barks.
```

V tomto příkladu jsme vytvořili třídy Animal, Cat a Dog, kde každá třída má metodu makeSound. Poté jsme vytvořili funkci animalSound, která bere objekt typu Animal jako parametr. Nakonec jsme vytvořili objekty animal, cat a dog a zavolali funkci animalSound nad každým z nich. Výsledkem volání nad objektem animal je řetězec "The animal makes a sound.", zatímco výsledkem volání nad objektem cat je řetězec "The cat meows." a výsledkem volání nad objektem dog je řetězec "The dog barks.".

Abstraktní třídy
----------------

Abstraktní třídy jsou třídy, které nemohou být vytvořeny přímo, ale musí být rozšířeny jinou třídou. Abstraktní třídy obsahují abstraktní metody, které musí být implementovány v podtřídách.

```php
abstract class Animal {
    protected $name;

    public function __construct($name) {
        $this->name = $name;
    }

    abstract public function makeSound();
}

class Cat extends Animal {
    public function makeSound() {
        echo "The cat meows.";
    }
}

$cat = new Cat("Tom");
$cat->makeSound();  // The cat meows.
```

V tomto příkladu jsme vytvořili abstraktní třídu Animal, která má chráněnou vlastnost name a abstraktní metodu makeSound. Poté jsme vytvořili třídu Cat, která dědí od třídy Animal a implementuje metodu makeSound. V konstruktoru třídy Cat jsme předali jméno zvířete a v metodě makeSound jsme vytiskli řetězec "The cat meows.". Nakonec jsme vytvořili objekt cat a zavolali jsme metodu makeSound. Výstupem bude řetězec "The cat meows.".

Rozhraní
--------

Rozhraní definuje soubor metod, které musí být implementovány v třídě, která rozhraní implementuje. Rozhraní mohou být použita k vytvoření kódu, který je nezávislý na konkrétní třídě, ale který vyžaduje určitou funkcionalitu.

```php
interface Animal {
    public function makeSound();
}

class Cat implements Animal {
    public function makeSound() {
        echo "The cat meows.";
    }
}

$cat = new Cat();
$cat->makeSound();  // The cat meows.
```

V tomto příkladu jsme vytvořili rozhraní Animal, které má jednu metodu makeSound. Poté jsme vytvořili třídu Cat, která implementuje rozhraní Animal a implementuje metodu makeSound. Nakonec jsme vytvořili objekt cat a zavolali jsme metodu makeSound. Výstupem bude řetězec "The cat meows.".

Závěr
-----

V tomto článku jsme prošli některé z nejdůležitějších pojmů objektově orientovaného programování v PHP. Objektově orientované programování je klíčové pro psaní čitelného, strukturovaného a údržbatelného kódu v PHP, což je velmi důležité pro velké projekty s více vývojáři.

Základními prvky objektově orientovaného programování jsou třídy, objekty, vlastnosti a metody. Další důležitým pojmem je dědičnost, která umožňuje třídám sdílet vlastnosti a metody s jinými třídami. Polymorfizmus umožňuje třídám mít stejná jména metod, ale různé implementace. Abstraktní třídy obsahují abstraktní metody, které musí být implementovány v podtřídách. Rozhraní definuje soubor metod, které musí být implementovány v třídě, která rozhraní implementuje.

Je důležité mít porozumění těmto pojmům, abyste mohli psát kvalitní kód v PHP. Všechny tyto pojmy byly představeny s příklady kódu, aby byly jednoduše pochopitelné. Pokud budete tyto pojmy správně využívat, pomohou vám psát čitelný, strukturovaný a údržbatelný kód v PHP.
