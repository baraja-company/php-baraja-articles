Fluent Interfaces
=================

> id: bb71c005-6ef1-4dd3-8fae-843286e3476d
> slug:
> 	cs: fluent-interfaces
>
> publicationDate: "2020-02-16 22:19:11"
> mainCategoryId: ae4c1c70-11b3-433e-b1d0-e590155bb8b9

Fluent Interface je koncept objektově orientovaného programování, který umožňuje psát srozumitelnější a snadno čitelný kód v PHP. Tento koncept se stal populárním v posledních letech a je používán v mnoha PHP frameworkách a knihovnách.

Fluent Interface umožňuje zřetězení metod volání objektů. To znamená, že metoda vrací objekt sama o sobě, což umožňuje volání další metody na tento objekt bez nutnosti ukládat objekt do proměnné. To výrazně zlepšuje čitelnost a srozumitelnost kódu.

Příklad použití
---------------

Představme si například třídu Person, která obsahuje několik vlastností jako name, age, gender a email. Bez Fluent Interface bychom mohli použít metody getter a setter k nastavení a získání hodnot těchto vlastností:

```php
class Person {
    private $name;
    private $age;
    private $gender;
    private $email;

    public function getName() {
        return $this->name;
    }

    public function setName($name) {
        $this->name = $name;
    }

    public function getAge() {
        return $this->age;
    }

    public function setAge($age) {
        $this->age = $age;
    }

    public function getGender() {
        return $this->gender;
    }

    public function setGender($gender) {
        $this->gender = $gender;
    }

    public function getEmail() {
        return $this->email;
    }

    public function setEmail($email) {
        $this->email = $email;
    }
}
```

Pomocí Fluent Interface však můžeme vytvořit metody, které umožňují zřetězení a mohou být snadno čteny a pochopeny:

```php
class Person {
    private $name;
    private $age;
    private $gender;
    private $email;

    public function name($name) {
        $this->name = $name;
        return $this;
    }

    public function age($age) {
        $this->age = $age;
        return $this;
    }

    public function gender($gender) {
        $this->gender = $gender;
        return $this;
    }

    public function email($email) {
        $this->email = $email;
        return $this;
    }
}
```

Nyní můžeme použít Fluent Interface k nastavení hodnoty vlastností našeho objektu Person bez nutnosti ukládat objekt do proměnné:

```php
$person = new Person();
$person
   ->name('John')
   ->age(25)
   ->gender('Male')
   ->email('john@example.com');
```

Použití Fluent Interface nám umožňuje řetězit metody volání našeho objektu Person. Navíc, můžeme snadno číst kód a porozumět, jaké hodnoty jsou nastaveny pro objekt.

Validace dat
------------

Fluent Interface také umožňuje použití dalších metod nad naším objektem, jako jsou například metody pro validaci dat. Například můžeme přidat metodu validate, která ověří, zda jsou všechny povinné vlastnosti nastaveny:

```php
class Person {
    private $name;
    private $age;
    private $gender;
    private $email;

    public function name($name) {
        $this->name = $name;
        return $this;
    }

    public function age($age) {
        $this->age = $age;
        return $this;
    }

    public function gender($gender) {
        $this->gender = $gender;
        return $this;
    }

    public function email($email) {
        $this->email = $email;
        return $this;
    }

    public function validate() {
        if (empty($this->name) || empty($this->age) || empty($this->gender) || empty($this->email)) {
            throw new Exception('Povinná pole nebyla vyplněna.');
        }
    }
}
```

Nyní můžeme použít metodu validate nad objektem Person, což nám umožní ověřit, zda jsou všechny povinné vlastnosti nastaveny:

```php
$person = new Person();
$person
   ->name('John')
   ->age(25)
   ->gender('Male')
   ->email('john@example.com')
   ->validate();
```

V tomto příkladu používáme Fluent Interface k nastavení všech vlastností objektu Person a poté voláme metodu validate nad objektem. Pokud by nebyly nastaveny všechny povinné vlastnosti, vyvolá se výjimka.

Fluent Interface tedy umožňuje psát čitelnější a srozumitelnější kód v PHP. Využívá se v mnoha PHP frameworkách a knihovnách, jako je například Laravel, Symfony nebo Doctrine ORM. Výhodou použití Fluent Interface je zlepšení čitelnosti kódu, zkrácení kódu a zlepšení udržovatelnosti kódu.
