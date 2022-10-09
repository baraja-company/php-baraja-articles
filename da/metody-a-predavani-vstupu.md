Metoder i PPE og overførsel af input
====================================

> id: '843fbfb4-daf2-4c2e-9d94-28d494025b2e'
> slug:
> 	cs: metody-a-predavani-vstupu
> 	da: metoder-i-ppe-og-overforsel-af-input
> 
> publicationDate: '2020-02-16 20:49:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3e1bca690ed70479ef9807a1f2a1f23

Metoder repræsenterer et objekts adfærd, fordi de giver dig mulighed for at arbejde med dets interne tilstand og for at påvirke objekter med hinanden.

Repræsentation af metoder i den virkelige verden
----------------------------------

Tænk på et hvilket som helst objekt i den virkelige verden, f.eks. en kat. En kat har visse egenskaber (navn, farve, vægt, ...), som vi beskriver ved hjælp af egenskaber, og så har den også adfærd (miaven, gåtur, sove, ...), som vi beskriver ved hjælp af metoder. Da der er mange katte i verden ("Og så jeg gøer som torden, lad der være en million katte") er det vigtigt at huske, at en metode er noget generelt, der gælder for alle objekter af en given type på samme måde.

Praktisk gennemførelse af en metode
-----------------------------

Med hensyn til sprogets syntaks kan vi definere en klasse for en kat:

```php
class Cat
{
    public string $name;

    public string $sound = 'Meow';
}
```

Når vi har oprettet en instans af en bestemt kat, kan vi blot opstille en liste over f.eks. lyden:

```php
$cat = new Cat;

echo $cat->sound; // siger "Meow"
```

Men hvad nu, hvis vi ønsker at formatere lyden på en bestemt måde, når vi skriver den ud? Så kommer metoderne ind i billedet:

```php
class Cat
{
    public string $name;

    public string $sound = 'Meow';

    public function getFormattedSound(): string
    {
        return 'Jeg laver lyd "' . $this->sound . '"!';
    }
}
```

Du skal allerede kende princippet om funktioner fra fortiden. Klasser gør det muligt at skrive en funktion direkte ind i dens krop med defineret tilgængelighed (som med egenskaber) og kaldes metoder.

Variablen `$this` opfører sig lidt "magisk". Den gemmer den aktuelle instans af det objekt, vi befinder os i. Hvis vi vil læse en værdi fra en egenskab eller kalde en anden metode inden for en metode, skal vi blot gøre det over variablen `$this`.

Metoden kaldes derefter indefra objektet som en klassisk funktion (med en parentes i slutningen) for at angive, at det ikke er en egenskab:

```php
$cat = new Cat;

echo $cat->sound; // siger "Meow"

echo $cat->getFormattedSound(); // udskriver "Jeg laver en "miav"-lyd!
```

Konstruktør - metode, der kaldes, når der oprettes en instans
--------------------------------------------------

Når vi opretter en objektinstans, skal vi ofte definere, hvordan vi skal indstille dens grundlæggende tilstand, og hvilke parametre (inputdata) der er obligatoriske.

I OOP er der for at løse dette problem en særlig offentlig metode `__construct`, som vi frivilligt kan implementere, og som altid og kun kaldes, når vi opretter en instans.

Praktisk eksempel:

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
        return 'Jeg laver lyd "' . $this->sound . '"!';
    }
}
```

Ved at definere konstruktøren har vi sikret os, at når vi opretter en instans, skal vi altid sende 2 obligatoriske parametre (`name` og `sound`), og objektet vil blive sat direkte.

Da konstruktøren er en klassisk metode, kan den gøre noget andet indeni med de indsatte data. For eksempel kan du omformatere strengen korrekt, men det tager vi fat på senere.

Det er så nemt at oprette en instans igen, vi skal bare sende de indledende parametre (de er skrevet i parentes til klassens navn, hvor klassens navn kaldes og instansen oprettes):

```php
$cat = new Cat('Minda', 'Vrr');

echo $cat->name; // Der står "Minda"
echo $cat->sound; // udskriver "Vrr"

echo $cat->getFormattedSound(); // udskriver "Jeg laver en "Vrr"-lyd!
```

Gettery og settery
-----------------

Nogle metoder kaldes `getters` og `setters`. Det er almindelige metoder, og det er blot en konvention at kalde dem det. Metoder bruges til at hente og indsætte data i et objekt. Den største fordel er, at vi kan validere dataene, inden vi indsætter dem, og eventuelt rette dem eller afvise dataene og sende en fejl.

For hver egenskab, der kan hentes (vi ønsker at aktivere datahentning), er det almindeligt at oprette en getter, der starter med ordet `get`. Hvis egenskaben returnerer et boolean (`true` eller `false`), er det almindeligt at navngive begyndelsen af metodens navn med ordet `is`.

Forenklet eksempel:

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

Når vi opretter en cat-instans, overfører vi dens navn til konstruktøren (hvilket altid er obligatorisk). Men da vi ønsker at dele logikken for indsættelse af navnet for både konstruktøren og setteren (metoden til at indsætte et nyt navn), kalder vi metoden `setName()` inde i konstruktøren for at sikre, at navnet indsættes.

Inde i setteren bruger vi <a href="/function-trim">trim()</a>-funktionen, som automatisk fjerner mellemrum fra begge sider af den indsatte streng.

Vi kan bruge metoden `getName()` til output. Metoden `isEmpty()` kan bruges til at tjekke, om der er tomhed.

> **Praktiske fordele:**
>
> En stor fordel ved getters og setters er **garanterede datatyper**. Hvis en getter hævder at returnere en `streng`, kan vi altid stole på den og altid få en streng.
>
> En setter i sin implementering hævder også at acceptere en `string`, hvilket for programmet betyder, at alt, hvad brugeren indtaster, enten vil få sit input konverteret til en `string` (hvis det bruger en kompatibel type, f.eks. `integer`) eller få en fejlmeddelelse om, at typen ikke kan konverteres (f.eks. `array`).

Den største merværdi af getters og setters vil især blive værdsat i praktisk brug.

Den magiske metode `__toString()`
-----------------------------

I en klasse kan du definere en magisk metode `__toString()`, som automatisk kaldes, når du forsøger at skrive et objekt ud som en streng. Metoden skal altid returnere en streng.

Eksempel på gennemførelse:

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
        return 'Hej, jeg hedder' . $this->name . ';)';
    }
}
```

Anvendelsen er så som følger:

```php
$cat = new Cat('Minda');

echo $cat; // Der står "Hej, jeg hedder Minda ;)"
```

Resumé
-------

Vi har vist, hvordan man definerer metoder og derefter kalder dem i en objektinstans.

Næste gang ser vi på <a href="/encapsulation">encapsulation-princippet</a>, som udnytter metodernes egenskaber fuldt ud.
