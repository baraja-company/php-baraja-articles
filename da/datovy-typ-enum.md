Datatype Enum-objekt i PHP
==========================

> id: '5b96765c-16c2-4f76-8e31-6de6e9f59365'
> slug:
> 	cs: datovy-typ-enum
> 	da: datatype-enum-objekt-i-php
> 
> publicationDate: '2022-05-29 15:30:00'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: '43735e926db87d2cc6c538ddb67e0a69'

Fra og med PHP 8.1 kan datatypen Enum bruges til at definere nøjagtige enumerationsværdier for en liste. Dette er nyttigt i tilfælde, hvor vi ved, at værdien af en variabel kun kan antage nogle få bestemte værdier.

Jeg gemmer f.eks. notifikationstyper på denne måde:

```php
enum OrderNotificationType: string
{
    case Email = 'e-mail';
    case Sms = 'tekst';
}
```

I PHP er Enum-datatypen et klassisk objekt, der opfører sig som en særlig type konstant, men som også har en instans, der kan videregives. I modsætning til et almindeligt objekt er det dog underlagt en række begrænsninger.

Forskelle mellem Enum og objekter
-----------------------

Selv om enums er bygget oven på klasser og objekter, understøtter de ikke alle de funktioner, der er forbundet med objekter. Det er især forbudt for enum-objekter at have intern tilstand (de skal altid være en statisk klasse).

En specifik liste over forskelle:

- Konstruktorer og destruktorer er forbudt.
- Arv er ikke understøttet. Enums kan ikke udvides eller arves af en anden klasse.
- Statiske egenskaber eller objektegenskaber er ikke tilladt.
- Kloning af specifikke Enum-værdier (instanser) understøttes ikke, hver enkelt instans skal være en singleton-instans.
- Magiske metoder er forbudt, undtagen som anført nedenfor.

Følgende objektfunktioner er tilgængelige og opfører sig som alle andre objekter:

- Offentlige, private og beskyttede metoder.
- Offentlige, private og beskyttede statiske metoder.
- Offentlige, private og beskyttede konstanter.
- Enums kan implementere et vilkårligt antal grænseflader.
- Attributter kan knyttes til enums og cases. Målfilteret `TARGET_CLASS` omfatter selve enummene. Målfilteret `TARGET_CLASS_CONST` omfatter Enum-tilfælde.
- Magiske metoder `__call`, `__callStatic` og `__invoke`.
- Konstanterne `__CLASS____` og `__FUNCTION__` opfører sig som normale konstanter
- Den magiske konstant `::class` på Enum-typen evalueres som det fulde navn på datatypen, inklusive et eventuelt namespace, præcis som for et objekt. Den magiske konstant `::class` på en instans af typen `Case` evalueres også som typen Enum, da det er en instans af en anden type.

Brug af Enum som datatype
-----------------------------

Forestil dig, at vi har et enum, der repræsenterer typerne af jakkesæt. I dette tilfælde skal vi blot definere typen `Suit` og gemme de enkelte gyldige værdier.

Vi får derefter en instans af den særlige mulighed på klassisk vis via en kvadratisk, som når vi arbejder med en statisk konstant.

Eksempel på at definere et Enum, påkalde det ved en bestemt type og sende det til en funktion:

```php
enum Suit
{
	case Hearts;
	case Diamonds;
	case Clubs;
	case Spades;
}

function doStuff(Suit $s)
{
	// ...
}

doStuff(Suit::Spades);
```

Sammenligning af to værdier
---------------------

Den grundlæggende fordel ved enums i forhold til objekter og konstanter er, at det er nemt at sammenligne deres værdier.

Den grundlæggende sammenligning, hvor vi arbejder med en specifik værdi, kan foretages på følgende måde:

```php
$a = Suit::Spades;
$b = Suit::Spades;

$a === $b; // sandt
```

Meget ofte skal vi også beslutte, at en bestemt værdi hører til i en gyldig Enum-værdiopregning. Dette kan let verificeres på følgende måde:

```php
$a = Suit::Spades;

$a instanceof Suit;  // sandt
```

Læsning af værdien af typen
---------------------

Vi kan læse en specifik typeværdi enten som et navn på en kaldende konstant eller direkte som en reelt defineret værdi (hvis den findes):

```php
enum Colors
{
	case Red;
	case Blue;
	case Green;

	public function getColor(): string
	{
	    return $this->name;
	}
}

function paintColor(Colors $colors): void
{
	echo "Maling:" . $colors->getColor();
}
```

Værdien af den kaldende konstant læses via egenskaben `name`. Det er også vigtigt, at der kan implementeres en brugerdefineret funktion direkte i Enum-datatypen, som kan kaldes over hvert Enum.

Hvis et bestemt Enum også implementerer reelle værdier (som er skjult under hver konstant), kan deres værdi også læses:

```php
enum OrderNotificationType: string
{
    case Email = 'e-mail';
    case Sms = 'tekst';
}

$type = OrderNotificationType::Email;

echo $type->value;
```

Alle gyldige Enum-værdier
-----------------------------

Ofte er vi nødt til at opstille en liste (f.eks. til brugeren i en fejlmeddelelse) over alle mulige værdier, som Enum kan antage. Når du brugte konstanter, var det ikke muligt, men Enum gør det nemt:

```php
Suit::cases();
```

Returnerer `[Suit::Hearts, Suit::Diamonds, Suit::Clubs, Suit::Spades]`.

Kontroller, at variablen er af typen Enum
---------------------------------

Vi kan nemt kontrollere, at en bestemt ukendt variabel indeholder et Enum ved hjælp af en betingelse:

```php
if ($haystack instanceof \BackedEnum) {
```

Hvert Enum-objekt er automatisk et barn af den generiske `\BackedEnum`-grænseflade.

Du kan finde flere oplysninger i diskussionen på PhpStan GitHub: https://github.com/phpstan/phpstan/issues/7304
