Magické metody v PHP
====================

> id: "4b1c9887-c5fa-4398-be3a-29c963616b8b"
> slug:
> 	cs: magicke-metody-oop
>
> publicationDate: "2020-02-16 22:19:02"
> mainCategoryId: ae4c1c70-11b3-433e-b1d0-e590155bb8b9

V PHP jsou k dispozici **Magické metody**, které začínají a končí dvojicí podtržítek. Tyto metody jsou používány pro speciální účely a jsou implementovány v různých třídách PHP. Magické metody jsou také známé jako přetížené metody nebo metodiky.

V tomto článku se budeme věnovat Magickým metodám v PHP a budeme se zaměřovat na to, jak se používají a jak mohou být užitečné v praxi.

Konstruktor a destruktor
------------------------

Dvě Magické metody, které jsou nejčastěji používány v PHP, jsou konstruktor a destruktor.

Konstruktor je Magická metoda, která se volá při vytváření nového objektu třídy. Tento konstruktor může mít parametry a může být použit k nastavení výchozích hodnot objektu.

```php
class Person {
    public $name;

    public function __construct($name) {
        $this->name = $name;
    }
}

$person = new Person("John");
echo $person->name;  // John
```

V tomto příkladu jsme vytvořili třídu Person s veřejnou vlastností $name. Konstruktor této třídy má jeden parametr $name, který je uložen do vlastnosti $name. Nakonec jsme vytvořili objekt person s jménem "John" a vytiskli jsme jméno objektu pomocí vlastnosti $name.

Destruktor
----------

Destruktor je Magická metoda, která se volá, když objekt není již používán nebo je ručně odstraněn. Destruktor může být použit k uvolnění zdrojů nebo k provedení jiných akcí.

```php
class Person {
    public $name;

    public function __construct($name) {
        $this->name = $name;
    }

    public function __destruct() {
        echo "The object is destroyed.";
    }
}

$person = new Person("John");
unset($person);  // The object is destroyed.
```

V tomto příkladu jsme vytvořili třídu Person s veřejnou vlastností $name a Magickou metodou __destruct. Tato metoda vytiskne řetězec "The object is destroyed." po odstranění objektu. Nakonec jsme vytvořili objekt person s jménem "John" a použili jsme funkci unset, aby byl objekt odstraněn a zavolána Magická metoda __destruct.

Přetížené metody
----------------

Další Magické metody jsou přetížené metody, které umožňují přepsat chování operátorů a funkcí v PHP. Tyto metody umožňují například porovnávat objekty, řazení nebo práci s proměnnými.

Některé z nejčastěji používaných přetížených metod jsou:

__toString()
------------

Tato metoda převádí objekt na řetězec a umožňuje jej vytisknout. Tuto metodu můžeme použít pro snadnější ladění a zobrazování objektů.

```php
class Person {
    public $name;

    public function __construct($name) {
        $this->name = $name;
    }

    public function __toString() {
        return "Name: " . $this->name;
    }
}

$person = new Person("John");
echo $person;  // Name: John
```

V tomto příkladu jsme vytvořili třídu Person s veřejnou vlastností $name a Magickou metodou `__toString`. Tato metoda vrací řetězec, který obsahuje jméno objektu. Nakonec jsme vytvořili objekt person s jménem "John" a vytiskli jsme ho pomocí echo.

__get() a __set()
-----------------

Tyto metody umožňují získat a nastavit hodnoty vlastností objektu.

```php
class Person {
    private $name;

    public function __get($property) {
        if (property_exists($this, $property)) {
            return $this->$property;
        }
    }

    public function __set($property, $value) {
        if (property_exists($this, $property)) {
            $this->$property = $value;
        }

        return $this;
    }
}

$person = new Person();
$person->name = "John";
echo $person->name;  // John
```

V tomto příkladu jsme vytvořili třídu `Person` s privátní vlastností `$name` a Magickými metodami `__get()` a `__set()`. Metoda `__get()` získá hodnotu vlastnosti, pokud existuje, a metoda `__set()` nastaví hodnotu vlastnosti, pokud existuje. Nakonec jsme vytvořili objekt person, nastavili jsme jeho jméno na `"John"` pomocí Magické metody `__set()` a vytiskli jsme ho pomocí Magické metody `__get()`.

__call() a __callStatic()
-------------------------

Tyto metody umožňují volat nedefinované metody. Metoda `__call()` se volá pro nedefinované instance metod a metoda `__callStatic()` se volá pro nedefinované statické metody.

```php
class Person {
    public function __call($name, $arguments) {
        echo "The method " . $name . " does not exist.";
    }

    public static function __callStatic($name, $arguments) {
        echo "The static method " . $name . " does not exist.";
    }
}

$person = new Person();
$person->doSomething();  // The method doSomething () does not exist.
Person::doSomething(); // The static method doSomething() does not exist.
```

V tomto příkladu jsme vytvořili třídu `Person` s Magickými metodami `__call()` a `__callStatic()`. Tyto metody vytisknou zprávu, že daná metoda neexistuje. Nakonec jsme vytvořili instanci třídy `Person` a zavolali jsme nedefinovanou metodu `doSomething()` pomocí Magické metody `__call()`. Poté jsme zavolali nedefinovanou statickou metodu `doSomething()` pomocí Magické metody `__callStatic()`.

Závěr
-----

Magické metody v PHP jsou velmi užitečné pro práci s objekty a mohou být použity k přetížení operátorů, k řízení chování objektů, k uvolnění zdrojů a dalším účelům. V tomto článku jsme se podívali na několik Magických metod, jako jsou konstruktor a destruktor, přetížené metody a další. Magické metody jsou důležitou součástí objektově orientovaného programování v PHP a umožňují vytvářet výkonné a flexibilní aplikace.
