Metody v OOP a předávání vstupů
================================

> id: "843fbfb4-daf2-4c2e-9d94-28d494025b2e"
> slugCS: metody-a-predavani-vstupu
> publicationDate: "2020-02-16 20:49:35"
> mainCategoryId: "3e45c55a-a4cd-4745-b1bb-0332702fefbf"

Metody reprezentují chování objektu, protože umožňují pracovat jak s jeho vnitřním stavem, tak ovlivňovat objekty navzájem.

Reprezentace metod v reálném světě
----------------------------------

Mějme jakýkoli reálný objekt, třeba kočku. Kočka má určité vlastnosti (jméno, barvu, váhu, ...), které popíšeme pomocí tzv. properties a pak také chování (mňouká, chodí, spí, ...) a to popíšeme pomocí metod. Protože na světě existuje koček hodně ("A tak štěkám jako hrom, ať je koček milion") tak je důležité si uvědomit, že metoda je něco obecného, co platí pro všechny objekty daného typu stejně.

Praktická implementace metody
-----------------------------

V rámci syntaxe jazyka si definujme třídu pro kočku:

```php
class Cat
{

    /** @var string */
    public $name;

    /** @var string */
    public $sound = 'Mňau';
}
```

Po vytvoření instance konkrétní kočky si můžeme jednoduše vypsat například zvuk:

```php
$cat = new Cat;

echo $cat->sound; // vypíše "Mňau"
```

Co když ale budeme chtít zvuk při vypsání určitým způsobem formátovat? Pak přijdou na řadu metody:

```php
class Cat
{

    /** @var string */
    public $name;

    /** @var string */
    public $sound = 'Mňau';

    public function getFormattedSound(): string
    {
        return 'Dělám zvuk "' . $this->sound . '"!';
    }
}
```

Princip funkcí už určitě znáte z minulosti. Třídy umožňují funkci zapsat přímo do svého těla s definovanou přístupností (stejně jako u properties) a říkají tomu metody.

Proměnná `$this` se chová trochu "magicky". Je v ní totiž uložena aktuální instance objektu, ve kterém se zrovna nacházíme. Pokud chceme v rámci metody přečíst hodnotu z property nebo zavolat jinou metodu, stačí to provést právě nad proměnnou `$this`.

Metoda se pak z vnitřku objektu volá jako klasická funkce (se závorkou na konci), abychom řekli, že nejde o property:

```php
$cat = new Cat;

echo $cat->sound; // vypíše "Mňau"

echo $cat->getFormattedSound(); // vypíše 'Dělám zvuk "Mňau"!'
```

Konstruktor - metoda volaná při vytváření instance
--------------------------------------------------

Při vytváření instance objektu často potřebujeme definovat, jak se má nastavit jeho základní stav a které parametry (vstupní data) jsou povinné.

V OOP pro řešení tohoto problému existuje speciální veřejná metoda `__construct`, kterou můžeme dobrovolně implementovat a volá se vždy a pouze při vytváření instance.

Praktický příklad:

```php
class Cat
{

    /** @var string */
    public $name;

    /** @var string */
    public $sound;

    public function __construct(string $name, string $sound)
    {
        $this->name = $name;
        $this->sound = $sound;
    }

    public function getFormattedSound(): string
    {
        return 'Dělám zvuk "' . $this->sound . '"!';
    }
}
```

Definováním kontruktoru jsme zařídili, že při vytváření instance musíme vždy předat 2 povinné parametry (`name` a `sound`) a objekt se tím rovnou nastaví.

Protože je konstruktor klasická metoda, tak může uvnitř s vloženými daty ještě něco udělat. Například řetězec vhodně přeformátovat, ale to vyřešíme až později.

Vytvoření instance je pak opět snadné, jenom musíme předat počáteční parametry (píší se do závorky k názvu třídy tam, kde se volá její název a vytváří instance):

```php
$cat = new Cat('Minda', 'Vrr');

echo $cat->name; // Vypíše "Minda"
echo $cat->sound; // vypíše "Vrr"

echo $cat->getFormattedSound(); // vypíše 'Dělám zvuk "Vrr"!'
```

Gettery a settery
-----------------

Některým metodám se říká `gettery` a `settery`. Jde o běžnou metodu, jenom je konvence je takto pojmenovávat. Metody slouží k získávání a vkládání dat do objektu. Výhoda je zejména v tom, že můžeme před vložením dat provést jejich validaci a případně je opravit nebo data odmítnout a vyhodit chybu.

Pro každou property kterou lze načíst (chceme umožnit získání dat), je zvykem vytvořit getter začínající slovem `get`. Pokud property vrací boolean (`true` nebo `false`), tak je zvykem začátek názvu metody pojmenovat slovem `is`.

Zjednodušený příklad:

```php
class Cat
{

    /** @var string */
    public $name;

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

Při vytvoření instance kočky předáme její jméno do konstruktoru (který je vždy povinný). Protože ale chceme sdílet logiku pro vkládání jména jak pro konstruktor, tak pro setter (metodu pro vložení nového jména), tak uvnitř konstruktoru zavoláme rovnou metodu `setName()`, která vložení jména zajistí.

Uvnitř setteru používáme funkci <a href="/funkce-trim">trim()</a>, která automaticky odebere bílé znaky z obou stran vloženého řetězce.

Pro výpis můžeme použít metodu `getName()`. Prázdnost lze ověřit zas metodou `isEmpty()`.

> **Praktické výhody:**
>
> Obrovská výhoda getterů a setterů jsou **garantované datové typy**. Pokud getter o sobě tvrdí, že vrací `string`, tak se na to můžeme vždy spolehnout a vždy dostaneme string.
>
> Setter ve své implementaci také tvrdí, že přijímá `string`, což znamená pro aplikaci to, že ať zadá uživatel cokoli, tak se buď jeho vstup přetypuje na `string` (pokud použije kompatibilní typ, například `integer`), nebo dostane chybu, že typ nelze převést (například `array`).

Největší přidanou hodnotu getterů a setterů oceníte zejména až při praktickém použití.

Magická metoda `__toString()`
-----------------------------

V rámci třídy lze definovat tzv. magickou metodu `__toString()`, která se automaticky volá při pokusu o vypsání objektu jako řetězec. Metoda musí vždy vrátit string.

Příklad implementace:

```php
class Cat
{

    /** @var string */
    public $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function __toString(): string
    {
        return 'Ahoj, já jsem ' . $this->name . ' ;)';
    }
}
```

Použití je pak následující:

```php
$cat = new Cat('Minda');

echo $cat; // Vypíše "Ahoj, já jsem Minda ;)"
```

Shrnutí
-------

Ukázali jsme si, jak definovat metody a pak je v rámci instance objektu zavolat.

Příště se podíváme na <a href="/zapouzdreni">princip zapouzdření</a>, který vlastností metod naplno využívá.
