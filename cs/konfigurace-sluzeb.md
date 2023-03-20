Konfigurace služeb v OOP
========================

> id: "03441bd4-13e4-4217-b162-50bde0ea166b"
> slug:
> 	cs: konfigurace-sluzeb
>
> publicationDate: "2020-02-16 22:18:32"
> mainCategoryId: "3e45c55a-a4cd-4745-b1bb-0332702fefbf"

V objektově orientovaném programování (OOP) jsou služby často vytvářeny jako objekty, které mohou být použity v celé aplikaci. Konfigurace těchto služeb je důležitá pro správné fungování aplikace. V tomto článku se podíváme na to, jak konfigurovat služby v OOP pomocí konstant, konstruktoru a parametrů.

Použití konstant
----------------

Konstanty jsou hodnoty, které jsou definovány v třídě a nemohou být změněny. Tyto konstanty se obvykle používají pro výchozí hodnoty nebo pro hodnoty, které se nemění v průběhu běhu aplikace. Například, pokud máme třídu Database, můžeme definovat konstantu pro výchozí jméno databáze:

```php
class Database {
    const DEFAULT_DATABASE_NAME = 'my_database';

    private $databaseName;

    public function __construct($databaseName = self::DEFAULT_DATABASE_NAME) {
        $this->databaseName = $databaseName;
    }
}
```

Toto umožňuje vytvořit třídu Database s výchozím jménem databáze, pokud není poskytnuto žádné jméno. Pokud chceme použít jiné jméno, můžeme poskytnout název databáze jako parametr konstruktoru.

Konstruktor
-----------

Konstruktor je metoda, která se spustí při vytváření instance třídy. Konstruktor může být použit pro konfiguraci služby pomocí výchozích hodnot nebo parametrů.

```php
class Database {
    private $host;
    private $username;
    private $password;
    private $databaseName;

    public function __construct($host, $username, $password, $databaseName) {
        $this->host = $host;
        $this->username = $username;
        $this->password = $password;
        $this->databaseName = $databaseName;
    }
}
```

V tomto příkladu jsme vytvořili konstruktor pro třídu Database, který přijímá parametry pro hostitele, uživatelské jméno, heslo a jméno databáze. Tyto parametry mohou být použity pro konfiguraci třídy Database při vytváření instance.

Parametry
---------

Parametry jsou dalším způsobem konfigurace služby v OOP. Můžeme použít parametry pro poskytnutí různých konfiguračních informací pro službu, jako jsou například hesla, klíče nebo tokeny.

Dále jsou užitečné, pokud potřebujeme změnit chování služby během běhu aplikace. Například, pokud máme třídu `PaymentGateway`, která odesílá platby, můžeme poskytnout parametr pro specifikaci, zda má být platba odeslána okamžitě nebo později.

```php
class PaymentGateway {
    private $isImmediatePayment;

    public function __construct($isImmediatePayment = true) {
        $this->isImmediatePayment = $isImmediatePayment;
    }

    public function processPayment($amount, $paymentMethod) {
        if ($this->isImmediatePayment) {
            // Odeslat platbu okamžitě
        } else {
            // Uložit platbu do fronty pro pozdější odeslání
        }
    }
}
```

V tomto příkladu jsme vytvořili třídu `PaymentGateway`, která přijímá parametr `isImmediatePayment`, který určuje, zda má být platba odeslána okamžitě nebo později. Toto umožňuje vytvářet flexibilní aplikace, které mohou být snadno přizpůsobeny různým situacím.

Závěr
-----

Konfigurace služeb v OOP je důležitá pro správné fungování aplikace. V tomto článku jsme se podívali na to, jak použít konstanty, konstruktor a parametry pro konfiguraci služeb v PHP. Konstanty se obvykle používají pro výchozí hodnoty nebo pro hodnoty, které se nemění v průběhu běhu aplikace. Konstruktor je metoda, která se spustí při vytváření instance třídy a může být použit pro konfiguraci služby pomocí výchozích hodnot nebo parametrů. Parametry jsou užitečné, pokud potřebujeme poskytnout různé konfigurační informace pro službu nebo změnit chování služby během běhu aplikace.
