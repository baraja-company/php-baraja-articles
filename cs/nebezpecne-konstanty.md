Nebezpečné použití konstant v PHP
=================================

> id: b4d0f45f-c059-4a47-9b63-8980d0d465ed
> slug:
> 	cs: nebezpecne-konstanty
> 
> publicationDate: "2021-06-22 10:49:46"
> mainCategoryId: "561b0614-e152-4a62-a634-1f2d605d39d9"

V PHP se při použití konstant skrývají dvě zrádnosti, které musíte mít na paměti.

Dynamická a statická konstanta
------------------------------

Konstantu lze v PHP definovat buď staticky přímo ve třídě (nejlepší řešení), třeba takto:

```php
class Region
{
	public const PREFIX = 420;
}
```

A použití je celkem jasné. V době kompilace třídy se hodnota konstanty rozhodne a můžeme k ní přistupovat voláním názvu třídy a samotné konstanty. Nejčastěji zápisem `Region::PREFIX`.

Druhý (mnohem horší způsob) je definovat konstantu dynamicky v runtime (nejčastěji někde v konfiguračním scriptu), kde je pak něco jako:


```php
define('BASE_DIR', __DIR__ . '/../');
```

Zásadní nevýhoda definice konstanty přes funkci `define` je v tom, že script, který konstantu definuje, nemusel být zavolán, takže při pokusu o čtení konstanta nebude existovat.

V kombinaci použití dynamické konstanty v rámci definice té statické ve třídě pak může dokonce vzniknout fatální chyba reflexe:

```php
class InvoiceGenerator
{
	// Toto je úplně špatně!
	public const DATA_DIR = BASE_DIR . '/data/invoice';
}
```

> **Vysvětlení:**
>
> Použití dynamické konstanty v rámci statické má zásadní nevýhodu v tom, že v době parsování scriptu (compiletime) nemůže být hodnota dynamické konstanty přečtena. Tento script se tedy musí zpracovávat v každém requestu znovu (tedy nemůže být uložen do OPCache pro optimalizaci rychlosti), a pokud konstanta vůbec neexistovala, vyhodí se fatální chyba kompilace a aplikace nepůjde vůbec spustit.

Pokud používáte PhpStan, tak na tento problém umí automaticky upozornit:

```
Reflection error: Could not locate constant "BASE_DIR" while
evaluating expression in InvoiceGenerator at line 6
```

> **Poučení:**
>
> Hodnota všech konstant by měla být vždy konstantní.


Dědičnost konstant při použití static
-------------------------------------

V některých případech dává smysl použít dědičnost pro přepsání hodnoty konstanty. V takovém případě ale nemůže předek číst hodnotu z potomka (resp. neměl by).

Příklad je například při definici zemí a regionů:

```php
abstract class Region
{
	public function getPrefix(): int
	{
		// Fatální chyba!
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

Paradoxní je, že uvedený kód chybu nutně nevyhodí, ale může být vyhozena nevhodným použitím dědičnosti.

Pokud bychom metodu `getPrefix()` volali na potomkovi `CzechRepublic`, tak bude vše správně, protože se hodnota konstanty korektně přečte. Pokud by ale potomek hodnotu konstanty nenastavil, vyhodí se fatální chyba neexistující konstanty. Nejhorší na celé věci je hlavně to, že jde o tzv. **skrytou závislost**, která vzniká v implementaci metody a vývojář, který třídu dědí, o této závislosti nemusí vůbec tušit.

Nejlepší řešení je v tomto případě definovat buď konstantu přímo v předkovi s defaultní hodnotou (aby logika vždy prošla), nebo aspoň vyhodit v getteru výjimku.

```php
abstract class Region
{
	public const REGION = null;

	public function getPrefix(): int
	{
		if (static::REGION === null) {
			throw new \LogicException('Region has not been defined.');
		}
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

PhpStan na tuto chybu reaguje takto:

```
Access to undefined constant static(Region):REGION.
```
