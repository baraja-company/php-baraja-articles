Metoder för PPE och överföring av input
=======================================

> id: '843fbfb4-daf2-4c2e-9d94-28d494025b2e'
> slug:
> 	cs: metody-a-predavani-vstupu
> 	sv: metoder-foer-ppe-och-oeverfoering-av-input
> 
> publicationDate: '2020-02-16 20:49:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3e1bca690ed70479ef9807a1f2a1f23

Metoder representerar ett objekts beteende eftersom de gör det möjligt att arbeta med dess interna tillstånd och att påverka objekt med varandra.

Representation av metoder i den verkliga världen
----------------------------------

Tänk på ett objekt i den verkliga världen, till exempel en katt. En katt har vissa egenskaper (namn, färg, vikt, ...) som vi beskriver med hjälp av egenskaper och sedan även beteende (gnäll, gå, sova, ...) som vi beskriver med hjälp av metoder. Eftersom det finns många katter i världen ("And so I bark like thunder, let there be a million cats") är det viktigt att komma ihåg att en metod är något allmänt som gäller för alla objekt av en viss typ.

Praktiskt genomförande av en metod
-----------------------------

När det gäller språksyntaxen kan vi definiera en klass för en katt:

```php
class Cat
{
    public string $name;

    public string $sound = 'Meow';
}
```

När vi har skapat en instans av en viss katt kan vi helt enkelt lista till exempel ljudet:

```php
$cat = new Cat;

echo $cat->sound; // säger "Meow"
```

Men vad händer om vi vill formatera ljudet på ett visst sätt när vi skriver det? Då kommer metoderna in i bilden:

```php
class Cat
{
    public string $name;

    public string $sound = 'Meow';

    public function getFormattedSound(): string
    {
        return 'Jag gör ljud "' . $this->sound . '"!';
    }
}
```

Du måste redan känna till funktionsprincipen från det förflutna. Klasser gör det möjligt att skriva en funktion direkt i dess kropp med definierad tillgänglighet (som med egenskaper) och kallas metoder.

Variabeln `$this` beter sig lite "magiskt". Den lagrar den aktuella instansen av det objekt vi befinner oss i. Om vi vill läsa ett värde från en egenskap eller anropa en annan metod inom en metod behöver vi bara göra det över variabeln `$this`.

Metoden anropas sedan inifrån objektet som en klassisk funktion (med en parentes i slutet) för att visa att det inte är en egenskap:

```php
$cat = new Cat;

echo $cat->sound; // säger "Meow"

echo $cat->getFormattedSound(); // skriver ut "Jag gör ett "Meow"-ljud!
```

Konstruktör - metod som anropas när en instans skapas
--------------------------------------------------

När vi skapar en objektinstans måste vi ofta definiera hur dess grundläggande tillstånd ska ställas in och vilka parametrar (indata) som är obligatoriska.

För att lösa detta problem finns det i OOP en särskild offentlig metod `__construct` som vi frivilligt kan implementera och som alltid och endast anropas när en instans skapas.

Praktiskt exempel:

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
        return 'Jag gör ljud "' . $this->sound . '"!';
    }
}
```

Genom att definiera konstruktören har vi sett till att när vi skapar en instans måste vi alltid skicka två obligatoriska parametrar (`name` och `sound`) och objektet kommer att ställas in direkt.

Eftersom konstruktören är en klassisk metod kan den göra något annat inuti med de infogade uppgifterna. Du kan till exempel omformatera strängen på lämpligt sätt, men det tar vi upp senare.

Det är sedan enkelt att skapa en instans igen, vi behöver bara skicka de första parametrarna (de skrivs inom parentes till klassnamnet när klassnamnet anropas och instansen skapas):

```php
$cat = new Cat('Minda', 'Vrr');

echo $cat->name; // Det står "Minda".
echo $cat->sound; // skriver ut "Vrr"

echo $cat->getFormattedSound(); // skriver ut "Jag gör ett "Vrr"-ljud!
```

Gettery och settery
-----------------

Vissa metoder kallas `getters` och `setters`. Det är vanliga metoder, det är bara en konvention att kalla dem så. Metoder används för att hämta och lägga in data i ett objekt. Den största fördelen är att vi kan validera data innan vi lägger in dem och eventuellt korrigera dem eller avvisa data och skicka ett fel.

För varje egenskap som kan hämtas (vi vill möjliggöra datahämtning) är det vanligt att skapa en getter som börjar med ordet `get`. Om egenskapen returnerar ett boolska värde (`true` eller `false`) är det vanligt att börja metodnamnet med ordet `is`.

Förenklat exempel:

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

När vi skapar en kattinstans skickar vi dess namn till konstruktören (vilket alltid är obligatoriskt). Men eftersom vi vill dela logiken för att infoga namnet för både konstruktören och settern (metoden för att infoga ett nytt namn) anropar vi metoden `setName()` i konstruktören för att se till att namnet infogas.

I settern använder vi <a href="/function-trim">trim()</a>-funktionen, som automatiskt tar bort vitrymder från båda sidor av den infogade strängen.

Vi kan använda metoden `getName()` för utdata. Metoden `isEmpty()` kan användas för att kontrollera om en fil är tom.

> **Praktiska fördelar:**
>
> En stor fördel med getters och setters är **garanterade datatyper**. Om en getter hävdar att den returnerar en sträng kan vi alltid lita på den och alltid få en sträng.
>
> En setter i sin implementering säger sig också acceptera en `string`, vilket för programmet innebär att vad användaren än skriver in kommer antingen att få sin inmatning konverterad till en `string` (om den använder en kompatibel typ, t.ex. `integer`) eller få ett felmeddelande om att typen inte kan konverteras (t.ex. `array`).

Det största mervärdet av getters och setters kommer att uppskattas särskilt i praktisk användning.

Den magiska metoden `__toString()`
-----------------------------

Inom en klass kan du definiera en magisk metod `__toString()` som automatiskt anropas när du försöker skriva ut ett objekt som en sträng. Metoden måste alltid returnera en sträng.

Exempel på genomförande:

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
        return 'Hej, jag heter' . $this->name . ';)';
    }
}
```

Användningen är då följande:

```php
$cat = new Cat('Minda');

echo $cat; // Det står "Hej, jag heter Minda ;)".
```

Sammanfattning
-------

Vi har visat hur man definierar metoder och sedan anropar dem i en objektinstans.

Nästa gång ska vi titta på <a href="/kapsling">kapslingsprincipen</a>, som utnyttjar metodernas egenskaper fullt ut.
