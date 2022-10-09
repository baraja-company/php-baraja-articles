Usikker brug af konstanter i PHP
================================

> id: b4d0f45f-c059-4a47-9b63-8980d0d465ed
> slug:
> 	cs: nebezpecne-konstanty
> 	da: usikker-brug-af-konstanter-i-php
> 
> publicationDate: '2021-06-22 10:49:46'
> mainCategoryId: '561b0614-e152-4a62-a634-1f2d605d39d9'
> sourceContentHash: '58d4ea2e4bf106402386506b023ba4d2'

Der er to vanskelige ting, som du skal være opmærksom på, når du bruger konstanter i PHP.

Dynamiske og statiske konstanter
------------------------------

En konstant kan defineres i PHP enten statisk direkte i klassen (den bedste løsning), f.eks. på følgende måde:

```php
class Region
{
	public const PREFIX = 420;
}
```

Og anvendelsen er helt klar. Ved kompileringen af klassen er værdien af konstanten fastlagt, og vi kan få adgang til den ved at kalde klassens navn og selve konstanten. Oftest ved at skrive `Region::PREFIX`.

Den anden (langt værre måde) er at definere konstanten dynamisk ved kørselstid (oftest et sted i et konfigurationsskript), hvor det så er noget i stil med:

```php
define('BASE_DIR', __DIR__ . '/../');
```

Den største ulempe ved at definere en konstant via funktionen `define` er, at det script, der definerer konstanten, måske ikke er blevet kaldt, så konstanten eksisterer ikke, når man forsøger at læse den.

Når det kombineres med brugen af en dynamisk konstant i definitionen af en statisk konstant i en klasse, kan dette endda resultere i en fatal reflection error:

```php
class InvoiceGenerator
{
	// Det er helt forkert!
	public const DATA_DIR = BASE_DIR . '/data/faktura';
}
```

> **Forklaring:**
>
> At bruge en dynamisk konstant i en statisk konstant har den store ulempe, at værdien af den dynamiske konstant ikke kan læses på kompilationstidspunktet. Dette script skal derfor behandles igen ved hver anmodning (dvs. det kan ikke gemmes i OPCache for at optimere hastigheden), og hvis konstanten slet ikke eksisterede, opstår der en fatal kompileringsfejl, og programmet kan slet ikke køre.

Hvis du bruger PhpStan, kan det automatisk advare dig om dette problem:

```txt
Reflection error: Could not locate constant "BASE_DIR" while
evaluating expression in InvoiceGenerator at line 6
```

> **Learning:**
>
> Værdien af alle konstanter skal altid være konstant.


Arv af konstanter ved brug af statisk
-------------------------------------

I nogle tilfælde giver det mening at bruge arv til at tilsidesætte værdien af en konstant. Men i det tilfælde kan forfaderen ikke læse værdien fra efterkommeren (eller bør ikke gøre det).

Et eksempel er, når lande og regioner skal defineres:

```php
abstract class Region
{
	public function getPrefix(): int
	{
		// Fatal fejltagelse!
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

Det paradoksale er, at ovenstående kode ikke nødvendigvis giver anledning til en fejl, men at den kan opstå ved uhensigtsmæssig brug af arv.

Hvis vi kalder metoden `getPrefix()` på en efterkommer af `CzechRepublic`, vil alt være korrekt, fordi værdien af konstanten vil blive læst korrekt. Hvis den efterfølger imidlertid ikke indstillede den konstante værdi, vil der blive sendt en fejl i form af en ikkeeksisterende konstant, der ikke eksisterer. Det værste ved det hele er, at det er en **skjult afhængighed**, der oprettes i implementeringen af metoden, og at den udvikler, der arver klassen, måske ikke engang kender til denne afhængighed.

Den bedste løsning i dette tilfælde er enten at definere en konstant direkte i forfaderen med en standardværdi (så logikken altid godkendes), eller i det mindste at gøre en undtagelse i getteren.

```php
abstract class Region
{
	public const REGION = null;

	public function getPrefix(): int
	{
		if (static::REGION === null) {
			throw new \LogicException('Regionen er ikke blevet defineret.');
		}
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

PhpStan reagerer på denne fejl på følgende måde:

```txt
Access to undefined constant static(Region):REGION.
```
