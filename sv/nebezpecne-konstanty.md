Osäker användning av konstanter i PHP
=====================================

> id: b4d0f45f-c059-4a47-9b63-8980d0d465ed
> slug:
> 	cs: nebezpecne-konstanty
> 	sv: osaeker-anvaendning-av-konstanter-i-php
> 
> publicationDate: '2021-06-22 10:49:46'
> mainCategoryId: '561b0614-e152-4a62-a634-1f2d605d39d9'
> sourceContentHash: '58d4ea2e4bf106402386506b023ba4d2'

Det finns två knepiga saker att tänka på när du använder konstanter i PHP.

Dynamiska och statiska konstanter
------------------------------

En konstant kan definieras i PHP antingen statiskt direkt i klassen (den bästa lösningen), till exempel på följande sätt:

```php
class Region
{
	public const PREFIX = 420;
}
```

Användningen är tydlig. Vid kompilering av klassen bestäms konstantens värde och vi kan få tillgång till det genom att kalla klassens namn och själva konstanten. Oftast genom att skriva `Region::PREFIX`.

Det andra (mycket sämre sättet) är att definiera konstanten dynamiskt vid körning (oftast någonstans i ett konfigurationsskript), där den då är något i stil med:

```php
define('BASE_DIR', __DIR__ . '/../');
```

Den största nackdelen med att definiera en konstant via funktionen `define` är att skriptet som definierar konstanten kanske inte har anropats, så konstanten finns inte när du försöker läsa den.

I kombination med användningen av en dynamisk konstant i definitionen av en statisk konstant i en klass kan detta till och med resultera i ett dödligt reflektionsfel:

```php
class InvoiceGenerator
{
	// Detta är helt fel!
	public const DATA_DIR = BASE_DIR . '/data/faktura';
}
```

> **förklaring:**
>
> Att använda en dynamisk konstant inom en statisk konstant har en stor nackdel, eftersom värdet på den dynamiska konstanten inte kan läsas vid kompilationstid. Skriptet måste därför bearbetas på nytt vid varje begäran (det kan alltså inte lagras i OPCache för att optimera hastigheten), och om konstanten inte fanns alls, uppstår ett fatalt kompileringsfel och programmet kan inte köras överhuvudtaget.

Om du använder PhpStan kan det automatiskt varna dig för detta problem:

```txt
Reflection error: Could not locate constant "BASE_DIR" while
evaluating expression in InvoiceGenerator at line 6
```

> **Lärande:**
>
> Värdet på alla konstanter ska alltid vara konstant.


Arv av konstanter vid användning av statisk
-------------------------------------

I vissa fall är det vettigt att använda arv för att åsidosätta värdet av en konstant. Men i det fallet kan inte förfadern läsa värdet från efterfadern (eller bör inte göra det).

Ett exempel är när man definierar länder och regioner:

```php
abstract class Region
{
	public function getPrefix(): int
	{
		// Ett ödesdigert misstag!
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

Det paradoxala är att ovanstående kod inte nödvändigtvis ger upphov till ett fel, men det kan uppstå om man använder arv på ett felaktigt sätt.

Om vi anropar metoden `getPrefix()` på en ättling till `CzechRepublic` kommer allt att bli korrekt eftersom värdet på konstanten kommer att läsas korrekt. Om den efterkommande inte har angett värdet för konstanten, kommer dock ett felmeddelande om att konstanten inte existerar att skickas ut. Det värsta är att det är ett **gömt beroende** som skapas i implementeringen av metoden, och utvecklaren som ärver klassen kanske inte ens känner till detta beroende.

Den bästa lösningen i det här fallet är att antingen definiera en konstant direkt i anförvanten med ett standardvärde (så att logiken alltid fungerar), eller åtminstone skapa ett undantag i gettern.

```php
abstract class Region
{
	public const REGION = null;

	public function getPrefix(): int
	{
		if (static::REGION === null) {
			throw new \LogicException('Regionen har inte definierats.');
		}
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

PhpStan svarar på felet på följande sätt:

```txt
Access to undefined constant static(Region):REGION.
```
