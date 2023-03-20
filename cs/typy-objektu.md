Typy objektů v OOP
==================

> id: "17777eac-67f6-4ae4-92b6-53964525a10e"
> slug:
> 	cs: typy-objektu
>
> publicationDate: "2020-02-16 22:18:42"
> mainCategoryId: "3e45c55a-a4cd-4745-b1bb-0332702fefbf"

V objektově orientovaném programování (OOP) v PHP se používají různé typy objektů, které mají různé účely a vlastnosti. V tomto článku se podíváme na pět hlavních typů objektů v OOP v PHP - třídu, objekt, službu, entitu a value-object.

Třída
-----

Třída je šablonou, ze které se vytvářejí objekty. Třída obsahuje definice vlastností a metod objektů, které jsou z ní vytvořeny. Třída se obvykle skládá z názvu, vlastností a metod.

```php
class User {
    private $id;
    private $name;

    public function __construct($id, $name) {
        $this->id = $id;
        $this->name = $name;
    }

    public function getName() {
        return $this->name;
    }
}
```

V tomto příkladu máme třídu User, která má dvě vlastnosti - id a name. Třída také obsahuje konstruktor a metodu getName(), která vrací jméno uživatele.

Objekt
-----

Objekt je instance třídy. Je to konkrétní reprezentace šablony třídy. Každý objekt má své vlastnosti a metody, které jsou definovány v jeho třídě. Objekt je vytvořen pomocí klíčového slova new.

```php
$user = new User(1, 'John');
echo $user->getName(); // vypíše "John"
```

V tomto příkladu jsme vytvořili instanci třídy User a uložili ji do proměnné $user. Následně jsme zavolali metodu getName(), která vrátila jméno uživatele.

Služba
------

Služba je objekt, který poskytuje určité funkcionality. Služba může být vytvořena jako samostatný objekt nebo může být součástí třídy.

```php
class UserService {
    private $userRepository;

    public function __construct(UserRepository $userRepository) {
        $this->userRepository = $userRepository;
    }

    public function getUser($id) {
        return $this->userRepository->findById($id);
    }
}
```

V tomto příkladu máme službu UserService, která využívá objekt UserRepository. Služba poskytuje funkci pro získání uživatele podle ID.

Entita
------

Entita je objekt, který reprezentuje jednu konkrétní entitu v systému. Entita může být například uživatel, produkt nebo objednávka. Entita obsahuje vlastnosti, které popisují tuto entitu a metody, které s ní mohou pracovat.

```php
class UserEntity {
    private $id;
    private $name;
    private $email;

    public function __construct($id, $name, $email) {
        $this->id = $id;
        $this->name = $name;
        $this->email = $email;
    }

    public function getId() {
        return $this->id;
    }

    public function getName() {
        return $this->name;
    }

    public function getEmail() {
        return $this->email;
    }
}
```

V tomto příkladu máme entitu UserEntity, která má tři vlastnosti - id, name a email. Entita obsahuje metody pro získání každé z těchto vlastností.

Value-Object
------------

Value-object je objekt, který reprezentuje hodnotu. Value-object má hodnotovou sémantiku, což znamená, že se porovnává hodnota objektu a ne jeho identita. Value-object může být použit například pro reprezentaci peněžní částky nebo časového intervalu.

```php
class Money {
    private $amount;
    private $currency;

    public function __construct($amount, $currency) {
        $this->amount = $amount;
        $this->currency = $currency;
    }

    public function getAmount() {
        return $this->amount;
    }

    public function getCurrency() {
        return $this->currency;
    }

    public function add(Money $money) {
        if ($this->currency !== $money->getCurrency()) {
            throw new InvalidArgumentException('Currencies do not match');
        }

        return new Money($this->amount + $money->getAmount(), $this->currency);
    }
}
```

V tomto příkladu máme value-object Money, který reprezentuje peněžní částku. Value-object obsahuje vlastnosti amount a currency a metody pro získání těchto vlastností. Value-object také obsahuje metodu add(), která slouží k přičtení peněžní částky. Při sčítání je ověřeno, zda se jedná o stejnou měnu.

Závěr
-----

V OOP v PHP existuje několik typů objektů - třída, objekt, služba, entita a value-object. Každý z těchto typů má své vlastní vlastnosti a využití v aplikacích. Znalost těchto typů objektů může pomoci při návrhu a implementaci aplikací v PHP.
