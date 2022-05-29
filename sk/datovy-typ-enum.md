Dátový typ Enum objekt v PHP
============================

> id: '5b96765c-16c2-4f76-8e31-6de6e9f59365'
> slug:
> 	cs: datovy-typ-enum
> 	de: type-php
>
> publicationDate: '2022-05-29 15:30:00'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: '43735e926db87d2cc6c538ddb67e0a69'

Od verzie PHP 8.1 možno dátový typ Enum použiť na definovanie presných výčtových hodnôt pre zoznam. To je užitočné v prípadoch, keď vieme, že hodnota premennej môže nadobúdať len niekoľko konkrétnych hodnôt.

Takto napríklad ukladám typy oznámení:

```php
enum OrderNotificationType: string
{
    case Email = 'e-mail';
    case Sms = 'text';
}
```

Dátový typ Enum je v PHP klasický objekt, ktorý sa správa ako špeciálny typ konštanty, ale má aj inštanciu, ktorú možno odovzdať. Na rozdiel od bežného objektu však podlieha viacerým obmedzeniam.

Rozdiely medzi enumami a objektmi
-----------------------

Hoci sú enumy postavené nad triedami a objektmi, nepodporujú všetky funkcie spojené s objektmi. Najmä enum objekty nesmú mať vnútorný stav (musia byť vždy statickou triedou).

Konkrétny zoznam rozdielov:

- Konštruktory a deštruktory sú zakázané.
- Dedičnosť nie je podporovaná. Enumy nemôžu byť rozšírené alebo zdedené inou triedou.
- Statické alebo objektové vlastnosti nie sú povolené.
- Klonovanie konkrétnych hodnôt (inštancií) enumu nie je podporované, každá jednotlivá inštancia musí byť singleton inštanciou.
- Magické metódy, s výnimkou nižšie uvedených, sú zakázané.

Nasledujúce funkcie objektu sú k dispozícii a správajú sa rovnako ako akýkoľvek iný objekt:

- Verejné, súkromné a chránené metódy.
- Verejné, súkromné a chránené statické metódy.
- Verejné, súkromné a chránené konštanty.
- Enumy môžu implementovať ľubovoľný počet rozhraní.
- Atribúty možno pripojiť k enumom a prípadom. Cieľový filter `TARGET_CLASS` obsahuje samotné enumy. Cieľový filter `TARGET_CLASS_CONST` zahŕňa prípady Enum.
- Magické metódy `__call`, `__callStatic` a `__invoke`.
- Konštanty `__CLASS__` a `__FUNCTION__` sa správajú ako bežné konštanty
- Magická konštanta `::class` na type Enum sa vyhodnotí ako úplné meno dátového typu vrátane prípadného menného priestoru, presne ako pre objekt. Magická konštanta `::class` na inštancii typu `Case` je tiež vyhodnotená ako typ Enum, pretože je inštanciou iného typu.

Používanie enumu ako dátového typu
-----------------------------

Predstavte si, že máme enum reprezentujúci typy oblekov. V tomto prípade stačí definovať typ `Suit` a uložiť jednotlivé platné hodnoty.

Príklad konkrétnej možnosti potom získame klasicky prostredníctvom štvorca, ako keď pracujeme so statickou konštantou.

Príklad definovania enumu, jeho vyvolania konkrétnym typom a odovzdania funkcii:

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

Porovnanie dvoch hodnôt
---------------------

Základnou výhodou enumov oproti objektom a konštantám je, že ich hodnoty sa dajú ľahko porovnávať.

Základné porovnanie, pri ktorom pracujeme s konkrétnou hodnotou, možno vykonať takto:

```php
$a = Suit::Spades;
$b = Suit::Spades;

$a === $b; // true
```

Veľmi často tiež potrebujeme rozhodnúť, či konkrétna hodnota patrí do platného výčtu hodnôt Enum. To sa dá ľahko overiť takto:

```php
$a = Suit::Spades;

$a instanceof Suit;  // true
```

Čítanie hodnoty typu
---------------------

Hodnotu konkrétneho typu môžeme čítať buď ako meno volajúcej konštanty, alebo priamo ako reálne definovanú hodnotu (ak existuje):

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
	echo "Farba:" . $colors->getColor();
}
```

Hodnota volajúcej konštanty sa načíta prostredníctvom vlastnosti `name`. Dôležité je aj to, že priamo v dátovom type Enum je možné implementovať vlastnú funkciu, ktorú možno volať nad každým Enumom.

Ak konkrétny enum implementuje aj reálne hodnoty (ktoré sa skrývajú pod každou konštantou), možno čítať aj ich hodnotu:

```php
enum OrderNotificationType: string
{
    case Email = 'e-mail';
    case Sms = 'text';
}

$type = OrderNotificationType::Email;

echo $type->value;
```

Všetky platné hodnoty Enum
-----------------------------

Často potrebujeme uviesť (napríklad používateľovi v chybovej správe) všetky možné hodnoty, ktoré môže enum nadobúdať. Pri použití konštánt to nebolo možné, Enum to umožňuje jednoducho:

```php
Suit::cases();
```

Vracia `[Suit::Hearts, Suit::Diamonds, Suit::Clubs, Suit::Spades]`.

Overte, či je premenná typu Enum
---------------------------------

To, že konkrétna neznáma premenná obsahuje Enum, môžeme ľahko overiť pomocou podmienky:

```php
if ($haystack instanceof \BackedEnum) {
```

Každý objekt Enum je automaticky potomkom všeobecného rozhrania `\BackedEnum`.

Viac informácií nájdete v diskusii na GitHube PhpStan: https://github.com/phpstan/phpstan/issues/7304
