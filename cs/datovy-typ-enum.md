Datový typ Enum objekt v PHP
============================

> id: "5b96765c-16c2-4f76-8e31-6de6e9f59365"
> slug:
> 	cs: datovy-typ-enum
>
> publicationDate: "2022-05-29 15:30:00"
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9

Od PHP 8.1 lze použít datový typ Enum, který slouží pro definici přesných výčtových hodnot seznamu. Hodí se to pro případy, kdy víme, že hodnota proměnné může nabývat jen konkrétních několika hodnot.

Například takto ukládám typy notifikací:

```php
enum OrderNotificationType: string
{
    case Email = 'email';
    case Sms = 'sms';
}
```

Datový typ Enum je v PHP klasický objekt, který se chová jako speciální typ konstanty, ale navíc má instanci, kterou můžeme předávat dál. Na rozdíl od běžného objektu se na něj ale vztahuje řada omezení.

Rozdíly Enumu a objektů
-----------------------

Přestože jsou enumy postaveny na třídách a objektech, nepodporují všechny funkce související s objekty. Zejména je zakázáno, aby objekty enumů měly vnitřní stav (vždy musí jít o statickou třídu).

Konkrétní výčet rozdílů:

- Zakázány jsou konstruktory a destruktory.
- Dědičnost není podporována. Enumy nelze rozšiřovat ani dědit jinou třídou.
- Statické nebo objektové vlastnosti nejsou povoleny.
- Klonování konkrétních hodnot (případů) Enum není podporováno, každý jednotlivý případ musí být singletonové instance.
- Magické metody, s výjimkou níže uvedených, jsou zakázány.

Následující funkce objektu jsou k dispozici a chovají se stejně jako u jakéhokoli jiného objektu:

- Veřejné, soukromé a chráněné metody.
- Veřejné, soukromé a chráněné statické metody.
- Veřejné, soukromé a chráněné konstanty.
- Enumy mohou implementovat libovolný počet rozhraní.
- K enumům a případům mohou být připojeny atributy. Cílový filtr `TARGET_CLASS` zahrnuje samotné Enumy. Cílový filtr `TARGET_CLASS_CONST` zahrnuje případy Enum.
- Magické metody `__call`, `__callStatic` a `__invoke`.
- Konstanty `__CLASS__` a `__FUNCTION__` se chovají jako normální konstanty
- Magická konstanta `::class` na typu Enum se vyhodnocuje jako celý název datového typu včetně případného jmenného prostoru, přesně stejně jako u objektu. Magická konstanta `::class` na instanci typu `Case` se také vyhodnocuje jako typ Enum, protože se jedná o instanci jiného typu.

Použití Enumu jako datový typ
-----------------------------

Představte si, že máme enum reprezentující druhy obleků. V takovém případě stačí definovat typ `Suit` a uložit jednotlivé validní hodnoty.

Instanci konkrétní možnosti pak získáme klasicky přes čtyřtečku, jako při práci se statickou konstantou.

Ukázka definici Enumu, jeho vyvolání podle konkrétního typu a předání do funkce:

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

Porovnání dvou hodnot
---------------------

Zásadní výhoda enumu oproti objektům a konstantám je ta, že lze snadno porovnávat jejich hodnotu.

Základní porovnání, že pracujeme s konkrétní hodnotou můžeme udělat takto:

```php
$a = Suit::Spades;
$b = Suit::Spades;

$a === $b; // true
```

Velmi často také potřebujeme rozhodnout, že konkrétní hodnota patří do validního výčtu hodnot Enumu. To lze ověřit snadno takto:

```php
$a = Suit::Spades;

$a instanceof Suit;  // true
```

Přečtení hodnoty typu
---------------------

Konkrétní hodnotu typu můžeme přečíst buď jako název volací konstanty, nebo přímo reálnou definovanu hodnotu (pokud existuje):

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
	echo "Paint: " . $colors->getColor();
}
```

Hodnota volací konstanty se čte přes property `name`. Důležité je také to, že přímo do datového typu Enum lze implementovat vlastní funkci, kterou můžeme nad každým Enumem zavolat.

Pokud konkrétní Enum implementuje také reálné hodnoty (které se skrývají pod každou konstantou), lze přečíst i jejich hodnotu:

```php
enum OrderNotificationType: string
{
    case Email = 'email';
    case Sms = 'sms';
}

$type = OrderNotificationType::Email;

echo $type->value;
```

Všechny validní hodnoty Enumu
-----------------------------

Často potřebujeme (například uživateli v chybové hlášce) vypsat všechny možné hodnoty, kterých může Enum nabývat. Při použítí konstant toto možné nebylo, Enum to umožňuje snadno:

```php
Suit::cases();
```

Vrátí: `[Suit::Hearts, Suit::Diamonds, Suit::Clubs, Suit::Spades]`.

Ověření, že je proměnná typu Enum
---------------------------------

Že konkrétní neznámá proměnná obsahuje Enum můžeme snadno ověřit podmínkou:

```php
if ($haystack instanceof \BackedEnum) {
```

Každý Enum objekt je automaticky potomek obecného rozhraní `\BackedEnum`.

Další informace jsou v diskusi na GitHubu PhpStan: https://github.com/phpstan/phpstan/issues/7304
