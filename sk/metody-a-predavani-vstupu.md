Metódy v PPE a prenos vstupov
=============================

> id: '843fbfb4-daf2-4c2e-9d94-28d494025b2e'
> slug:
> 	cs: metody-a-predavani-vstupu
> 	sk: metody-v-ppe-a-prenos-vstupov
> 
> publicationDate: '2020-02-16 20:49:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3e1bca690ed70479ef9807a1f2a1f23

Metódy predstavujú správanie objektu, pretože umožňujú pracovať s jeho vnútorným stavom, ako aj ovplyvňovať objekty navzájom.

Reprezentácia metód v reálnom svete
----------------------------------

Zoberme si akýkoľvek objekt z reálneho sveta, napríklad mačku. Mačka má určité vlastnosti (meno, farbu, hmotnosť, ...), ktoré popisujeme pomocou vlastností, a potom aj správanie (mňaučenie, chodenie, spanie, ...), ktoré popisujeme pomocou metód. Keďže na svete je veľa mačiek ("A tak štekám ako hrom, nech je milión mačiek"), je dôležité si uvedomiť, že metóda je niečo všeobecné, čo platí rovnako pre všetky objekty daného typu.

Praktická implementácia metódy
-----------------------------

Z hľadiska syntaxe jazyka definujme triedu pre mačku:

```php
class Cat
{
    public string $name;

    public string $sound = "Mňau;
}
```

Po vytvorení inštancie konkrétnej mačky môžeme jednoducho vypísať napríklad zvuk:

```php
$cat = new Cat;

echo $cat->sound; // hovorí "Mňau"
```

Ale čo ak chceme zvuk pri zápise naformátovať určitým spôsobom? Potom prichádzajú na rad metódy:

```php
class Cat
{
    public string $name;

    public string $sound = "Mňau;

    public function getFormattedSound(): string
    {
        return "Vydávam zvuk . $this->sound . '"!';
    }
}
```

Princíp funkcií musíte poznať už z minulosti. Triedy umožňujú zapísať funkciu priamo do jej tela s definovanou prístupnosťou (ako pri vlastnostiach) a nazývajú sa metódy.

Premenná `$this` sa správa trochu "magicky". Ukladá aktuálnu inštanciu objektu, v ktorom sa práve nachádzame. Ak chceme prečítať hodnotu z vlastnosti alebo zavolať inú metódu v rámci metódy, stačí to urobiť cez premennú `$this`.

Metóda sa potom volá zvnútra objektu ako klasická funkcia (so zátvorkou na konci), aby bolo jasné, že nejde o vlastnosť:

```php
$cat = new Cat;

echo $cat->sound; // hovorí "Mňau"

echo $cat->getFormattedSound(); // vypíše 'Vydávam zvuk "Mňau"!'
```

Konštruktor - metóda volaná pri vytváraní inštancie
--------------------------------------------------

Pri vytváraní inštancie objektu musíme často definovať, ako nastaviť jeho základný stav a ktoré parametre (vstupné údaje) sú povinné.

V OOP na riešenie tohto problému existuje špeciálna verejná metóda `__construct`, ktorú môžeme dobrovoľne implementovať a ktorá sa volá vždy a len pri vytváraní inštancie.

Praktický príklad:

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
        return "Vydávam zvuk . $this->sound . '"!';
    }
}
```

Definovaním konštruktora sme zabezpečili, že pri vytváraní inštancie musíme vždy odovzdať 2 povinné parametre (`name` a `sound`) a objekt sa nastaví priamo.

Keďže konštruktor je klasická metóda, môže vo vnútri robiť s vloženými údajmi niečo iné. Napríklad vhodne preformátovať reťazec, ale tým sa budeme zaoberať neskôr.

Vytvorenie inštancie je potom opäť jednoduché, stačí odovzdať počiatočné parametre (píšu sa v zátvorkách k názvu triedy, kde sa volá názov triedy a vytvára sa inštancia):

```php
$cat = new Cat("Minda, 'Vrr');

echo $cat->name; // Píše sa tam "Minda"
echo $cat->sound; // vypíše "Vrr"

echo $cat->getFormattedSound(); // vypíše 'Vydávam zvuk "Vrr"!'
```

Gettery a settery
-----------------

Niektoré metódy sa nazývajú `getters` a `setters`. Ide o bežné metódy, je len zvykom ich tak nazývať. Metódy sa používajú na získavanie a vkladanie údajov do objektu. Hlavnou výhodou je, že môžeme údaje pred vložením overiť a prípadne ich opraviť alebo odmietnuť a vyhodiť chybu.

Pre každú vlastnosť, ktorú možno získať (chceme umožniť získanie údajov), je zvykom vytvoriť getter začínajúci slovom `get`. Ak vlastnosť vracia logickú hodnotu (`true` alebo `false`), je zvykom pomenovať začiatok názvu metódy slovom `is`.

Zjednodušený príklad:

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

Pri vytváraní inštancie mačky odovzdáme jej meno konštruktorovi (čo je vždy povinné). Keďže však chceme zdieľať logiku vkladania mena pre konštruktor aj setter (metóda na vkladanie nového mena), voláme metódu `setName()` vnútri konštruktora, aby sme zabezpečili vloženie mena.

Vo vnútri nastavovača použijeme funkciu <a href="/function-trim">trim()</a>, ktorá automaticky odstráni biele znaky z oboch strán vloženého reťazca.

Na výstup môžeme použiť metódu `getName()`. Na kontrolu prázdnoty možno použiť metódu `isEmpty()`.

> **Praktické výhody:**
>
> Obrovskou výhodou getterov a setterov sú **zaručené dátové typy**. Ak getter tvrdí, že vracia `reťazec`, môžeme sa naň vždy spoľahnúť a vždy dostaneme reťazec.
>
> Setter vo svojej implementácii tiež tvrdí, že akceptuje `string`, čo pre aplikáciu znamená, že čokoľvek používateľ zadá, buď sa jeho vstup skonvertuje na `string` (ak používa kompatibilný typ, napríklad `integer`), alebo dostane chybu, že typ nemožno skonvertovať (napríklad `array`).

Najväčšiu pridanú hodnotu getterov a setterov oceníte najmä pri praktickom použití.

Magická metóda `__toString()`
-----------------------------

V rámci triedy môžete definovať magickú metódu `__toString()`, ktorá sa automaticky zavolá, keď sa pokúsite vypísať objekt ako reťazec. Metóda musí vždy vracať reťazec.

Príklad implementácie:

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
        return "Ahoj, ja som . $this->name . ' ;)';
    }
}
```

Použitie je potom nasledovné:

```php
$cat = new Cat("Minda);

echo $cat; // Píše sa tam "Ahoj, ja som Minda ;)"
```

Zhrnutie
-------

Ukázali sme si, ako definovať metódy a potom ich volať v rámci inštancie objektu.

Nabudúce sa pozrieme na <a href="/encapsulation">princíp zapuzdrenia</a>, ktorý plne využíva vlastnosti metód.
