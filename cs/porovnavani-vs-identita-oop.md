Porovnávání objektů vs. identita
================================

> id: "0bd5d511-a672-4f81-8bf5-48e736983446"
> slug:
> 	cs: porovnavani-vs-identita-oop
>
> publicationDate: "2020-02-16 22:17:37"
> mainCategoryId: "3e45c55a-a4cd-4745-b1bb-0332702fefbf"

V PHP existují dva způsoby, jak porovnávat objekty - porovnávání hodnot a porovnávání identit. V tomto článku se podíváme na rozdíly mezi těmito dvěma způsoby a na to, kdy by měl být použit každý z nich.

Porovnávání hodnot
------------------

Porovnávání hodnot objektů znamená, že jsou porovnávány jejich vlastnosti, tedy data, která ukládají. Pokud jsou tyto vlastnosti stejné, jsou objekty považovány za ekvivalentní.

Při porovnávání hodnot můžeme využít různé metody, jako jsou například metody `equals()` nebo `compare()`. Tyto metody porovnávají vlastnosti objektů a vrací true nebo false v závislosti na tom, zda jsou objekty ekvivalentní.

```php
class User {
    private $id;
    private $name;

    public function __construct($id, $name) {
        $this->id = $id;
        $this->name = $name;
    }

    public function equals(User $otherUser) {
        return $this->id === $otherUser->id && $this->name === $otherUser->name;
    }
}
```

V tomto příkladu máme třídu `User`, která má dvě vlastnosti - `id` a `name`. Metoda `equals()` porovnává tyto vlastnosti s vlastnostmi jiného uživatele a vrací true nebo false v závislosti na tom, zda jsou objekty ekvivalentní.

Porovnávání identit
-------------------

Porovnávání identit objektů znamená, že jsou porovnávány samotné instance objektů. Pokud jsou instance stejné, jsou objekty považovány za ekvivalentní.

V PHP můžeme porovnávat instance objektů pomocí operátorů `==` nebo `===`. Operátor `==` porovnává hodnoty objektů, zatímco operátor `===` porovnává instance objektů. Pokud jsou instance stejné, vrací `=== true`.

```php
$user1 = new User(1, 'John');
$user2 = new User(1, 'John');

if ($user1 == $user2) {
    // true - porovnává hodnoty objektů
}

if ($user1 === $user2) {
    // false - porovnává instance objektů
}
```

V tomto příkladu jsme vytvořili dvě instance třídy `User` s identickými vlastnostmi. Pokud použijeme operátor `==`, vrací `true`, protože hodnoty objektů jsou stejné. Pokud použijeme operátor `===`, vrací `false`, protože porovnává instance objektů, které jsou navzájem různé.

Kdy použít porovnávání hodnot vs. identit
-----------------------------------------

Obecně platí, že pokud chceme porovnávat data v objektu, měli bychom použít porovnávání hodnot. Pokud potřebujeme porovnat samotné instance objektů, měli bychom použít porovnávání identit.

Porovnávání identit se obvykle používá, když porovnáváme reference na objekty, například při práci s databází nebo v případě, kdy máme více instancí třídy a chceme zjistit, zda jsou totožné. Na druhé straně, porovnávání hodnot se obvykle používá, když potřebujeme porovnávat data uložená v objektech.

Závěr
-----

Porovnávání objektů v PHP může být prováděno pomocí porovnávání hodnot nebo porovnávání identit. Porovnávání hodnot se používá pro porovnávání dat uložených v objektech, zatímco porovnávání identit se používá pro porovnávání samotných instancí objektů. Je důležité zvolit správný způsob porovnávání, aby se minimalizovala pravděpodobnost nekonzistentních výsledků.
