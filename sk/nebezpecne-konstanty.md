Nebezpečné používanie konštánt v PHP
====================================

> id: b4d0f45f-c059-4a47-9b63-8980d0d465ed
> slug:
> 	cs: nebezpecne-konstanty
> 	sk: nebezpecne-pouzivanie-konstant-v-php
> 
> publicationDate: '2021-06-22 10:49:46'
> mainCategoryId: '561b0614-e152-4a62-a634-1f2d605d39d9'
> sourceContentHash: '58d4ea2e4bf106402386506b023ba4d2'

Pri používaní konštánt v PHP je potrebné mať na pamäti dve záludnosti.

Dynamické a statické konštanty
------------------------------

Konštantu možno v PHP definovať buď staticky priamo v triede (najlepšie riešenie), napríklad takto:

```php
trieda Region
{
	public const PREFIX = 420;
}
```

A použitie je celkom jasné. V čase kompilácie triedy je hodnota konštanty určená a môžeme k nej pristupovať volaním názvu triedy a samotnej konštanty. Najčastejšie zápisom `Region::PREFIX`.

Druhý (oveľa horší spôsob) je definovať konštantu dynamicky za behu (najčastejšie niekde v konfiguračnom skripte), kde je potom niečo ako:


```php
define('BASE_DIR', __DIR__ . '/../');
```

Hlavnou nevýhodou definovania konštanty pomocou funkcie `define` je, že skript, ktorý definuje konštantu, nemusí byť zavolaný, takže konštanta nebude existovať, keď sa ju pokúsite prečítať.

V kombinácii s použitím dynamickej konštanty v rámci definície statickej konštanty v triede to môže dokonca viesť k fatálnej chybe reflexie:

```php
trieda InvoiceGenerator
{
	// To je úplne nesprávne!
	public const DATA_DIR = BASE_DIR . '/data/invoice';
}
```

> **Vysvetlenie:**
>
> Použitie dynamickej konštanty v rámci statickej konštanty má tú hlavnú nevýhodu, že hodnotu dynamickej konštanty nemožno prečítať v čase kompilácie. Tento skript sa preto musí pri každej požiadavke spracovať znova (t. j. nemôže sa uložiť do vyrovnávacej pamäte OPCache kvôli optimalizácii rýchlosti), a ak konštanta vôbec neexistovala, vyhodí sa fatálna chyba kompilácie a aplikácia sa vôbec nemôže spustiť.

Ak používate program PhpStan, môže vás na tento problém automaticky upozorniť:

```
Nepodarilo sa nájsť konštantu "BASE_DIR" pri
vyhodnocovanie výrazu v InvoiceGenerator na riadku 6
```

> **Naučené poznatky:**
>
> Hodnota všetkých konštánt by mala byť vždy konštantná.


Dedičnosť konštánt pri použití statických
-------------------------------------

V niektorých prípadoch má zmysel použiť dedičnosť na prepísanie hodnoty konštanty. V takom prípade však predok nemôže (alebo by nemal) čítať hodnotu z potomka.

Príkladom je definovanie krajín a regiónov:

```php
abstraktná trieda Region
{
	verejná funkcia getPrefix(): int
	{
		// Fatálna chyba!
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

Paradoxne, uvedený kód nemusí nutne vyhodiť chybu, ale môže byť vyhodený nevhodným použitím dedičnosti.

Ak zavoláme metódu `getPrefix()` na potomkovi `CzechRepublic`, všetko bude správne, pretože hodnota konštanty bude načítaná správne. Ak by však potomok nenastavil hodnotu konštanty, vyhodila by sa fatálna chyba neexistujúcej konštanty. Najhoršie na celej veci je, že ide o takzvanú **skrytú závislosť**, ktorá sa vytvára v implementácii metódy, a vývojár, ktorý triedu zdedí, o tejto závislosti nemusí ani vedieť.

Najlepším riešením v tomto prípade je buď definovať konštantu priamo v predkovi s predvolenou hodnotou (aby logika vždy prešla), alebo aspoň vyhodiť výnimku v getteri.

```php
abstraktná trieda Region
{
	public const REGION = null;

	verejná funkcia getPrefix(): int
	{
		if (static::REGION === null) {
			vyhodí novú \LogicException('Región nebol definovaný.');
		}
		return static::REGION;
	}
}

final trieda CzechRepublic extends Region
{
	public const REGION = 420;
}
```

PhpStan reaguje na túto chybu takto:

```
Prístup k nedefinovanej konštante static(Region):REGION.
```
