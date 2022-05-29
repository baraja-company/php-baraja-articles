Datatyp Enum-objekt i PHP
=========================

> id: '5b96765c-16c2-4f76-8e31-6de6e9f59365'
> slug:
> 	cs: datovy-typ-enum
> 	de: type-php
>
> publicationDate: '2022-05-29 15:30:00'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: '43735e926db87d2cc6c538ddb67e0a69'

Från och med PHP 8.1 kan datatypen Enum användas för att definiera exakta uppräkningsvärden för en lista. Detta är användbart när vi vet att värdet på en variabel endast kan anta ett visst antal värden.

Så här lagrar jag till exempel anmälningstyper:

```php
enum OrderNotificationType: string
{
    case Email = 'e-post';
    case Sms = 'text';
}
```

I PHP är datatypen Enum ett klassiskt objekt som fungerar som en speciell typ av konstant, men som också har en instans som kan skickas vidare. Till skillnad från ett vanligt objekt omfattas det dock av ett antal begränsningar.

Skillnader mellan Enum och objekt
-----------------------

Även om enums är byggda ovanpå klasser och objekt har de inte stöd för all objektrelaterad funktionalitet. Enumobjekt får inte ha interna tillstånd (de måste alltid vara en statisk klass).

En särskild förteckning över skillnader:

- Konstruktorer och destruktorer är förbjudna.
- Arv stöds inte. Enums kan inte utökas eller ärvas av en annan klass.
- Statiska egenskaper eller objektegenskaper är inte tillåtna.
- Kloning av specifika Enum-värden (instanser) stöds inte, varje enskild instans måste vara en singletoninstans.
- Magiska metoder är förbjudna, med undantag för vad som anges nedan.

Följande objektfunktioner är tillgängliga och beter sig precis som alla andra objekt:

- Offentliga, privata och skyddade metoder.
- Offentliga, privata och skyddade statiska metoder.
- Offentliga, privata och skyddade konstanter.
- Enum kan implementera hur många gränssnitt som helst.
- Attribut kan knytas till enum och fall. Målfiltret `TARGET_CLASS` inkluderar själva enumerna. Målfiltret `TARGET_CLASS_CONST` omfattar Enum-fall.
- Magiska metoder `__call`, `__callStatic` och `__invoke`.
- Konstanterna `__CLASS__` och `__FUNCTION__` beter sig som vanliga konstanter.
- Den magiska konstanten `::class` på Enum-typen utvärderas som det fullständiga namnet på datatypen, inklusive alla namnområden, precis som för ett objekt. Den magiska konstanten `::class` på en instans av typen `Case` utvärderas också som typ Enum, eftersom det är en instans av en annan typ.

Använda Enum som datatyp
-----------------------------

Föreställ dig att vi har en enum som representerar olika typer av kostymer. I det här fallet behöver vi bara definiera typen `Suit` och lagra de enskilda giltiga värdena.

Vi får sedan en instans av det särskilda alternativet på ett klassiskt sätt via en kvadratisk formel, som när vi arbetar med en statisk konstant.

Exempel på hur man definierar en Enum, anropar den med en specifik typ och överlämnar den till en funktion:

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

Jämförelse av två värden
---------------------

Den grundläggande fördelen med enums jämfört med objekt och konstanter är att det är lätt att jämföra deras värden.

Den grundläggande jämförelsen att vi arbetar med ett specifikt värde kan göras på följande sätt:

```php
$a = Suit::Spades;
$b = Suit::Spades;

$a === $b; // sant
```

Mycket ofta måste vi också bestämma att ett visst värde hör till en giltig Enum-värdesuppräkning. Detta kan enkelt verifieras på följande sätt:

```php
$a = Suit::Spades;

$a instanceof Suit;  // sant
```

Läsa värdet av typen
---------------------

Vi kan läsa ett specifikt typvärde antingen som ett namn på en anropande konstant eller direkt som ett verkligt definierat värde (om det finns):

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
	echo "Färg:" . $colors->getColor();
}
```

Värdet på den anropande konstanten läses via egenskapen `name`. Det är också viktigt att en anpassad funktion kan implementeras direkt i datatypen Enum, som kan anropas över varje Enum.

Om en viss Enum också implementerar verkliga värden (som är dolda under varje konstant) kan deras värde också läsas:

```php
enum OrderNotificationType: string
{
    case Email = 'e-post';
    case Sms = 'text';
}

$type = OrderNotificationType::Email;

echo $type->value;
```

Alla giltiga Enum-värden
-----------------------------

Ofta behöver vi lista (till exempel för användaren i ett felmeddelande) alla möjliga värden som Enum kan ha. När du använde konstanter var detta inte möjligt, men Enum gör det enkelt:

```php
Suit::cases();
```

Återger `[Suit::Hearts, Suit::Diamonds, Suit::Clubs, Suit::Spades]`.

Kontrollera att variabeln är av typen Enum
---------------------------------

Vi kan enkelt verifiera att en viss okänd variabel innehåller en Enum med hjälp av ett villkor:

```php
if ($haystack instanceof \BackedEnum) {
```

Varje Enum-objekt är automatiskt ett barn av det generiska gränssnittet `\BackedEnum`.

För mer information, se diskussionen på PhpStan GitHub: https://github.com/phpstan/phpstan/issues/7304
